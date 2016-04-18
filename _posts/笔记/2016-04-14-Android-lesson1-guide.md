---
layout: post
title: 安卓启程
category: 笔记
tags: Android
keywords: 
description: 
---


Android大致可以分为四层架构，五块区域。
###一、Android系统架构
![安卓框架](/_posts/img/Android_layout.jpg)

- 应用程序层
    - 所有安装在手机上的应用程序都是属于这一层的，比如系统自带的联系人、短信等程序。
- 应用程序框架层
	- 该层主要提供了构建应用程序时可能用到的各种API，Android自带的一些核心应用就是使用这些API完成的，开发者可以通过使用这些API来构建自己的应用程序。
- 函数库
    - Android包含一套被不同组件所使用的C/C++库的集合。
    - 该层通过一些C/C++库来为Android系统提供了主要的特性支持。如SQLite库--提供数据库支持；OpenGLIES库--提供3D绘图支持；Webkit库--提供浏览器内核的支持等。
- Android运行时
    - 核心库集，提供了Java语言核心库所能使用的绝大部分功能。能够允许开发者使用Java语言来编写Android应用。
    - Dalvik虚拟机，负责运行安卓应用程序。它使得每一个Android应用都能运行在独立的进程当中，并且拥有一个自己的Dalvik虚拟机实例。相较于Java虚拟机，Dalvik是专门为移动设备定制的，它针对手机内存、CPU性能有限等情况做了优化处理。
- Linux内核层
    - Android系统是基于Linux 2.6内核的，提供了安全性、内存管理、进程管理、网络协议栈和驱动模型等核心系统服务。除此之外，Linux内核也是系统硬件和软件叠层之间的仇抽象层。
    该层为Android设备的各种硬件提供了底层的驱动，如显示驱动、音频驱动、照相机驱动、蓝牙驱动、wifi驱动、电源管理等。


###二、Android应用开发特色
- 四大组件
    - Activity(活动)
        - 是所有Android应用程序的门面，凡是在应用中你看得到的东西，都是放在活动中的。
    - Service(服务)
        - 你无法看到它，但它会一直在后台默默地运行，即使用户退出了应用，服务仍然是可以继续运行的。
    - BroadcastReceiver(广播接收器)
        - 可以允许你的应用接收来自各处的广播消息，比如电话、短信等，当然你的应用也可以向外发出广播消息。
    - ContentProvider(内容提供器)
        - 为应用程序之间共享数据提供了可能，比如你想要读取系统contact中的联系人，就需要通过内容提供器来实现。
 
- 丰富的系统控件
    - Android系统为开发者提供了丰富的系统控件，使得我们可以很轻松地编写出漂亮的界面。也可以定制属于自己的控件。

- SQLite数据库
    - Android系统自带了这种轻量级、运算速度极快的嵌入式关系型数据库。它不仅支持标准的SQL语法，还可以通过Android封装好的API进行操作，让存储和读取数据变得非常方便。
    
- 地理位置定位
    - 安卓手机内置GPS

- 强大的多媒体
    - 如音乐、视频、录音、拍照、闹铃等，这些都可以在程序中通过代码进行控制。让应用更加丰富多彩。。。

- 传感器
    - 安卓手机都会内置多种传感器，如加速度传感器、方向传感器等。


###三、分析第一个安卓程序（Hello world）
*3.1、项目的目录结构*
- app/libs:如果你的项目中使用到了第三方的Jar包，就需要把这些Jar包都放在libs目录下，放在这个目录下的Jar包都会被自动添加到构建路径里去。
- app/src/main/AndroidManifest.xml:整个项目的配置文件，你在程序中定义的所有四大组件都需要在这个文件里注册。另外还可以在这个文件中给应用程序添加权限声明，也可以重新指定你创建项目时指定的程序最低兼容版本和目标版本。
- app/src/main/java:放置所有Java代码的地方。
- app/src/main/res:该目录下文件较多，简单来说，就是你在项目中使用到的所有图片、布局、字符串等资源都要存放在这个目录下。有很多子目录如下：
    - drawable: 所有以drawable开头的文件夹都是用来放置图片的。之所以有很多drawable开头的文件夹，其实主要是为了让程序能够兼容更多的设备。在制作程序的时候最好能够给同一张图片提供几个不同分辨率的副本，分别放在这些文件夹下，然后当程序运行的时候会自动根据当前运行设备分辨率的高低选择加载哪个文件夹下的图片。但是一般美工只会提供给我们一份图片，这时你就把所有图片都放在drawable-hdpi文件夹下好了。
    - layout: 用来放置布局文件的。
    - values: 所有以values开头的文件夹都是用来放字符串的。
    - menu: 用来放菜单文件的。
