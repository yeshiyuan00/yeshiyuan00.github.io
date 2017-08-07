---
title: react-native学习系列--环境搭建
date: 2016-08-19 16:11:04
tags: react-native
---
接下来的计划是学习react-native开发，最终目标是能够使用react-native完成一个App。为了提升学习效率、加深印象，在学习的过程中将使用博客的方式来做笔记，将学到的知识、心得体会以及遇到的坑记录下来，方便回顾和记忆。本篇文章时react-native系类的第一篇文章，我们从react-native的开发环境搭建开始。  
  react-native的官方项目地址：<https://github.com/facebook/react-native>

### 环境搭建
  工欲善其事，必先利其器，在使用react-native开发之前，我们必须先将开发环境搭建起来。由于我的电脑是MAC，所以下面的操作都是基于MAC环境下进行的。  
  
  首先需要安装node和watchman，官方推荐使用[Homebrew](http://brew.sh/)来进行安装,终端执行命令：

```  
brew install node  
brew install watchman  
```
接下来使用node提供的npm工具安装react-native的命令行工具：  

```
npm install -g react-native-cli
```
如果提示需要权限则使用sudo来安装。如果以上命令都成功执行，则我们的开发环境也已经搭建完成了，接下来就可以使用react-native来进行开发了。

### 创建第一个React-native项目
首先使用react-native命令行工具创建一个名为AwesomeProject的项目：  

```
react-native init AwesomeProject
```
然后连接手机或Android模拟器，执行下面命令：  

```
cd AwesomeProject
react-native run-android
```
这样项目就在Android上面跑起来了，初始化的项目界面如下：  
![](/img/3/react-native-init-project.png)

### 遇到的问题
1.如果是在小米手机上跑该项目的话，执行react-native run-android可能会不成功，这个bug是MIUI系统造成的，在开发者选项中把启用MIUI优化那个选项关掉就可以了。还有一个解决方法就是使用Android studio打开Android项目，然后再Android studio中启动即可。  
2.在真机上运行的时候可能会出现could not connect to development server的错误，执行下面的adb命令，然后reload即可。
  
```
adb reverse tcp:8081 tcp:8081
```
3.如果端口号8081被其他进程占用了的话，那么就会导致我们的服务无法启动，下面命令可以查看占用8081端口的进程：  

```
lsof -n -i4TCP:8081 | grep LISTEN
```
执行上面命令后会列出占用端口的进程的信息，然后执行：  

```
kill -9 pid
```
pid用进程的端口号代替，然后重新运行：  

```
react-native start
```
即可启动服务。
















