---
layout: article
title: "蓝天砖头本 Ubuntu 19.10 历险记"
tags: Ubuntu 记录
modify_date: 2020-08-14
---
只要搜索不滑坡，方法总比困难多。
<!--more-->

同样适用于 Ubuntu 20.04 LTS。
{:.info}

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

## 安装准备
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
我完全知晓 Secure Boot 对于系统安全的意义，但为了避免驱动问题决定关闭。

1. Win10 下按住 Shift 键重启进入 WinRE
2. 疑难解答 → 高级选项 → UEFI 固件设置
3. 找到并关闭 Secure Boot

也可从设置 → 更新和安全 → 恢复 → 高级启动进入 WinRE。
{:.info}

### U 盘引导启动
从上一步的 UEFI 菜单选择 U 盘作为引导设备进入安装程序。

1. 由于显卡兼容问题，选择「Install Ubuntu (safe graphics)」
2. 通过 WLAN 联网
3. 选择「最小安装」与「为图形……安装第三方软件」
4. 选择「安装 Ubuntu，与 Windows Boot Manager 共存」
   - GRUB 自动安装在 SM951 的 EFI 分区
   - 系统自动安装在 32GB 空闲空间中
5. 等待安装与更新完成，重启

## 首次启动
### 解决无限循环登陆界面
[Login loop with fresh 19.10 install](https://askubuntu.com/a/1185667)

显卡驱动问题导致，需要修改 GRUB 配置以延迟加载显卡驱动。

1. 使用 `Ctrl + Alt + F3` 进入 TTY 并登陆账户
2. 打开 GRUB 配置
   ```console
   $ sudo nano /etc/default/grub
   ```
3. 修改内容，保存
   ```
   GRUB_CMDLINE_LINUX_DEFAULT="nomodeset"
   ```
4. 更新 GRUB 并重启
   ```console
   $ sudo update-grub
   $ sudo reboot
   ```

## 设置系统
### 更改软件源
1. 软件和更新 → 下载自 → 其他站点
2. 选择阿里源 `mirrors.aliyun.com`
3. 回校后修改为学校镜像站以节省网费开销

### 显示器缩放
设置 → 设备 → 显示器 → 缩放 200%

### 应用屏幕校色
1. 校色文件存储在 `C:\Windows\system32\spool\drivers\color`，复制到可挂载位置
2. 设置 → 设备 → 色彩

### 临时解决网卡软死锁问题
[19.10 big issue with intel 9260ac wifi: iwlwifi brings system to a halt
](https://askubuntu.com/a/1188002)

AC 9260 驱动与 Linux 5.3 内核不兼容导致，本硬件的解决方法为**启动 Linux 前关闭电脑并移除电源一段时间再插电启动**。

以下为第二种临时解决方法，无需移除电源的麻烦但大大牺牲了网络速度，仅供参考。

1. 从 [英特尔® 无线适配器的 Linux* 支持](https://www.intel.cn/content/www/cn/zh/support/articles/000005511/network-and-i-o/wireless-networking.html) 下载并安装 AC 9260 最新 Linux 驱动
2. 创建网卡设置
   ```console
   $ sudo nano /etc/modprobe.d/iwlwifi.conf
   ```
3. 写入内容，保存
   ```
   options iwlwifi power_save=0 11n_disable=1
   ```

这将禁用 802.11n/ac，5Ghz 仅能使用802.11a，速度受到极大限制。

### 解决时区冲突
[怎样解决Windows10时间快和Ubuntu时间差问题？](https://www.zhihu.com/question/46525639/answer/157272414)

禁用 Ubuntu 的 UTC
```console
$ timedatectl set-local-rtc 1 --adjust-system-clock
```
记得在 Win10 重新同步时钟。

## 提升体验
### 修改 GRUB 设置
#### 默认启动上次的系统
[How To Configure GRUB2 Boot Loader Settings In Ubuntu](https://www.ostechnix.com/configure-grub-2-boot-loader-settings-ubuntu-16-04/)

双系统下相当实用。

1. 打开 GRUB 配置
   ```console
   $ sudo nano /etc/default/grub
   ```
2. 修改与添加内容，保存
   ```
   GRUB_DEFAULT=saved
   GRUB_SAVEDEFAULT=true
   ```
3. 更新 GRUB
   ```console
   $ sudo update-grub
   ```

#### 降低分辨率
[How do I safely change grub2 screen resolution?](https://askubuntu.com/a/54068)

4K 分辨率下的 GRUB 速度非常慢也没有自动缩放。

1. GRUB 菜单下按 `C` 键进入命令行
2. 列出支持的显示模式，发现只有 `1280x720` 最合适
   ```console
   $ videoinfo
   ```
3. 进入系统后打开 GRUB 配置
   ```console
   $ sudo nano /etc/default/grub
   ```
4. 修改内容，保存
   ```
   GRUB_GFXMODE=1280x720
   ```
5. 更新 GRUB
   ```console
   $ sudo update-grub
   ```

#### 黑色背景
[How to change the purple background color in grub?](https://askubuntu.com/a/82223)

直男黑万岁！

1. 打开 GRUB 主题配置
   ```console
   $ sudo -H nano /usr/share/plymouth/themes/default.grub
   ```
2. 修改 RGBA 数值，保存
   ```
   if background_color 0,0,0,0 ; then
   ```
3. 更新 GRUB
   ```console
   $ sudo update-grub
   ```

### 安装键盘灯控制模块
[tuxedocomputers/tuxedo-keyboard](https://github.com/tuxedocomputers/tuxedo-keyboard)

TUXEDO 公司开源的蓝天笔记本键盘灯控制模块，快捷键与 Win 下相同。

1. 按照「The DKMS route」编译安装模块
2. 设置开机启动
   ```console
   $ sudo echo tuxedo_keyboard >> /etc/modules
   ```
3. 创建默认关灯与低亮度的配置文件
   ```console
   $ sudo echo "options tuxedo_keyboard state=0 brightness=10" > /etc/modprobe.d/tuxedo_keyboard.conf
   ```

### 临时解决默认最大亮度问题
[Brightness is reset to maximum on every restart](https://askubuntu.com/a/359644)

由于 [延迟加载显卡驱动](#解决无限循环登陆界面) 导致无法使用常见方法解决，以下为临时解决方法。

1. 安装 `xbacklight`
   ```console
   $ sudo apt install xbacklight
   ```
2. Ubuntu「启动应用程序」中添加条目
   ```
   xbacklight -set 25
   ```

尽管如此，开机后亮度滑块依然停留在 100%，第一次调整亮度不应使用快捷键。

### 安装常用软件
#### apt
```console
$ sudo apt install synaptic psensor
```
#### deb
[Google Chrome](https://www.google.cn/chrome/)

[Visual Studio Code](https://code.visualstudio.com/)

[VNC Viewer](https://www.realvnc.com/en/connect/download/viewer/)

#### Deepin Wine
[Deepin wine for Ubuntu and Debian](https://github.com/wszqkzqk/deepin-wine-ubuntu)

TIM 勉强能用，比 Linux QQ 靠谱。

打开配置修改 DPI 以缩放应用
```console
$ WINEPREFIX=~/.deepinwine/Deepin-TIM deepin-wine winecfg
```

### 个性化
#### GNOME 个性化
[通过gnome-tweaks工具……看起来像Windows](https://www.jianshu.com/p/33a75d16f384)

1. 安装 GNOME Tweaks 和 Dash to panel 扩展
   ```console
   $ sudo apt install gnome-tweaks gnome-shell-extensions-dash-to-panel
   ```
2. `Alt + F2` 输入 `r` 重启 GNOME
3. GNOME Tweaks 中启用 Dash to panel 扩展

#### 安装字体
1. 字体存储在 `C:\Windows\Fonts` ，复制到可挂载位置
2. GNOME Tweaks 中设置系统字体与渲染选项
3. 终端单独设置终端字体

## 总结
经过两天的折腾后，终于得到了可日常使用的 Ubuntu 19.10 ~

事实证明，只要搜索不滑坡，方法总比困难多。
