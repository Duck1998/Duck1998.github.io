---
layout: post
title: "记一次 BUG 排除经历"
subtitle: "临时解决 BDAc v1.2.2.2 组件消失的 BUG"
date: 2018-10-18
author: "Duck"
header-img: "img/posts/bda-bug-workaround-00.jpg"
tags: 
    - KSP
    - BDAc
    - BUG
---

> 题图：Steam 集换式卡牌《Kerbal Recruit》

## 起因
自上个月以来，坎巴拉太空计划吧的提问楼偶尔会出现类似「为什么我的 BDA 只显示这些组件」这种提问，也就是下图中这种情况。  
![image](/img/posts/bda-bug-workaround-01.jpg)

提问者得到的回答一般是「按制造商筛选就能找到全部 MOD 组件」。这样做确实能解决问题，于是我也没有深究下去，毕竟我个人并不使用这个 MOD。
直到前几天，又有位在提问楼提出了同样的问题。在得到同样的回答后，那位并不满意并表示已经解决了这个 BUG ——这引起了我的好奇心。

**那就来动手分析并解决 BUG 吧！**

## 复现 BUG
游戏版本 **1.4.5** ，语言为**简体中文**

1. 下载安装 [BDAc v1.2.2.2][1] 与其依赖的 [PRE v1.7.0][2]
2. 打开游戏进入 VAB / SPH ，选择 **BDA Weapons** 分类，只能显示少量组件
3. 改为**按制造商筛选**，选择**巴哈姆特动力**分类，能够找到所有 BDA 相关组件

## 分析 BUG
BDAc 最近一次更新于8月8日，距离现在已有超过两个月时间，但如此严重的 BUG 却一直没有得到解决。因此这个 BUG 很可能只出现在中文玩家中，因为依赖于汉化的玩家可能缺乏用英文直接向开发者反馈足够信息的能力，这样开发者便难以及时发现并解决 BUG 。

结合刚才复现的情况，综上判断：

1. BDAc 所有组件已正确加载
2. 问题可能出现在汉化过程中
3. 问题可能出现在 BDA 组件分类中

