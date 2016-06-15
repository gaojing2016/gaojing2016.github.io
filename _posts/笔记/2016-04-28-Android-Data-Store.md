---
layout: post
title: 数据存储
category: 笔记
tags: Android
keywords: 
description: 
---

##### 一、数据持久化简介
    * - 瞬时数据：存储在内存当中，但是可能会因为程序关闭或其他原因导致内存被回收而丢失的数据。
      - 数据持久化：将瞬时数据保存到存储设备中，保证即使在手机或电脑关机的情况下，哪些数据仍然不会丢失。
      - 保存在内存中的数据处于瞬时状态；保存在存储设备中的数据处于持久状态；持久化技术则提供了一种机制可以让数据在瞬时状态和持久状态之间进行转换。
    *  

    1.1、Android中主要的数据持久化技术
        - 1）文件存储
            * 用于存储一些简单的文本数据或者二进制数据 *
        - 2）SharedPreference存储
            * 适用于存储一些键值对 *
        - 3）数据库存储
            * 用于存储复杂的关系型数据 *

##### 二、文件存储
    - 文件存储是Android中最基本的一种数据存储方式。
    - 它不对存储的内容进行任何的格式化处理，所有数据都是原封不动地保存到文件当中的，因而比较适合存储简单的文本数据或二进制数据。
    - 如果想保存一些较为复杂的文本数据，就需要定义一套自己的格式规范，便于之后将数据从文件中重新解析出来。

    2.1、 将数据存储到文件中
        - 1）将数据保存到文件  openFileOutput(文件名,文件操作模式)方法
            - 文件名：在文件创建的时候就是这个名称，指定文件名时不可以包含路径。因为所有文件都是默认存储到/data/data/<package name>/files/目录下的。
            - 文件操作模式：主要有两种模式 
                - MODE_PRIVATE，默认的操作模式，表示当指定同样文件名的时候，所写入的内容将会覆盖源文件中的内容。
                - MODE_APPEND，表示如果该文件已存在就往文件里面追加内容，不存在就创建新文件。


        - 2） 从文件中读取数据 openFileInput(要读取的文件名)方法
            - 系统会自动到/data/data/<package name>/files/目录下加载这个文件，并返回一个FileInputStream对象，得到这个对象后再通过JAVA流就可以将数据读取出来了。

##### 三、 SharedPreferences存储
        - SharedPreferences是使用键值对的方式来存储数据的。
        - 也就是说当保存一条数据的时候，需要给这条数据提供一个对应的键，这样在读取数据的时候就可以通过这个键把对应的值取出来。
        - SharedPreferences还支持多种不同的数据类型存储，如果存储的数据类型是整型（字符串），那么读取出来的数据也是整型（字符串）；

    3.1、 将数据存储到SharedPreferences中
        * 要想使用SharedPreferences来存储数据，首先要获取到SharedPreferences对象，获取对象的三种方法如下： *
        - 1） getSharedPreferences(arg1, arg2)方法
            - arg1:指定SharedPreference文件的名称，如果指定的文件不存在则会创建一个，文件路径/data/data/<package name>/shared_prefs/目录。
            - arg2:指定操作模式 
            - MODE_PRIVATE：默认的操作模式，和直接传入0效果相同，表示只有当前的应用程序才可以对这个SharedPreferenced文件进行读写。
            - MODE_MULTI_PROCESS：一般用于多个进程中对同一个SharedPreferences文件进行读写的情况。

        - 2） Activity类中的getPreferences(arg)方法
            - 该方法同getSharedPreferences(arg1,arg2)类似
            - arg:操作模式,只接收这一个参数，因为使用这个方法时会自动地将当前活动类名作为SharedPreferences的文件名。

        - 3） PreferenceManager类中的getDefaultSharedPreferences(arg)方法
            - 这是一个静态方法，它接收一个Context参数，并自动使用当前应用程序的包名作为前缀来命名SharedPreferences文件。

    3.2、 得到SharedPreferences对象之后，就可以开始向SharedPreferences文件中存储数据了：
        - step1:调用SharedPreferences对象的edit()方法来获取一个SharedPreferences.Editor对象。
        - step2:SharedPreferences.Editor对象中添加数据,比如添加一个布尔型数据就使用putBoolean方法,添加一个字符串则使用putString()方法,以此类推.
        - step3:调用commit()方法将添加的数据提交,从而完成数据存储操作。


##### 四、SQLite数据库存储
    - SQLite是一款轻量级的关系型数据库，它的运算速度非常快，占用资源很少，通常只需要几百K的内存就足够了，因而特别适合在移动设备上使用。
    - SQLite不仅支持标准的SQL语法，还遵循了数据库的ACID事务，所以只要你以前使用过其他的关系型数据库，就可以很快上手SQLite。
    - SQLite比一般数据库要简单，它甚至不用设置用户名和密码就可以使用。
    - Android正是把这个功能极为强大的数据库嵌入到了系统当中，使得本地持久化的功能有了一次质的飞跃。
    - 文件存储和SharedPreferences存储毕竟只适用于去保存一些简单的数据和键值对，当需要存储大量复杂的关系型数据时，就会发现以上两种存储方式很难应付得了。

    4.1 创建数据库
        * Android为了让我们能够更加方便地管理数据库，专门提供了一个SQLiteOpenHelper帮助类，借助这个类就可以非常简单地对数据库进行创建和升级。 *
        * SQLiteOpenHelper是一个抽象类，如果想使用它，就需要创建一个自己的帮助类去继承它。 *
    
    4.2 升级数据库

    4.3 添加数据

    4.4 更新数据库

    4.5 删除数据

    4.6 查询数据

    4.7 使用SQL操作数据库

##### 五、SQLite数据库的最佳实践











