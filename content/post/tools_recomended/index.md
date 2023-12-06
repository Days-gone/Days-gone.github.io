+++
title = 'Tools_recomended'
date = 2023-12-06T09:01:58Z
draft = false
+++
# Operate System

## Windows
大家都比较熟悉，暂且先不做介绍
## Linux

### Ubuntu

给出一个比较好的B站视频链接：

[Windows 和 Ubuntu 双系统的安装和卸载\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1554y1n7zv/?spm_id_from=333.337.search-card.all.click)

# 浏览器

浏览器作为访问web网页的客户端，使用体验的影响比较大，这里介绍一些浏览器（不包含mac端）。在安装时建议在你的C盘外的硬盘上开辟新的文件夹安装。

## Edge

[Download Microsoft Edge](https://www.microsoft.com/en-us/edge/download?form=MA13FW)

## Firefox火狐

[Firefox 火狐浏览器 - 全新、安全、快速 | 官方最新下载](http://www.firefox.com.cn/)

## Chrome

[Google Chrome 网络浏览器](https://www.google.cn/chrome/index.html)


# 搜索引擎

浏览器的体验其实也有很大部分决定于搜索引擎，如果你用过百度那么你一定知道，当你搜索某项事物时，排名最靠上的那几个就往往不是你想要的（多半是广告/一些“下崽”软件）。

## Bing

如果你使用的是Edge那么默认的搜索引擎就是bing，他是由微软提供的一款搜索引擎。

[Bing](www.bing.com)

## Google

全球最大的搜索引擎，但是搜索结果中含有英文结果比较多，比较锻炼大家的英文水平。缺点是需要使用科学上网。

[Google]([www.google.com](https://www.google.com/))

# IDE

ide是我们平常工作的地方，有一个你满意的ide环境会让你工作的更加舒心。在这里我们分语言进行推荐。

## C++

### Dev-C++

更适合新手入门快速部署本地C++编译环境，省去了花费在环境配置上的经历，更专注于快速入手学习C++的语法。

### Microsoft Visual Studio

由微软公司开发，为C/C++提供专业的工作环境，对于初学者而言安装比较简单，无需自己配置太多东西。

### Visual Studio Code

也是由微软公司开发，但相对于MVS更加轻量级，拥有比较多的拓展包可供下载。是IDE中的万金油！使用我们之前推荐的搜索引擎进行下载即可。因为自己用的比较多的是VSC，所以就先介绍VScode吧。

#### TRY VScode入门

##### 简单了解

LSP即Language Server Protocol，LSP是少数基于JSON的语言服务器数据交换协定，由GitHub代管，并采用CC及MIT授权。该协定主要用来促进编辑器及语言服务器之间的互动，允许开 发人员在各种编辑器或整合开发环境中存取智慧型的程序语言工具，像是以符号搜寻、语法分析、自动完成代码、移至定义、描绘轮廓或重构等。LSP一个非常显著的功能就是智能感知（包括智能提示、错误检测、文件/函数跳转，IntelliSense）。

C语言常见的LSP插件有：**clangd**、**ccls**、**MS****的****C++****拓展**。对于单个文件的C++程序你可以采用MS的C++拓展，或者Clangd，但对于多文件的C++工程来说我们更推荐使用Clangd + Cmake + CmakeTools的编辑器设置，比较好用。

##### 配置自己的VScode-C++环境。

首先安装好VScode，注意此处假设我们的电脑用户名不出现中文，因为似乎在最新的MingW中如果要使用LLDB进行调试那么你的C盘下的用户是不可以为中文名的，否则会调试的时候出现找不到LLDB插件的错误。

假设我们已经安装好了VScode，并且将MingW下的bin目录和CMake添加到了环境变量中，那么接下来，打开VScode：

1、点击拓展栏，搜索“Clangd”，并安装。

2、搜索“CMake Tools”下载并安装。

3、点击左下角的设置，并在搜索栏中输入“clangd path"，在对应的栏中输入clangd的具体位置（如果你已经添加了环境变量可以直接输入clangd，否则就写出完整的路径。）

4、创建一个新的项目文件夹假设此处就叫做Project，在Project/ 下创建一个叫做CMakeLists.txt的文件，注意大小写。之后我们就可以使用CMakeLists.txt来管理我们这个项目的构建过程了。

5、创建几个新的文件夹满足：Project/src, Project/build, Project/include这样的结构，在我们的src文件夹下面创建一个新的cpp文件，假如叫做main.cpp。那么在main.cpp中输入:

```C++
#include <iostream>

int main(){
    std::cout << "hello,c++";
    return 0;
}
```

接下来修改CMakeLists.txt:

```
cmake_minimum_required(VERSION 3.12)
project(BoostExample)

set(CMAKE_CXX_STANDARD 14)

# Find Boost components
# find_package(Boost REQUIRED COMPONENTS filesystem)

# Add the include directory
# 我们自己写的头文件可以放在这个include目录下面
include_directories(include)

# Add the executable target
# 生成可执行文件即能够运行的程序
add_executable(result src/main.cpp)
```

此处只是做了一个非常简单的介绍，具体的文件可以阅读Cmake的文档。

6、重新打开VScode，注意点击左上角的“文件->打开文件夹->选中Project（这个文件夹的一级子目录下应该有个CMakeLists.txt）。打开之后，clangd和cmaketools会自动扫描可使用的套间（clang++、g++）等，选择你的套件。之后在左下角往右浏览找到build或者一个三角形（像播放按钮的形状）并点击，；build只生成可执行文件，而运行则会生成并执行该文件。

# 记笔记

这里我们说的是在学习过程中记录blog式的笔记软件推荐：

## Markdown

Markdown是一种轻量级标记语言，排版语法简洁，让人们更多地关注内容本身而非排版。它使用易读易写的纯文本格式编写文档，可与HTML混编，可导出 HTML、PDF 以及本身的 .md 格式的文件。因简洁、高效、易读、易写，Markdown被大量使用，如Github、Wikipedia、简书等。 -- markdown official introduction

[Markdown 基本语法 | Markdown 官方教程](https://markdown.com.cn/basic-syntax/)

### Typora

给出一些文章链接来介绍吧：

[2020Typora小白完全使用教程](https://zhuanlan.zhihu.com/p/293557841)

[typora是什么软件\_一款好用的轻量级Markdown编辑器 - 极客库](https://www.geekku.com/software/1533.html)

### Zettlr

## LateX

latex适合用于写一些论文/报告之类的任务，如果你也对word的使用不够熟悉并烦恼于它的布局配置问题的话，不如学习latex来完成你的任务，初学需要花费时间了解latex的使用方法，但一旦学会就会省去很多时间并轻松完成精美的布局设置。

因为自己只使用过线上的，所以推荐一下这个网站。

[Overleaf, Online LaTeX Editor](https://www.overleaf.com/)

Tutorial:

[Learn LaTeX in 30 minutes](https://www.overleaf.com/learn/latex/Learn_LaTeX_in_30_minutes)

# Thank U For Reading
![喜多！](test1.jpg)