既然这个 BUG 自上个月以来就已经多次在贴吧被人提出，那么其 GitHub 上大概率会有相关的 Issue——果不其然， [#580][3] 便反映了这个 BUG 。
维护者提供的解决方法是删除 `\GameData\BDArmory\Localization` 下的 `localization-zh-cn.cfg` ，但这样所有文本都将会丢失，经确认此方法有效。  
所以问题出在这文件的哪里呢？

使用 VS Code 打开此文件，首先引起我注意的是右下角显示的 **UTF-8 with BOM** ，这种编码方式在某些场合可能会带来兼容性问题。尝试以标准的 **UTF-8** 编码保存文件，但打开游戏后发现问题依旧存在，所以问题并不出在此文件的编码方式上。

那可能是最近一次更新意外搞乱了这个文件？查阅汉化文本文件相关的 [Commit][4] 也没有发现任何可疑处。

毫无头绪，那就暂时忘记 `localization-zh-cn.cfg` ，把注意力集中在组件分类上。原版 KSP 中组件分类的实现依靠于组件的 cfg 配置文件中的 `category` 参数——例如当参数的值为 `Pods` 时该组件将会被分类到**座舱**下。  
既然这样那随便找一个组件的配置文件看看吧，这里挑选的是 *AIM-120 导弹* ：
```
    category = none
    subcategory = 0
    title = AIM-120 AMRAAM Missile
```
不对啊，如果分类是 `none` 的话按理说英文用户也会看不到这个组件才对？在同文件夹下打开另一组件的配置文件：
```
    category = none
    subcategory = 0
    title = AMRAAM EMP Missile
```
似乎没什么区别。但仔细一看——这个名为 *AMRAAM EMP Missile* 的导弹却正常出现在 BDA 组件分类中！  
![image](/img/posts/bda-bug-workaround-02.jpg)

鉴于 BDAc 并没有使用第三方分类 MOD（例如 [CCK][5]）进行组件分类，所以 BDAc 一定有自己的分类功能模块，那为什么这个 BUG 在以前并没有出现呢？
在 Release 页面查看最新版本的更新日志，这两条记录十分关键：
> Added 2 new EMP equipped missiles HellFire EMP, and AIM-120 EMP. Small pulse radius but demonstrates the feature.

刚才的 *AMRAAM EMP Missile* 便是最新添加的组件，它能正常出现在 BDA Weapons 分类中。

> Removed BDACategoryModule as a result of the BDA categories refactor.

这意味着 BDA 分类功能模块刚刚更新过，那就来研究一下它吧。  
以 *catagory* 为关键词在 GitHub 仓库中搜索源代码文件，果然找到了 `BDAEditorCategory.cs` ，这一段映入眼帘：
```
    protected string Manufacturer
    {
        get { return "Bahamuto Dynamics"; }
        set { }
    }
```
灵光一现，马上回到 `localization-zh-cn.cfg` 中，找到了这一条汉化文本：
```
#loc_BDArmory_part_manufacturer = 巴哈姆特动力
```
**豁然开朗！** 分类功能的判定原来是 **制造商名称** 等于 **Bahamuto Dynamics** 。

现在让我们把所有的线索串起来，整理出这个 BUG 的来龙去脉：
- 两个月前 BDAc 更新 v1.2.2.2 版本
    - 新版分类模块的筛选**只考虑到英文用户**
    - 所有**新加入的组件**都没有汉化
- 由于旧组件的**制造商名称**被汉化，因此所有旧组件都无法被新版分类模块筛选出来
    - **新加入的组件** 完全没有汉化，因此不受影响
- 中文玩家更新 MOD 后发现了此 BUG，开始在不同地方提问寻求解决方案
    - 问题配图中所有能正常显示的组件均为**新加入的组件**

## 解决 BUG
对于 MOD 用户而言，最好的临时解决方法是修改 `\GameData\BDArmory\Localization` 下的 `localization-zh-cn.cfg` 第 9 行：
```
#loc_BDArmory_part_manufacturer = Bahamuto Dynamics
```
而如果要绝后患，则需要将 `BDAEditorCategory.cs` 中涉及字符串判断的部分改为特殊的本地化字符串，以应对不同的本地化环境。
这明显超出了我的能力范围，所以我提交了这个 [Pull Request][6] 作为临时方案，同时指出了问题的根源所在。  
高兴的是 BDAc 项目的一位协作者很快审阅并肯定了这个修改，并表示将修复 BDA 组件分类模块中的问题。
> Good catch @Duck1998 ! :)  
  I guess we should really fix our Category module to use the localized string, but this is a good workaround until someone gets to it. :)

---

**2018/11/27 更新**  
BDAc已于10天前 [更新][7] ，修复了这个问题。  
v1.2.3: Thanks to SpannerMoneky, Gedas-S, DoctorDavinci, Kergan, Gomker, PapaJoesSoup, TheDogKSP, Duck1998, and Fitiales for their work on this release!  
- FIXES
  - Fixed weapons category due to localization issues. #580  

---

最后附上问题解决后显示正常的截图  
![image](/img/posts/bda-bug-workaround-03.jpg)

## 总结
作为和 KSP MOD 打了四年多交道的老油条，经过这次排 BUG 的经历，个人认为这些帮到了我：
- 过硬的英文**阅读**能力
- 细致的调查和推理
- 熟悉 GitHub 基本操作
- 理解游戏配置文件中的大部分参数含义
- 一点好运气

那为什么这个 BUG 存在了两个月也没有被找到原因？  
可能在 KSP 中文游戏圈里，只有我无聊到给一个自己并不玩的 MOD 排除 BUG 而且还要把整个过程写在博客上了吧。

[1]:https://github.com/PapaJoesSoup/BDArmory/releases/tag/v1.2.2.2
[2]:https://github.com/jrodrigv/PhysicsRangeExtender/releases/tag/1.7.0
[3]:https://github.com/PapaJoesSoup/BDArmory/issues/580
[4]:https://github.com/PapaJoesSoup/BDArmory/commit/0312aee3841bf3953e29ea4499bf9b06b04e6e4d#diff-4d82c2810389739e75bf7c291ecbaad3
[5]:https://forum.kerbalspaceprogram.com/index.php?/topic/149840-discussion-community-category-kit/
[6]:https://github.com/PapaJoesSoup/BDArmory/pull/585
[7]:https://github.com/PapaJoesSoup/BDArmory/releases/tag/v1.2.3.0
