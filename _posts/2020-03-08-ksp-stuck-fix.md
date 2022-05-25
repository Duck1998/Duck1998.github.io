---
layout: article
title: "原版 KSP 无法启动的可能解决方案"
tags: 坎巴拉太空计划 FAQ
---
「禁止拍屏，信息不足无法解答」
<!--more-->

本文为 [坎巴拉太空计划 FAQ](/archive.html?tag=FAQ) 的一部分。
{:.info}

## 本文不适用范围
1. 哀嚎「为什么游戏打不开」**但不提供任何有效信息**
2. 安装了任何 MOD，请自行检查并解决兼容性问题
#### 有效信息的例子
- 游戏版本
- 有无安装 MOD
  - 安装了什么版本的什么 MOD
  - /GameData 截图
- 电脑配置，包括 CPU、内存、显卡
- 操作系统版本

## 不支持 32 位操作系统
### 问题表现
使用 Steam 下载并启动游戏得到以下错误信息

更新 Kerbal Space Program 时出现错误（缺失可执行文件）:  
Steam 库目录\Kerbal Space Program\KSP.app
{:.error}

### 原因与解决方法
自 2018/10/15 发布的 1.5 版本起，游戏不再提供 32 位客户端，Steam 无法处理这种情况。

最后提供 32 位客户端的版本为 **1.4.5**，使用 Steam 游戏属性的「测试」选项卡可回滚至此版本。  
![image](/images/ksp-stuck-fix-01.webp)

尽管可以打开游戏，但出于游戏体验的考虑，建议安装 64 位操作系统并配备 8GB 或更大的内存。

## 可用内存不足
### 问题表现
游戏加载中报错退出，可以观察到极高的内存占用。  
![image](/images/ksp-stuck-fix-02.webp)

### 原因与解决方法
游戏推荐配备 **8GB** 或更大的内存与独立显卡。  
安装了所有 DLC 的 1.9.1 版本，带独显的配置在最高设置下占用 3-4GB 内存。
#### 使用启动器降低贴图设置
1. 打开游戏根目录下的「Launcher.exe」
2. 降低下图的「Texture Quality」设置，点击「Accept」确认

![image](/images/ksp-stuck-fix-03.webp)

#### 1.8 版本前使用 DX11 启动游戏
配备独立显卡时使用 DX11 启动游戏可缓解内存压力，但可能存在兼容性问题。
##### 通过 Steam 启动
![image](/images/ksp-stuck-fix-04.webp)

##### 通过快捷方式启动
![image](/images/ksp-stuck-fix-05.webp)

##### 解决编辑器中部件贴图丢失
安装对应游戏版本的 [Textures Unlimited](https://forum.kerbalspaceprogram.com/index.php?/topic/167450-TU) 和 [Module Manager](https://forum.kerbalspaceprogram.com/index.php?/topic/50533-MM)。

## 检测不到时区设置
### 问题表现
游戏启动时卡在黑色背景与极短的进度条。  
![image](/images/ksp-stuck-fix-06.webp)

`%USERPROFILE%\AppData\LocalLow\Squad\Kerbal Space Program\Player.log` 中可搜索到以下关键词
> TimeZoneNotFoundException

### 原因与解决方法
原因未知，发生在 **1.8** 版本后。
1. 将时区修改至任意设置
2. 尝试打开游戏
3. 将时区修改回原来的设置

### 参考链接
[Loading stuck issue after 1.8](https://forum.kerbalspaceprogram.com/index.php?/topic/190110-d)

## Unity 引擎游戏的显卡缩放 BUG
### 问题表现
游戏完全黑屏，但除画面外可能正常运行。
也可能会得到以下错误信息
> Couldn't switch to requested monitor resolution

`%USERPROFILE%\AppData\LocalLow\Squad\Kerbal Space Program\output_log.txt` 中可搜索到以下关键词
> IndexOutOfRangeException

### 原因与解决方法
可能为 Win10 + Unity 引擎的 BUG。
#### 同时使用 Intel 核显与 A/N 独显的笔记本
1. 打开「英特尔®显卡控制面板」或「英特尔®显卡控制中心」
2. 找到「显示」或「显示器」页面
3. 将「缩放」或「扩展」选项修改为「无缩放」

#### 只使用独显的台式机/极少数笔记本
与上文同理，找到对应的显卡控制面板修改缩放选项。

### 参考链接
[Unity games won't run on Windows 10 1709](https://steamcommunity.com/discussions/forum/1/1480982971174752598)

## 附录
![image](/images/ksp-stuck-fix-07.webp)

[本文B站专栏版本](https://www.bilibili.com/read/cv5013785){:.button.button--secondary.button--rounded}
