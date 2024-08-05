---
title: Minecraft 生电服务器搭建指南
date: 2024-08-01 17:00:00
tags: [Minecraft,Java,Redstone,server,我的世界,红石,服务器,生电,国际版,Fabric]
cover: https://cdn.jsdelivr.net/gh/Zachery-Liu/blog-comments@main/pics/connor-gan-EJ-9oQx8ABQ-unsplash.jpg
banner: https://cdn.jsdelivr.net/gh/Zachery-Liu/blog-comments@main/pics/connor-gan-EJ-9oQx8ABQ-unsplash.jpg
topic: tutorials
---

应群友要求，特写此篇生电服的搭建教程，

本篇教程详细介绍了生电服务器从零开始的架设过程。
当然，本篇教程也可作为 Fabric 模组服的搭建教程阅读。

<!-- more -->

## 前期准备

### 服务器系统的选择

个人建议新手选择 **Windows Server** 或者 **Ubuntu** 系统。

- 服务器配置足够好就可以选择 Windows Server，可以为后期维护节省大量时间，唯一缺点就是占用较高，挤占了 Minecraft 的可用资源。

- 服务器配置不够好可以选择 Ubuntu，资料足够多，足够好用，动手搜索一下大部分问题都可以解决，占用低。

>**🚨不推荐新手使用其他系统**

### 服务端软件的选择

在 Minecraft 中，生电服务器是一种特殊的服务器，介乎原版与模组服之间，基本与原版类似但有不同。

理论上可以直接将原版服务端作为生电服务端进行使用。但是在实务中，由于原版服务端低下的性能以及少之又少的功能，一般都会使用其他服务端。

「生电」一般指「生存」与「红石电路」的结合。因此，架设生电服务器的一个重要考虑因素就是原版的特性的保留。

然而，常见的 ```Bukkit```、```Spigot``` 以及 ```Paper``` 之类的服务端都修改了或者删除了原版的部分特性。

