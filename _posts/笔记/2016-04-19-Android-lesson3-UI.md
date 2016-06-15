---
layout: post
title: UI开发
category: 笔记
tags: Android
keywords: 
description: 
---

##### 一、常见控件的使用方法

    1.1 TextView
        - TextView可以说是Android中最简单的一个控件，主要用于在界面上显示一段文本信息。

    1.2 Button
        - Button是程序用于和用户进行交互的一个重要控件。属性类似TextView。

    1.3 EditText
        - EditText是程序用于和用户进行交互的另一个重要控件，它允许用户在控件里输入和编辑内容，并可以在程序中对这些内容进行处理。

    1.4 ImageView
        - ImageView是用于在界面上展示图片的一个控件，通过它可以让我们的程序界面变得更加丰富多彩。

    1.5 ProgressBar
        - ProgressBar用于在界面上显示一个进度条，表示我们的程序正在加载一些数据。

    1.6 AlertDialog
        - AlertDialog可以在当前的界面弹出一个对话框，这个对话框是置顶于所有界面元素之上的，能够屏蔽掉其他控件的交互能力，一般用于提示一些非常重要的内容或者警告信息。

    1.7 ProgressDialog
        - ProgressDialog和AlertDialog有点类似，都可以在界面上弹出一个对话框，都能够屏蔽掉其他控件的交互能力。
        - 不同的是， ProgressDialog会在对话框中显示一个进度条，一般是用于表示当前操作比较耗时，让用户耐心等待。用法也同AlertDialog类似。
        ```java
        ProgressDialog progressDialog = new ProgressDialog(MainActivity.this);//构建ProgressDialog对象
        progressDialog.setTitle("This is ProgressDialog");
        progressDialog.setMessage("Loading...");
        progressDialog.setCancelable(true);//如果设为false的话必须调用.dismiss()来关闭
        progressDialog.show();
        ```

##### 二、详解四种布局
    ![布局-控件](/public/img/android/布局-控件.png)

    2.1 LinearLayout
        - LinearLayout又称线性布局，是一种非常常用的布局。这个布局会将它所包含的控件在线性方向上依次排列。
        - 排列是水平还是垂直，可通过android:orientation来指定。

    2.2 RelativeLayout
        - RelativeLayout又称相对布局，也是一种非常常用的布局。
        - 相比LinearLayout的排列，RelativeLayout显得更加随意一些，它可以通过相对定位的方式让控件出现在布局的任何位置。

    2.3 FrameLayout
        - FrameLayout相比前两种布局就简单太多了，因此应用场景也少很多。这种布局没有任何的定位方式，所有的控件都会拜访在布局的左上角。
        - eg：按钮和图片都位于布局左上角，由于图片是按钮之后添加的，所以图片压在了按钮的上面

    2.4 TableLayout
        - TableLayout允许我们使用表格的方式来排列控件，这种布局也不是很常用。


##### 三、自定义控件
    ![控件](/public/img/android/控件.png)

    - 如图，我们所用的所有控件都是直接或者简介继承自View，所用的所有布局都是直接或间接继承自ViewGroup的。
    - View是Android中一种最基本的UI组件，它可以在屏幕上绘制一块矩形区域，并能响应这块区域的各种事件，因此我们使用的各种控件其实就是在View的基础上又添加了各自特有的功能。
    - ViewGroup则是一种特殊的View,它可以包含很多字View和子ViewGroup，是一个用于放置控件和布局的容器。

    3.1 引入布局
        - 先自定义一个布局，比如tittle.xml
        - 然后在需要引用布局的布局里面直接 include,即：<include layout="@layout/tittle" />

    3.2 创建自定义控件
        - 引入布局的技巧解决了重复编写布局代码的问题
        - 但是如果布局中有一些控件要求能够响应事件，我们还是需要在每个活动中为这些控件单独编写一次事件注册的代码。
        ```html
        <!--包名要完整。每当我们在一个布局中引入ThirdLayout，返回按钮和编辑按钮的点击事件就已经自动实现好了，也是省去了很多编写重复代码的工作-->
        <gaojing.layoutui.TittleLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        </gaojing.layoutui.TittleLayout>
        ```

    3.3 最常用和最难用的控件---ListView
        - ListView允许用户通过手指上下滑动的方式将屏幕外的数据滚动到屏幕内，同时屏幕上原有的数据则会滚动出屏幕。比如查看手机联系人、翻阅微博等。。

##### 四、ListView

    4.1 ListView的简单用法
        - 比如在手机界面上显示很多行水果名称等，超过手机屏幕大小后就可以通过上下滑动的方式来查看数据。

    4.2 定制ListView界面
        - 在手机屏幕上不仅能够显示出水果名称，旁边还能显示水果图片。

    4.3 提升ListView的运行效率

    4.4 ListView的点击事件

##### 五、 控件、布局的单位和尺寸
    * 尽量将控件或者布局的大小指定成match_parent或者wrap_content，如果必须要指定一个固定值，则使用dp来作为单位，指定文字大小的时候使用sp作为单位 *

    5.1 px pt 不常用，因为分辨率不同的屏幕显示差异较大
        - px 像素，即屏幕中可以显示的最小元素单元
        - pt 磅数，一般作为字体的单位来使用

    5.2 dp sp
        - dp 又称dip,是密度无关像素的意思，和px相比，它在不同密度的屏幕中的显示比例将保持一致。
        - sp 可伸缩像素的意思，它采用了和dp同样的设计理念，解决了文字大小的适配问题。
        - Android中的密度就是屏幕每英寸所包含的像素数，通常以dpi位单位，eg:一个手机屏幕宽2英寸长3英寸，分辨率是320*480，那这个屏幕的密度就是160dpi，密度值越高，屏幕的显示效果就越清晰。我们可以通过代码来得知当前屏幕的密度值。
        ```java
        setContentView(R.layout.activity_main);

        float xdpi = getResources().getDisplayMetrics().xdpi;
        float ydpi = getResources().getDisplayMetrics().ydpi;
        Log.d("MainActivity", "xdpi is " + xdpi);
        Log.d("MainActivity", "ydpi is " + ydpi);
        ```

##### 六、动手制作一个精美的聊天界面






