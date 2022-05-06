# Android 简介

## Android 系统架构

### Linux 内核层

Android系统基于Linux内核，这层为Android设备的各种硬件提供了底层的驱动，如显示、音频、相机、蓝牙、WiFi等驱动和电源管理。

### 系统运行库层

系统运行库层通过C/C++库为Android系统提供了主要特性支持。如SQLite库提供了数据库支持，OpenGL|ES库提供了3D绘图支持，Webkit库提供了浏览器内核支持。
在这层还有Android运行时库,它主要提供了一些核心库,允许开发者使用Java语言来编写Android应用。另外,Android运行时库中还包含了Dalvik虚拟机(5.0系统之后改为ART运行环境) ,它使得每一个Android应用都能运行在独立的进程中,并且拥有一个自己的虚拟机实例。相较于Java虚拟机,Dalvik和ART都是专门为移动设备定制的,它针对手机内存、CPU性能有限等情况做了优化处理。

### 应用框架层

这层提供了构建应用程序时用到的各种API，开发者可以使用这些API构建自己的应用。

### 应用层

所有安装在手机的应用程序都属于这一层。

<img src="../../res/img/image-20220501220803769.png" alt="image-20220501220803769" style="zoom:80%;" />

## Android 开发环境

开发Android程序需要准备的工具：

- JDK：Java软件开发工具包，包含Java运行环境、工具集合、基础类库等
- Android SDK：Google提供的Android开发工具包，开发Android程序时，需要引入该工具包来使用Android相关API
- Android Studio

## Android 项目结构

<img src="../../res/img/image-20220501233131617.png" alt="image-20220501233131617" style="zoom:67%;" />

### Project 模式的项目结构

- .gradle和.idea：两个目录下放置Android Studio自动生成的一些文件
- app：项目代码、资源等都是放置在这个目录
- gradle：该目录下包含gradle wrapper的配置文件
- .gitignore：该文件用来将指定目录或文件排除在版本控制之外的
- build.gradle：项目全局的gradle构建脚本
- gradle.properties：全局的gradle配置文件，配置的属性会影响到项目中所有的gradle编译脚本
- gradlew和gradlew.bat：命令行界面中执行gradle命令，gradlew在Linux或Mac使用，gradlew.bat在Windows使用
- iml：所有IntelliJ IDEA项目自动生成的一个文件
- local.properties：该文件用于指定本机中的Android SDK路径
- setting.gradle：用于指定项目中所有引入的模块

<img src="../../res/img/image-20220501234215796.png" alt="image-20220501234215796" style="zoom: 80%;" />

### app 目录下的项目结构

- build：和外层build目录相似，包含一些编译时自动生成的文件
- libs：使用到的第三方jar包，需要把jar包都放在libs目录下，放在这个目录下的jar包会被自动添加到项目的构建路径里
- androidTest：用来编写Android Test测试用例
- java：放置所有Java代码的地方
- res：项目中时用到的图片、布局、字符串都放在这个目录下。图片放在drawable、布局放在layout、字符串放在values
- AndroidManifest.xml：Android项目的配置文件，在这个文件注册定义的四大组件，给应用程序添加权限声明
- test：编写Unit Test测试用例
- .gitignore：
- app.iml
- build.gradle：app模块的gradle构建脚本
- proguard-rules.pro：用于指定项目代码的混淆规则

## 项目资源

### 使用项目资源

- 在代码中通过R.string.app_name可以获得该字符串的引用
- 在XML中通过@string/app_name可以获得该字符串的引用

string可以替换，引用的图片资源可以替换成drawable，应用图标替换成mipmap，布局文件替换成layout
