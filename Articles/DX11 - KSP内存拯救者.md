![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/Banner.png)
> *请问您今天要来点Oops吗？*

## 调用DX11以大幅降低KSP内存占用

### 前言
**《坎巴拉太空计划》** 自从诞生之初，玩家们就一直在与高内存占用造成的游戏崩溃不断斗争。游戏崩溃可能发生在任何时候，它毫不留情，简单利落，只留下冰冷的崩溃对话框与屏幕前沮丧的玩家。

游戏制作组和玩家并没有被击倒，他们以各自的努力改善游戏体验：
- KSP 1.1版，游戏引擎升级到Unity 5，64位客户端终于稳定可用
- **Active Texture Management** (MOD)可以主动压缩贴图以节约内存使用
- **Texture Replacer** (MOD)可以主动卸载未使用贴图以节约内存使用
- 玩家主动调用**OpenGL**降低内存使用，但游戏性能可能降低

如今在制作组的不断努力下，游戏日趋稳定，玩家可以尽情享受稳定的64位客户端带来的大内存福利。

> 居安思危，能否在64位客户端无限制内存使用的基础上同时节省内存使用呢？

**答案是肯定的，只需调用DX11。**

### 效果展示
#### KSP 1.3.1 原版未装MOD 最高画质
![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/Stock_Menu.jpg)
![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/Stock_Size.png)

默认使用**DX9**时，内存占用**2293MB**  
![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/Stock_DX9.png)

指定使用**DX11**时，内存占用**1211MB**，降低**47.2%**  
![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/Stock_DX11.png)

*不够刺激？那我们来点认真的！*
#### KSP 1.3.1 安装大量MOD 最高画质
![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/Mod_Menu.jpg)
![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/Mod_Size.png)

默认使用**DX9**时，内存占用**6887MB**  
![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/Mod_DX9.png)

指定使用**DX11**时，内存占用**4161MB**，降低**39.6%**  
![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/Mod_DX11.png)

### 使用前提
1. 电脑配备了支持DX11的**独立**显卡，显存建议至少为1GB
   > 若不清楚是否支持DX11，搜索“显卡型号+DX11”  
   > Intel核芯显卡与AMD APU未测试，**不保证效果**
2. 操作系统为Windows Vista或以上，其中Vista需要安装DX11升级补丁

### 使用方法
#### 如果使用Steam启动
1. 在“库”中右键单击游戏，打开“属性”窗口  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/Steam_1.png)
2. 点击“设置启动选项”，在文本框中输入以下内容并确定：  
   *注意没有空格*  
   ```
   -force-d3d11
   ```
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/Steam_2.png)
3. 使用Steam启动时即可调用DX11，选择“Launch KSP (64-bit)”以使用64位客户端  
   *如使用32位操作系统，则保持默认*  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/Steam_3.png)  
   **注意：仅当在游戏页面点击“开始”才会出现此菜单，否则均默认启动32位客户端**

#### 如果不使用Steam启动
1. 游戏安装目录下找到“KSP_x64.exe”，创建一个快捷方式。这里以创建桌面快捷方式为例  
   *如使用32位操作系统，则对应“KSP.exe”*  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/NonSteam_1.png)
2. 右键单击刚刚创建的快捷方式，打开“属性”窗口  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/NonSteam_2.png)
3. 找到“目标”文本框，从最后添加以下内容并确定：  
   *注意开头的空格*  
   ```
    -force-d3d11
   ```
   此处的完整内容例子：  
   ```
   "D:\SteamLibrary\steamapps\common\Kerbal Space Program\KSP_x64.exe" -force-d3d11
   ```
   一些情况下可能没有引号，例如：
   ```
   D:\KSP\KSP1310\KSP_x64.exe -force-d3d11
   ```
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/NonSteam_3.png)  

4. 使用该快捷方式启动即可调用DX11

### 进阶操作
#### 确认游戏成功调用DX11
- 启动游戏后，打开游戏安装目录，找到KSP.log  
  如果有“Direct3D 11.0”字样，则成功调用DX11
  ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/Log.png)
- ~~观察游戏的内存占用变化，明显减少则成功调用DX11~~（废话）

#### 强制Steam默认启动64位客户端同时调用DX11
> **意义何在？**
> - 懒，不想每次启动都手动选择64位客户端
> - 不使用Steam启动就无法积累Steam游戏时长，也不能使用Steam游戏内覆盖功能

