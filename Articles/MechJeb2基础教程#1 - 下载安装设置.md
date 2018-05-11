![banner](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/banner.jpg)  
> *经过多年研究，我们的科学家依然无法解释为什么Jebediah Kerman是位如此优秀的飞行员。因此我们决定制作一份他大脑的机械拷贝，以协助驾驶我们的飞船。*

## MechJeb2基础教程#1 - 下载/安装/设置
### 简介
#### MechJeb2是什么
1. 多功能信息显示工具
2. 多功能自动驾驶
3. 简称“MJ”或“MJ2”

#### MechJeb2的功能
> 截图来自2.7.3汉化版  

![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/01.jpg)
#### 推荐理由
MJ是笔者在入门时期接触的第一个MOD，因此印象最为深刻。它能帮助玩家在火箭建造和轨道机动中少走弯路，在熟悉游戏后可用于执行重复枯燥的操作，在航天器交汇、对接、定点降落等场景中提供十分高的精确度。此MOD一定程度上降低了游戏的入门门槛，同时也可以为更加复杂的航天计划奠定基础。

### 下载和安装
#### 汉化版
> 由于汉化版由第三方（[贴吧@孤生苦世](http://tieba.baidu.com/home/main?un=%E5%AD%A4%E7%94%9F%E8%8B%A6%E4%B8%96)）管理，此部分内容**不保证永久准确有效**  
1. 打开[MechJeb2汉化更新发布贴](https://tieba.baidu.com/p/2928105246)，找到[下载地址](http://pan.baidu.com/s/1sjHhgHB)并打开（密码：k5nc）  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/02.png)
2. **首先阅读“MechJeb2汉化说明.txt”，下载对应版本**  
   本教程以目前最新的“MechJeb2-2.7.3-releases-zh_CN-180404.zip”为例，其中2.7.3版实际支持KSP 1.4.x版，因此可用于目前最新的KSP 1.4.3  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/03.png)  
   压缩包内文件：  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/04.png)
3. 将“MechJeb2”文件夹和“ModuleManager.3.0.6.dll”复制到**游戏根目录下的“GameData”文件夹**内
   > Steam版快速找到游戏根目录：  
     在游戏属性中，打开“**本地文件**”选项卡，点击“**浏览本地文件**”  
     ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/05.PNG)  
 
    ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/05.png)  
    复制完成后“GameData”文件夹内应是这样，大功告成！  
    ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/06.png)

#### 原版（英文）
> 此部分内容默认你具有足够的英语阅读理解能力，这里只提供必要的说明和链接  

原版由MechJeb维护者[@sarbian](https://forum.kerbalspaceprogram.com/index.php?/profile/57146-sarbian/)直接管理，更新速度快
- 位于官方论坛的[发布帖链接](https://forum.kerbalspaceprogram.com/index.php?/topic/154834-d)  
  ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/07.jpg)
- 功能与汉化版整合的“指令舱/座椅内置MJ脚本”接近的[指令舱内置MJ脚本](http://forum.kerbalspaceprogram.com/index.php?/topic/88726-d)  

### 确认安装成功
MJ功能菜单出现的判定条件为**当前航天器的任意组件中存在MechJeb模块**，但这个模块并不存在于原版游戏中  
如果**只安装原版MechJeb2**，则需要在航天器上安装MJ提供的“AR202盒子”组件，因为这个组件含有MechJeb模块  
![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/08.jpg)  
而汉化版MechJeb2整合的“指令舱/座椅内置MJ脚本”给游戏中所有指令舱与座椅都添加了MechJeb模块  
因此只需在VAB/SPH中**创建一个指令舱**，右下方的应用启动器便会出现“MJ”按钮，点击后即可调出MJ功能菜单  
![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/09.jpg)   
![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/10.jpg)

### 基础设置详解
在MJ功能菜单中打开设置  
**右键按住MechJeb菜单按钮**以拖动功能菜单  
![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/11.jpg)  
![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/12.jpg)  
- 恢复默认设置：顾名思义
- 使用xxx皮肤：  
  “1号皮肤”为早期版本风格，现已无法使用；“紧凑皮肤”缩小了行距，一定程度上节省了屏幕空间占用；“2号皮肤”为默认启用
- 皮肤比例：  
  MJ界面全局缩放比例
- 用箭头选择器替代下拉菜单：见下图示例  
  开启前：  
  ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/13.jpg)  
  开启后：  
  ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/14.jpg)
- 隐藏探测器驾驶面板里的“驾驶员被弹出时制动”：顾名思义  
  ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/15.jpg)
- 只有标题栏用于拖动窗口：顾名思义
- 使用app导航图标/隐藏侧边菜单按钮：  
  需要注意的是“app导航图标”是右侧红圈  
  ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/16.jpg)
- [顶部] [-] 2 [+]：  
  分别调整MJ功能菜单的位置与显示列数，见下图示例  
  ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/17.jpg)
- RO/RSS特殊处理(WIP)：  
  在使用“自动发射”功能时，如果手动停止自动发射，将不会自动关闭引擎
- 加速时激活SAS：  
  “加速”指MJ自动进行的时间加速

### 进阶操作
#### 启用隐藏的MJ无人指令舱（题图的玩意）
> 警告：后果自负，不保证实际使用效果！  

1. 使用记事本等软件打开**游戏根目录\GameData\MechJeb2\Parts\MechJeb2_Pod**文件夹中的**part.cfg**  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/18.png)
2. 找到**category = none**，修改为**category = Pods**，保存  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/19.png)
3. 在“座舱”分类中即可找到它  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/20.jpg)

#### 使用Toolbar (MOD)快捷按钮调用MJ功能
> 此部分内容默认你具有足够的英语阅读理解能力，这里只提供必要的说明和链接  
> Toolbar本身自带良好的简体中文翻译，故此处不做额外的设置介绍

[Toolbar](https://forum.kerbalspaceprogram.com/index.php?/topic/161857-d)可供第三方MOD显示功能按钮，而且具备一定的自定义功能。  
![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/21.jpg)  
Toolbar可实现的效果预览：  
![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/22.jpg)  
![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/23.jpg)

#### 使用开发版（英文）
> 此部分内容默认你具有足够的英语阅读理解能力，这里只提供必要的说明和链接  

[开发版](https://ksp.sarbian.com/jenkins/job/MechJeb2-Dev/)包含所有开发中的新功能/BUG修复。当KSP版本更新或正式版MJ存在严重BUG时，开发版中的新功能/BUG修复将加入正式版并作为全新的正式版发布。由于开发版添加了新的功能，因此可能存在比正式版更多的BUG。  
![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/MechJeb2%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B%231/24.jpg)

### 后记
感谢阅读《MechJeb2基础教程》系列第1篇：下载/安装/设置  
本期从最基础开始，接下来每期将专注于其中一个功能，通过专栏图文与视频解说结合，尽我所能完整详细地介绍此功能的使用方法。  
下一期将带来《MechJeb2基础教程》系列第2篇：自动发射

### 附录
图片制作与编辑软件：
> 画图  
> Image Tuner
