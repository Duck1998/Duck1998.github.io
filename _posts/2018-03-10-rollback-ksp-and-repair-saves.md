---
layout: post
title: "坎巴拉版本回滚计划"
subtitle: "如何回滚 Steam 版 KSP 并修复存档"
date: 2018-03-10
author: "Duck"
tags:
    - KSP
    - 历史文章
---

![title](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/Title.jpg)

## 前言
本文发布不久前，**《坎巴拉太空计划》**更新了 **1.4.0** 版本，这使得绝大部分 MOD 需要作者更新才能继续使用。  
与此同时，Steam 默认开启的游戏自动更新功能“摧毁”了部分 MOD 玩家的游戏体验：  
- 自动更新后开启游戏，游戏直接崩溃
- 游戏启动过程中提示 MOD 不兼容当前版本
- 游戏存档受到一定程度影响

> 本教程适用于使用 Steam 启动 KSP 的玩家

## 回滚 KSP 至旧版本
1. 在“库”中右键单击游戏，打开“**属性**”窗口  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/01.PNG)
2. 在游戏属性中，打开“**测试**”选项卡，点击“**请选择你想要参与的测试**”下拉框，选择需要回滚至的版本
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/02.PNG)
3. 如果需要更新至最新版本，步骤2中选择“**无-不选择任何测试活动**”即可

### 勘误
B站up主[@M31的夜空](https://space.bilibili.com/2996571/)曾表示该选项将使 KSP 回滚至“预发布版”并导致未知的 BUG 。本人验证回滚后并**没有**发生文件变化，与 1.3.1 版本文件**完全一致**。  
选项中的 “previous” 指的是“过去”，而非 “pre-release” 的“预发布”。

## 阻止 Steam 未经确认自动更新 KSP
在游戏属性中，打开“**更新**”选项卡，点击“**自动更新**”下拉栏，选择“**只在我启动时更新游戏**”  
![image](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/03.PNG)
### 意义何在？
- 如果没有选择任何“测试”选项，版本更新时 Steam 将会自动下载新版本
- 如果选择了该项，版本更新时仅需在“测试”选项卡选择当前版本即可无需回滚，直接保留在当前版本

例子：
> 1.3.1 版本时，选择“只在我启动时更新游戏”，此时 1.4.0 版本尚未更新  
> 1.4.0 版本发布后，Steam 提示 KSP 更新下载，但未开始  
> “测试”选项选择 “previous_1.3.1 - 1.3.1 Kerbal Space Program”  
> 此时 KSP 更新下载自动消失，可继续使用 Steam 启动 1.3.1 版本

## 修复旧版本无法打开的存档
如果你在更新游戏**打开过**先前版本的存档，再**回滚至先前版本**后，就会发现游戏提示存档无法打开  
这原本是个善意的功能，防止高版本存档在低版本使用造成 BUG ，但如果你确实需要打开它呢？  
![image](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/04.jpg)
> **救救存档！**

1. 在游戏属性中，打开“**本地文件**”选项卡，点击“**浏览本地文件**”  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/05.PNG)
2. 打开 “**saves**” 文件夹，这里负责存放游戏存档  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/06.PNG)
3. 打开你需要修复的存档文件夹，例如这里存档名为 “**Duck1998**”  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/07.PNG)
4. 右键单击 “**persistent.sfs**” ，选择“**打开方式**”  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/08.PNG)
5. 使用“**记事本**”打开，不必勾选“始终使用此应用打开 .sfs文件”  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/09.PNG)
6. 将**第三行**的 “version = ” 后的版本号改为你需要的版本号，**保存文件**
   > 例子：1.4.0 改为 1.3.1  

   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/10.PNG)
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/11.PNG)
7. 按 “Ctrl+F” 打开**查找**功能，输入 “version = ” 并搜索，将版本号改为你需要的版本号，**保存文件**  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/14.PNG)
8. 回到**存档目录**，打开 “**Ships**” 文件夹，这里负责存放载具存档  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/15.PNG)
9. 找到你的载具存档，这里以 SPH 为例，存档文件格式为 “**.craft**”  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/16.PNG)
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/17.PNG)
10. 以同样的方法用记事本打开文件，将版本号改为你需要的版本号，**保存文件**  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/18.PNG)
11. 旧存档修复完成，尽情享受吧  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/12.jpg)
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/deprecated/Assets/%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9ASteam%E7%89%88KSP%E5%B9%B6%E4%BF%AE%E5%A4%8D%E5%AD%98%E6%A1%A3/13.jpg)