1. 在游戏属性中，打开“本地文件”选项卡，点击“浏览本地文件”  
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/Steam_Adv1.png)
2. 点击箭头所指位置，获得游戏安装路径，全选并复制。例如这里是：  
   ```
   D:\SteamLibrary\steamapps\common\Kerbal Space Program
   ```
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/Steam_Adv2.png)
3. 在游戏属性中，打开“常规”选项卡，点击“设置启动选项”，在文本框中输入以下内容并确定：  
   *注意英文引号与空格*  
   ```
   "刚才复制的地址\KSP_x64.exe" %command% -force-d3d11
   ```
   此处的例子：
   ```
   "D:\SteamLibrary\steamapps\common\Kerbal Space Program\KSP_x64.exe" %command% -force-d3d11
   ```
   ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/Steam_Adv3.png)
4. 无论从Steam创建的桌面快捷方式、开始菜单快捷方式、Steam程序内、Steam图标右键菜单启动KSP，都会启动64位客户端同时调用DX11

### 兼容性问题/BUG
- **原版游戏目前无任何问题**
- 少数美化类MOD**可能**不兼容或存在BUG
- [**Kronal Vessel Viewer**](https://forum.kerbalspaceprogram.com/index.php?/topic/152430-link) (MOD)在DX11下无法正常使用
> **如果你认为确实遇到了兼容性问题/BUG：**  
> 取消或删除上文提到的所有修改，使用默认的DX9进入游戏，检查兼容性问题/BUG是否继续存在  
> 如果问题/BUG消失，则可以确定是DX11带来的问题  
> 欢迎提供相关反馈

### FAQ
- Q：为什么调用DX11可以降低内存占用？  
  A：个人猜测在DX11下贴图会直接载入独立显存中  
  > 例子：KSP 1.3.1 安装大量MOD 最高画质

  使用**DX9**时，独立显存占用**0.6GB**  
  ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/DX9_GPU.png)

  使用**DX11**时，独立显存占用**2.7GB**  
  ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/DX11_GPU.png)
- Q：为什么调用OpenGL也可以降低内存使用，**但游戏性能可能降低**？  
  A：根据本人曾经于1.1.3版本做的测试[《DX9、DX11和OpenGL同平台下简单性能对比》](https://tieba.baidu.com/p/4917015866)  
  节省内存的原理与DX11相同，性能较低可能为驱动优化不足或OpenGL自身效率低下。
- Q：为什么以前只听说过调用OpenGL？  
  A：因为在KSP 1.1版本前，也就是使用Unity 5引擎前，调用DX11会造成影响体验的BUG以及MOD兼容问题，而OpenGL并不会。 
- Q：为什么OpenGL不会遇到问题？  
  A：个人猜测因为Mac版和Linux版都只能使用OpenGL，故开发游戏时已经解决了问题，且MOD开发时一般也考虑了OpenGL兼容性。  
- Q：如何调用OpenGL？  
  A：将此文章中的“d3d11”替换为“opengl”即可。
- Q：能不能调用DX10/DX12/Vulkan？  
  A：不能，游戏不支持。

### 后记
居然真的写完了，这么长，这么详细，现在还觉得这一切都有点不可思议，我是在做梦吗？  
作为一个涉足视频制作的KSP老玩家，我渐渐认识到自己制作的载具不如别人，游戏技术也不如不少老玩家，视频制作也力不从心只能遥望知名UP主。  
我必须承认这一点——在一些方面，我还不够努力，也有太多人比我优秀。  
我不甘心。  
我想用自己的方式在这个圈子里留下属于我的痕迹。  

以一己之力在贴吧重新建立[MOD大楼](https://tieba.baidu.com/p/5219531488)后，我找到了自己最擅长的方面——  
收集、整理、发布和帮助他人。  
于是就有了Rabbit House这个迷你博客，就有了这第一篇文章。  
我的痕迹，自此开始。

### 附录
- 测试平台：  
  未来人类 P775DM2-G 准系统
  > Core i7-6700K  
  > GTX1070  
  > 2x 8GB DDR4-3000  
  > 三星SM951 128GB  
  > 三星PM871a 1TB
- 图片制作与编辑软件：
  > 画图  
  > PowerPoint 2016  
  > Image Tuner
- 题图无水印无遮挡版  
  **这不是截图**  
  ![image](https://github.com/Duck1998/Duck1998.github.io/raw/master/Assets/DX11%20-%20KSP%E5%86%85%E5%AD%98%E6%8B%AF%E6%95%91%E8%80%85/Oops.png)
  *发布于公有领域，无限制自由使用*
