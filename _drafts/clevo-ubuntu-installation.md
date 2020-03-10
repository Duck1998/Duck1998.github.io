---
layout: article
title: "蓝天砖头本 Ubuntu 19.10 历险记"
tags: Ubuntu 记录
---
只要搜索不滑坡，方法总比困难多。
<!--more-->

## 环境
### 硬件
截取自[《消费性电子产品变迁记录》](/2019/07/02/consumer-electronics-log.html)
{:.info}

未来人类 P775TM1-G 准系统
> Intel Core i9-9900K  
> NVIDIA GeForce RTX 2080  
> 2 × 8GB DDR4-3000  
> 三星 SM951 128GB  
> 三星 PM871a 1TB

- 屏幕分辨率为 4K，已于 Win 下校色
- 键盘灯为三区 RGB，通过 Fn 快捷键控制
- 无线网卡为 Intel AC 9260 马甲 Killer 1550
- 物理屏蔽核芯显卡，接近于台式机而非传统的游戏本

### 软件
- Win10 以 **UEFI+GPT** 方式安装在 SM951
- PM871a 将划分 32GB 安装 Ubuntu 19.10
- UEFI 菜单允许开关 Secure Boot，默认开启

## 安装前
### 划分磁盘空间
使用 [DiskGenius](http://www.diskgenius.cn/) 的「调整分区大小」功能在 PM871a 末尾划分 32GB 空闲空间。

由于涉及部分系统文件，DiskGenius 确认更改后自动进入 WinPE 完成操作。

也可使用系统自带磁盘管理的「压缩卷」功能。
{:.info}

### 制作启动 U 盘
使用 [Rufus](https://rufus.ie/zh_CN.html) 加载 Ubuntu 安装镜像，「分区类型」必须选择 **GPT** 否则无法引导。

这次使用的是又爱又恨的铁板烧 CZ73 64GB U盘，制作完毕后无需移除原地待命。

### 准备第二台 PC
Surface Go 在一旁待命，随时准备谷歌遇到的问题，必要时也可制作救援 U 盘。

## 安装过程
### 关闭 Secure Boot
我完全知晓 Secure Boot 对于系统安全的意义，但为了避免相关疑难杂症决定关闭。
1. Win10 下按住 Shift 键重启进入 WinRE
2. 疑难解答 → 高级选项 → UEFI 固件设置
3. 找到并关闭 Secure Boot

也可从设置 → 更新和安全 → 恢复 → 高级启动进入WinRE。
{:.info}

### U 盘引导启动
从上一步的 UEFI 菜单选择 U 盘作为引导设备进入安装程序。
1. 由于显卡兼容问题，选择「Install Ubuntu (safe graphics)」
2. 通过 WLAN 联网
3. 选择「最小安装」与「为图形……安装第三方软件」
4. 选择「安装 Ubuntu，与 Windows Boot Manager 共存」
   1. GRUB 2 自动安装在 SM951 的 EFI 分区
   2. 系统自动安装在 32GB 空闲空间中
5. 等待安装与更新完成，重启

## 首次启动
### 解决无限循环登陆界面

### 解决时区冲突

## 设置系统
### 更改软件源

### 临时解决网卡软死锁问题

## 提升体验
### 修改 GRUB 分辨率

### 安装常用软件

### 个性化美化

## 总结