- project.properties:就是通过一行代码指定了编译程序时所使用的SDK版本。


*3.2、项目具体是怎么运行呢？*
- app/src/main/AndroidManifest.xml:
```html
<!-- 以下代码表示对MainActivity这个活动进行注册，没有在AndroidManifest.xml里面注册的活动是不能使用的 -->
<activity
    android:name=".MainActivity"
    android:label="@string/app_name">
    <!-- intent这两行代码很重要，它表示MainActivity是这个项目的主活动，在手机上点击应用图标，首先启动的就是这个活动 -->
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />

        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```
- app/src/main/java/gaojing/myapp/MainActivity.java:
```java
//gaojing:继承，项目中所有的活动都必须要继承Activity才能拥有活动的特性。
public class MainActivity extends ActionBarActivity {
    
    @Override
        //gaojing：方法一 是一个活动被创建时必定要执行的方法，其中只有两行代码，并且没有Hello world！字样，那么Hell world在哪里定义呢？
        //Android程序的设计讲究额逻辑和视图分离，因此是不推荐在活动中直接编写界面的，更加通用的一种作法是，在布局文件中编写界面，然后在活动中引入进来。
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
        }

    @Override
        //gaojng:方法二 用于创建菜单
        public boolean onCreateOptionsMenu(Menu menu) {
            // Inflate the menu; this adds items to the action bar if it is present.
            getMenuInflater().inflate(R.menu.menu_main, menu);
            return true;
        }
}
```
- app/src/main/res/layout/activity_main.xml:
```html
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:tools="http://schemas.android.com/tools"
android:layout_width="match_parent"
android:layout_height="match_parent"
android:paddingBottom="@dimen/activity_vertical_margin"
android:paddingLeft="@dimen/activity_horizontal_margin"
android:paddingRight="@dimen/activity_horizontal_margin"
android:paddingTop="@dimen/activity_vertical_margin"
tools:context="gaojing.myapp.MainActivity">

<!-- TextView是安卓系统提供的一个控件，用于在布局中显示文字的。 -->
<TextView
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:text="Hello World!" />
<!-- 注意，真正的字符串也不是在布局文件中定义的，Android不推荐在程序中对字符串仅此能够硬编码，更好的作法一般是把字符串定义在/res/value/strings.xml里面，然后可以
在布局文件或代码中引用。不过通过AndroidStudio默认创建的这个HelloWorld app的字符串是直接定义在这里的。 -->
</RelativeLayout>
```

###四、详解项目中的资源
在之前的3.1章节，我们介绍了项目的目录结构，其中包含项目资源，那么我们应该如何去使用这些资源呢？
比如在strings.xml中找到Hello world1字符串，我们有两种方式可以引用它：
- 1、在代码中通过R.string.hello_world可以获得该字符串的引用；(R.layout.activity_main)
- 2、在xml中通过@string/hello_world可以获得该字符串的引用。
基本的语法就是上面两种方式，其中string部分是可以替换的，如果是引用的图片资源就可以换成drawable，如果是引用的布局文件就可以替换成layout,以此类推。

项目的图标：在AndroidManifest.xml中通过android:icon="@mipmap/ic_launcher"定义，所以修改项目图标就知道怎么办了吧。


###五、项目日志
这个目前我还没有设置。不过提醒一点，在项目中不要使用System.out.print()哦。

