因此，常见的生电服务器大多使用 [Fabric](https://fabricmc.net/) 作为服务端。并由 [Fallen_Breath](https://fallenbreath.me/) 开发的 [MCDreforged](https://mcdreforged.com/) 框架作为功能的补充。

>MCDReforged（以下简称 MCDR）是一个可以在完全不对 Minecraft 服务端进行修改的情况下，通过可自定义的插件系统，提供对服务端的管理能力的工具
>小至计算器、高亮玩家、b 站弹幕姬，大至操控计分板、管理结构文件、自助备份回档，都可以通过 MCDR 及相配套的插件实现 ————摘自 [MCDreforged README](https://github.com/MCDReforged/MCDReforged/blob/master/README_cn.md)

### 服务器必需环境的安装

#### 安装 Java

不同版本的 Minecraft 需求不同版本的 Java。

可以查阅[软件需求](https://zh.minecraft.wiki/w/Java%E7%89%88#%E8%BD%AF%E4%BB%B6%E9%9C%80%E6%B1%82)页面来查询需求的 Java 版本。

根据 Minecraft 的实际需求，本文推荐使用 LibericaJRE 的 Full Version，且下文将使用 LibericaJRE Full Version 进行讨论。

##### Windows 下的安装

打开 [Liberica 下载页面](https://bell-sw.com/pages/downloads/#jdk-21-lts)，在菜单栏选择对应 JAVA 版本，向下翻找到如图部分的 Windows 下载区域，在 ```Package``` 中选择 ```Full JRE```，后单击 ```MSI``` 下载安装程序即可。

![Windows Java download](https://cdn.jsdelivr.net/gh/Zachery-Liu/blog-comments@main/pics/mcfabric/1.png)

下载后打开安装程序，点击 ```Liberica JRE Full``` 选项旁边的图标，然后选择 ```Entire feature will be installed on local hard drive```，按需修改安装位置后一直点击下一步即可。

![Windows Java install](https://cdn.jsdelivr.net/gh/Zachery-Liu/blog-comments@main/pics/mcfabric/2.png)

##### Linux 下的安装

使用具有 su 权限的账户登录至服务器，然后根据对应系统进行操作。

###### 使用 APT 包管理器的系统（如 Ubuntu、Debian 等）

1. 设置 APT 库

```sh
wget -q -O - https://download.bell-sw.com/pki/GPG-KEY-bellsoft | sudo apt-key add -
echo "deb [arch=amd64] https://apt.bell-sw.com/ stable main" | sudo tee /etc/apt/sources.list.d/bellsoft.list
```

其他架构的用户需要修改命令中 ```[arch=X]``` 中的 X 部分。

可供修改的选项有 ```amd64```, ```i386```, ```arm64```, ```armhf```。

命令行出现 ```OK``` 即为完成。

2. 安装 JAVA

```sh
sudo apt-get update
sudo apt-get install bellsoft-java<VNUM>-runtime-full
```

其中 ```<VNUM>``` 需要替换为 Java 版本。可供修改的目前有 8、11、12、13、14、15、16、17、18、19、20、21、22。跟随 JAVA 的更新可供安装的版本也会变化。

例如安装 Java 22 需要使用命令

```sh
sudo apt-get update
sudo apt-get install bellsoft-java22-runtime-full
```

可以使用以下指令查看安装情况，返回 java 版本则安装成功。

```sh
java --version
```

一种可能的返回：

```sh
openjdk 22.0.2 2024-07-16
OpenJDK Runtime Environment (build 22.0.2+11)
OpenJDK 64-Bit Server VM (build 22.0.2+11, mixed mode, sharing)
```

###### 使用 YUM 包管理器的系统（如 CentOS 等）

1. 设置 YUM 库

```sh
echo | sudo tee /etc/yum.repos.d/bellsoft.repo > /dev/null << EOF
[BellSoft]
name=BellSoft Repository
baseurl=https://yum.bell-sw.com
enabled=1
gpgcheck=1
gpgkey=https://download.bell-sw.com/pki/GPG-KEY-bellsoft
priority=1
EOF
```

此库包含所有架构设备所需的包，不需修改。

2. 安装 JAVA

```sh
sudo yum update
sudo yum install bellsoft-java<VNUM>-runtime-full
```

其中 ```<VNUM>``` 需要替换为 Java 版本。可供修改的目前有 8、11、12、13、14、15、16、17、18、19、20、21、22。跟随 JAVA 的更新可供安装的版本也会变化。

例如安装 Java 22 需要使用命令

```sh
sudo yum update
sudo yum install bellsoft-java22-runtime-full
```

可以使用以下指令查看安装情况，返回 java 版本则安装成功。

```sh
java --version
```

一种可能的返回：

```sh
openjdk 22.0.2 2024-07-16
OpenJDK Runtime Environment (build 22.0.2+11)
OpenJDK 64-Bit Server VM (build 22.0.2+11, mixed mode, sharing)
```

###### 其它 Liunx 系统下 Java 安装

>**🚨不推荐新手使用其他系统**

前往[Liberica 下载页面](https://bell-sw.com/pages/downloads/#jdk-21-lts)下载对应 tar.gz 包解压后配置相应环境变量即可，此处不再展开。

#### 安装 Python (非必需)

Python 仅在需要安装 MCDR 的情况下才需要，如果不需要 MCDR 相关功能则可以跳过。**安装时需要注意 MCDR 需求 Python 3.8 及以上版本。**

##### Windows 下的安装

打开 [Python 下载页面](https://www.python.org/downloads/)

![windows python download](https://cdn.jsdelivr.net/gh/Zachery-Liu/blog-comments@main/pics/mcfabric/4.png)

直接点击按钮安装最新版 Python 即可。

安装时注意勾选将其加入 PATH，之后点击 ```Install Now``` 即可。

![windows python install](https://cdn.jsdelivr.net/gh/Zachery-Liu/blog-comments@main/pics/mcfabric/5.png)

在开始菜单看到 Python 软件即为安装成功。

![python done](https://cdn.jsdelivr.net/gh/Zachery-Liu/blog-comments@main/pics/mcfabric/7.png)

##### Linux 下的安装

使用具有 su 权限的账户登录至服务器，然后根据对应系统进行操作。

###### 使用 APT 包管理器的系统（如 Ubuntu、Debian 等）

直接使用下列命令安装即可

```sh
sudo apt install python3 -y
```

可以使用以下指令查看安装情况，返回 python 版本则安装成功。

```sh
python3 --version
```

一种可能的返回：

```sh
Python 3.10.12
```

###### 使用 YUM 包管理器的系统（如 CentOS 等）以及其他 Linux 系统

需要使用源码进行编译，新手建议使用 Ubuntu。

可以参考 Kyle 大佬的文章 [在 CentOS 7 上无缝安装 Python 3.12](https://www.bytezonex.com/archives/Iuh_gcJd.html)，
以及 信橙则灵 大佬的文章 [Linux 安装 Python 各个版本，这一篇就够了](https://blog.csdn.net/qq_42571592/article/details/122902266)。

可以使用以下指令查看安装情况，返回 python 版本则安装成功。

```sh
python3 --version
```

一种可能的返回：

```sh
Python 3.10.12
```

## 开始配置

此步骤开始 Windows 或 Linux 系统操作基本一致。

Windows 用户可以使用以下方法打开命令行：

- 使用快捷键 ```Windows徽标键```+```R```，输入 ```cmd``` 点击确定。
- 右键桌面左下角 Windows 图标，选择 ```Windows Powershell(管理员)```
- 右键桌面左下角 Windows 图标，选择 ```终端管理员```

### 创建服务器目录

新建一个文件夹，将其命名为 ```MC```。（其他自定义名称也可以，将下文中所有提及 ```MC``` 文件夹的部分全部替换成该自定义名称即可）

**Linux：**

```sh
cd /
mkdir MC
```

**Windows：**

在桌面创建文件夹或使用命令：

```bat
cd X:\Desktop
mkdir MC
```

### 配置 MCDR 框架（非必需）

参见 MCDR 官方文档中[快速上手](https://docs.mcdreforged.com/zh-cn/latest/quick_start.html)部分。

在阅读中需要注意的是，部分情况下的 ```pip``` 命令需要替换为 ```pip3```，另外一部分情况下的 ```pip3```命令也需替换为```pip```，可以多试一下。

### 配置 Minecraft 服务端

在 MC 文件夹下创建一个 ```server``` 文件夹（或使用 MCDR 安装后已有的 ```server``` 文件夹）。

**Linux:**

```sh
cd /MC
mkdir server
```

**Windows:**

使用文件管理器创建文件夹或使用命令：

```bat
cd X:\Desktop\MC
mkdir server
```

前往 [Fabric Server 下载页面](https://fabricmc.net/use/server/)，选择 Minecraft 版本，Fabric 版本和 Installer 版本都选最新即可。

点击蓝色按钮即可下载。

下载后移动/上传到 ```server``` 文件夹内。

![fabric download](https://cdn.jsdelivr.net/gh/Zachery-Liu/blog-comments@main/pics/mcfabric/6.png)

如果你的服务器没有 GUI（图形界面），并且装有 curl，可以在对应目录下使用下面的 ```CLI download``` 下载命令直接下载即可。

例如需要下载 1.21 版本的 Fabric 可以使用：

```sh
cd /MC/server
curl -OJ https://meta.fabricmc.net/v2/versions/loader/1.21/0.16.0/1.0.1/server/jar
```

下载完成后在 ```server``` 文件夹中运行 ```Launch Command``` 中的命令即可启动服务器。

>

例如：

```sh
cd /MC/server
java -Xmx2G -jar fabric-server-mc.1.21-loader.0.16.0-launcher.1.0.1.jar nogui
```

此命令中的```-Xmx2G```，即 ```-Xmx<MaxMem>``` 中的 ```<MaxMem>``` 为 JVM 可用的最大内存，可以按照服务器具体情况进行配置，例如空闲 6G 可以在该位置填写 ```6G```，空闲 5000M 可以填写 ```5000M``` 或者干脆直接删去由 JVM 虚拟机自动配置。

同样的，运行命令还可以加入其他参数，例如 ```-Xms<MinMem>``` 、```-javaagent``` 等，可以按需配置。

>命令行中，Java 选项应该添加在-jar 选项之前。
>对运行 Minecraft 服务器来说，最重要的事情莫过于内存。你可以使用-Xmx 选项设置服务器能被允许使用的内存量。通常-Xmx2G（最大内存 2GB）就已经够用了。*【编者注：此处存疑】*
>-Xms（初始化内存大小）不会对长时间运行有性能上的影响，但是你也可以设置它。-Xms512M（512MB）应该够了。
>对于一些版本的 JRE，可以使用“soft max heap size”（-XX:SoftMaxHeapSize=1G）。JRE 将尝试只使用那么多的内存，但如有必要，它将超过-Xmx 设置的最大值。如果你在服务器上运行许多东西，这可能会很有用。
>如果你的服务器运行在 64 位的 Solaris 系统上，并且使用了 64 位 Java，请添加-d64。  ————摘自 [Minecraft Wiki](https://zh.minecraft.wiki/w/Tutorial:%E6%9E%B6%E8%AE%BEJava%E7%89%88%E6%9C%8D%E5%8A%A1%E5%99%A8#Java%E9%80%89%E9%A1%B9)

最后可以将配置好的该条命令配置到 MCDR 中，详见 [配置 start_command](https://docs.mcdreforged.com/zh-cn/latest/configuration.html#start-command)。

## 启动

### 启动服务器

在 ```MC``` 目录中执行以下命令即可启动 MCDR 框架以及 Fabric 服务器。

```sh
mcdreforged
```

或者在 ```server``` 目录中执行上一步中的运行命令即可单独启动 Fabric 服务器。

### 服务器模组的选择

常见生电服务器大多使用了 ```Carpet Mod```。

> Carpet Mod  一款为原版 Minecraft 玩法带来的革新性模组，提升了玩家对游戏的控制力和理解度。通过精心设计的改动，消除了游戏中一些烦人的问题。在不影响游戏正常运行与体验的情况下，地毯端提供了一些可选的游戏特性或者原版特性缺少的内容，填补了原版游戏功能的空白。———— 摘自[MCMOD.CN](https://www.mcmod.cn/class/2361.html)

其他服务器模组可以按照需求自行添加，下面是部分个人常用服务端模组的推荐：

|模组名称 | 基本功能 |
|:-----:|:-----:|
|Fabric API|前置中的前置 |
|Carpet Extra|Carpet Mod 功能扩展 |
|Carpet TIS Addon|Carpet Mod 功能扩展 |
|C^2M Engine (C2ME、钡)|区块优化 |
|EasyAuth|离线服务器登录插件 |
|Essential Commands|提供 /tpa 之类的常用基础指令 |
|Gugle Carpet Addition|又一个 Carpet Mod 功能扩展 |
|No Chat Report|屏蔽微软高版本的聊天举报 |
|Servux|为部分客户端模组提供支持 |
|Shulkerbox|快捷潜影箱 |
|Sodium|优化模组 |
|Syncmatica|共享投影图 |

同时，推荐在服务端安装部分客户端所用模组以获得更好的游戏体验。

>**🚨安装 mod 时请注意添加对应模组依赖的前置，未添加前置时可以通过运行 Fabric 服务器并查看报错信息来知晓所需的前置。**

将选好的模组直接添加到 ```server``` 目录下的 ```mods``` 文件夹中并重启服务端即可。

### MCDReforged 插件的选择

请访问 [插件仓库](https://mcdreforged.com/zh-CN/plugins) 来选取自己心仪的插件。

以下是部分个人常用插件的推荐：

|插件名称 | 基本功能 |
|:-----:|:-----:|
|Quick Backup Multi|备份插件 |
|Timed QBM|定时备份插件 |
|QQChat|Q 群机器人 |
|Here|显示坐标并高亮玩家|

>**🚨安装插件时请注意安装插件所需的对应的 Python 模块，一般在插件的自述文件部分都有写明。**

将选好的插件直接加入 ```MC``` 目录下的 ```plugins``` 文件夹中并重启服务器或在已运行的服务器中执行下面的命令即可。

```
!!MCDR reload plugin
```

## 服务器的深入配置及其他内容

关于 Fabric 服务器配置（Server.properties）的设置请参阅 [Minecraft 服务器属性](https://zh.minecraft.wiki/w/Server.properties#Minecraft%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%B1%9E%E6%80%A7)。

关于 MCDR 框架的配置请参阅[MCDReforged 官方文档](https://docs.mcdreforged.com/zh-cn/latest/)。

关于端口转发可以参阅 [→利用端口映射实现远程联机|MinecraftMC 我的世界联机教程](https://zhuanlan.zhihu.com/p/342532953)。
