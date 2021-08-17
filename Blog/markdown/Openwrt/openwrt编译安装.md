---
title: openwrt 编译安装
date: 2017-01-10 23:12:43
tags:
- Openwrt
- 笔记
categories: 
- openwrt
---
# <center>openwrt 编译安装</center> 

---
### 编译硬件环境要求（官方推荐）
编译一个可以安装的OpenWrt固件镜像文件（大约8MB大小的），你需要：
1. 一个纯净的OpenWrt编译系统大约需要200MB的空间。
2. 一个包含feeds的OpenWrt编译系统大约需要300MB的空间。
3. 编译feeds中的软件包大约需要2.1GB的空间用于存放下载来的源代码
4. 构建OpenWrt并生成固件文件需要大约3-4 GB的空间。
5. 编译OpenWrt需要大约1-4 GB的内存。

###### 编译工具安装
教程系统环境 ubuntu 16.04,其他系统需做相应调整。
1. 安装git下载源代码

```
sudo apt-get update
sudo apt-get install git-core build-essential libssl-dev libncurses5-dev unzip
```

2. 部分feeds包，仅发布在SVN 上，so也需安装SVN


```
sudo apt-get install subversion mercurial
```
###### notice：
1. 请使用一个非root用户来完成这些工作！
2. 这里的所有命令都在OpenWrt编译系统的根目录下运行（例如~/openwrt/trunk/）
3. 编译系统的绝对路径中不能含有空格！
4. 如果你使用root用户下载了源码，请把你下载来的源码的所有者更改为一个非root用户。(sudo chown -R user:user /openwrt/)
1. 关于编译环境的详细信息请参见：[make](http://man.cx/make)和build-essential
2. 关于git的详细信息请参见：git(7)
关于subversion tool的详细信息请参见：svn和subversion documentation (multiple languages)

###### 下载源码
1. 通过git来下载OpenWrt bleeding edge(trunk版本)：(参见[Downloading Sources](https://wiki.openwrt.org/zh-cn/doc/howto/buildroot.exigence#downloading_sources) 以获得更多选择):

```
git clone git://git.openwrt.org/openwrt.git
```
这将会创建'openwrt'这个目录。这个目录将会是OpenWrt的编译主目录。
OpenWrt的交叉编译工具链也已经被包含在内。

2. (可选)下载并安装所有可用的"feeds"(参见OpenWrt Feeds以获取更多选择):

```
cd openwrt
./scripts/feeds update -a
./scripts/feeds install -a
```

3. 运行下面的命令(**3选1**!)让OpenWrt编译系统检查你的编译环境中缺失的软件包:

```
make menuconfig  (推荐使用此命令)
或者
make defconfig
或者
make prereq
```
//如果以上3个命令都运行了,编译会出错! 


```
make -j1 V=s
```
 -j 编译核心数 ，V=s 输出详细信息
 
//编译这个过程需要较长时间，且需下载文件有可能中途报错 

编译完成后

openwrt/bin/cpu平台/  
有相应的包和ipk 文件

- ## 编译安装nodejs

```
make menuconfig
```
- 选择编程语言

![image](https://note.youdao.com/yws/api/personal/file/WEB635da5c7b120f029b2426e896806d3db?method=getImage&version=3141&cstk=0PLGrt4w)

- 选择nodejs
 
- ![image](
https://note.youdao.com/yws/api/personal/file/WEB5fb737063b388ebb012807c03f6b6f11?method=getImage&version=3151&cstk=0PLGrt4w)

- 选择nodejs需要安装模块

  路径： > Languages > Node.js

- ## 多核，大内存版本编译
```
make kernel_menuconfig

```


![image](https://note.youdao.com/yws/api/personal/file/WEB5b4db45502397d5d9e8d3b8b0b33f2dd?method=getImage&version=3165&cstk=0PLGrt4w)

参数说明
```



Processor type and features  --->
    [*] Symmetric multi-processing support
    Processor family (Core 2/newer Xeon)  --->#自行选择处理器平台
    [*] Supported processor vendors  --->#自行选择处理器平台
    (2) Maximum number of CPUs #自行编辑
    [*] SMT (Hyperthreading) scheduler support#超线程支持
    [*] Multi-core scheduler support 
    High Memory Support (4GB)  --->

#############################################

           [*] Symmetric multi-processing support                                 
          -*- Processor feature human-readable names                             
          [ ] Support for big SMP systems with more than 8 CPUs                  
          [ ] Support for extended (non-PC) x86 platforms                        
          < > Intel SoC IOSF Sideband support for SoC platforms                  
          < > Eurobraille/Iris poweroff module                                   
          [*] Single-depth WCHAN output                                          
          [ ] Linux guest support  ----                                          
          [ ] Memtest                                                            
              Processor family (Intel Atom)  --->     处理器平台                           
          [*] Generic x86 support                                                
          [*] Supported processor vendors  --->                                  
          [*] HPET Timer Support                                                 
          [ ] Enable DMI scanning                                                
          (4) Maximum number of CPUs   --->最大cpu数                                          
          [*] SMT (Hyperthreading) scheduler support                             
          [*] Multi-core scheduler support                                       
              Preemption Model (No Forced Preemption (Server))  --->             
          [*] Reroute for broken boot IRQs                                       
          [*] Machine Check / overheating reporting                              
          [*]   Intel MCE features                                               
          [*]   AMD MCE features                                                 
          [ ]   Support for old Pentium 5 / WinChip machine checks               
          < > Machine check injector support                                     
          [*] Enable VM86 support                                                
          [ ] Enable support for 16-bit segments                                 
          < > Toshiba Laptop support                                             
          < > Dell laptop support                                                
          [ ] Enable X86 board specific fixups for reboot                        
          < > CPU microcode loading support                                      
          <*> /dev/cpu/*/msr - Model-specific register support                   
          <*> /dev/cpu/*/cpuid - CPU information support                         
              High Memory Support (4GB)  --->    配置最大内存（最大64G）                               
              Memory split (3G/1G user/kernel split)  --->                       
              Memory model (Flat Memory)  --->                                   
          [ ] Allow for memory compaction                                        
          [*] Enable bounce buffers                                              
          [ ] Enable KSM for page merging                                        
          (4096) Low address space to protect from user allocation               
          [ ] Enable recovery from hardware memory errors                        
          [ ] Transparent Hugepage Support                                       
          [ ] Enable cleancache driver to cache clean pages if tmem is present   
          [ ] Enable frontswap to cache swap pages if tmem is present            
          [ ] Contiguous Memory Allocator                                        
          < > Common API for compressed memory storage                           
          < > Low density storage for compressed pages                           
          < > Memory allocator for compressed pages                              
          [ ] Allocate 3rd-level pagetables from highmem                         
          [ ] Check for low memory corruption  


```
