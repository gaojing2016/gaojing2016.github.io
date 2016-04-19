---
layout: post
title: 探究activity
category: 笔记
tags: Android
keywords: 
description: 
---

手动创建活动
活动（Activity）是最容易吸引到用户的地方，它是一种可以包含用户界面的组件，主要用于和用户进行交互。一个应用程序中可以包含零个或多个活动，但步包含任何活动的应用程序很少见，谁也不想让自己的应用永远无法被用户看到吧？

### 一、活动的基本用法
- 1.1 手动创建活动
    - package->New->Java Class
    - onCreate()方法，调用父类的onCreate()方法。这是默认的实现，后面我们需要在里面加入很多自己的逻辑。
    ```java
    public class FirstActivity extends Activity {
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(saveInstanceState);
       }
    }
    ```
- 1.2 创建布局
    -res/layout->New->AndroidXMLFile, first_layout.xml, 根元素(RootElement)默认选择位LinearLayout
    - 如之前所说，安卓程序设计讲究逻辑和视图分离，最好每一个活动都能对应一个布局，布局就是用来显示界面内容的。
    
    ```html
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_heigth="match_parent"
        android:orientation="vertical">

        <!-- Button元素：对布局稍作编辑，添加一个按钮-->
        <!-- android:id 是给当前元素定义一个唯一标识符，之后可以在代码中对这个元素进行操作。-->
        <!-- 在XML中引用一个id,就使用@id/id_name这种语法-->
        <!-- 在XML中定义一个id,就使用@+id/id_name这种语法-->
        <!-- android:layout_with指定了当前元素的宽度（这里是让当前元素和父元素一样宽） -->
        <!-- android:layout_heigth指定了当前元素的高度（这里是让当前元素的高度只要能刚好包含里面的内容就行）-->
        <!-- android:text指定了元素中显示的文字内容。-->
        <Button
            android:id="@+id/button_1" 
            android:layout_width="match_parent"
            android:layout_heigth="wrap_parent"
            android:text="Button 1"
        />
    </LinearLayout>
    ```

- 1.3 加载布局
    - 一个简单的布局编写完成后，我们就要在活动中加载这个布局,通过调用setContentView()方法实现。
    
    ```java
    public class FirstActivity extends Activity {
        @Override
            protected void onCreate(Bundle savedInstanceState) {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.first_layout);
                //调用setContentView()方法给当前的活动加载一个布局，在setContentView方法中，我们一般都会传入一个布局文件id。
                //项目中添加的任何资源都会在R文件中生成一个相应的资源id,因此我们刚才创建的first_layout.xml布局的id现在应该是已经添加到R文件中了。
            }   
    }
    ```

- 1.4 在AndroidManifest文件中注册
    - 所有的活动都要在AndroidManifest.xml中进行注册才能生效。活动的声明注册要放在<application>标签内，通过<activity>标签来对活动进行注册。
    
    ```html
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">

        <!-- 活动的声明注册要放在<application>标签内，通过<activity>标签来对活动进行注册。 -->
        <!-- android:name用于指定具体注册哪一个活动，gaojing.activitytest.FirstActivity的缩写，package/id已经指定-->
        <!-- android:label用于指定活动中标题栏的内容，标题栏是显示在活动最顶部的给活动指定的label不仅会成为标题栏中的内容，还会成为启动器(Launcher)中应用程序显示的名称-->
        <!-- intent-filter如果你想让FirstActivity作为我们这个程序的主活动，即点击 桌面应用程序图标时首先打开的就是这个活动，那就一定要加入这两句声明。如果你的应用程勋中没有声明任何一个活动作为主活动，这个程序仍然是可以正常安装的，帙shi卷你无法在启动器中看到或者打开这个程序。这种程勋一般都是作为第三方服务供其他的应用在内部调用的，如支付宝快捷支付服务。-->

        <activity android:name=".FirstActivity" 
            android:label="This is FirstActivity" >  
                
                <intent-filter> 
                    <action android:name="android.intent.action.MAIN" />

                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
        </activity>
    </application>
    ```
    
- 1.5 隐藏标题栏
    - requestWindowFeature(Window.FEATURE_NO_TITLE);（不过我在Android Studio中不管填补添加该句都没有标题栏==）
    
    ```java
    public class FirstActivity extends Activity {
        @Override
            protected void onCreate(Bundle savedInstanceState) {
                super.onCreate(savedInstanceState);
                requestWindowFeature(Window.FEATURE_NO_TITLE);//该句就是不在活动中显示标题栏。在setContentView之前执行，否则会报错
                setContentView(R.layout.first_layout);
            }
    }
    ```
- 1.6 在活动中使用toast
    - toast是Android系统提供的一种非常好的提醒方式，在程序中可以使用它将短小的信息通知给用户，这些信息会在一段时间后自动消失，并且不会占用任何屏幕空间。现在我们就来尝试一下如何在活动中使用toast吧。
    首先，需要定义一个弹出toast的触发点，正好界面有个按钮，我们就让点击这个按钮的时候弹出一个toast咯。

    ```java
    public class FirstActivity extends Activity {
        @Override
            protected void onCreate(Bundle savedInstanceState) {
                super.onCreate(savedInstanceState);
                requestWindowFeature(Window.FEATURE_NO_TITLE);//no use here
                setContentView(R.layout.first_layout);//加载布局
                //findViewById()获取到布局文件中定义的元素，这里传入R.id.button_1来得到按钮实例，该方法返回的是一个View对象，我们需要向下转型将它转为Button对象
                Button button1 = (Button) findViewById(R.id.button_1);
                //调用setOnClickListener()方法位按钮注册一个监听器，点击按钮时就会监听执行器中的onClick()方法。因此，弹出Toast的功能当然要在onClick()方法中编写了。
                button1.setOnClickListener(new View.OnClickListener() {
                        @Override
                        public void onClick(View view) {
                        //通过静态方法makeText()创建出一个Toast对象，然后调用show()将Toast显示出来就可以了。
                        //需要注意makeText()方法需要传入三个参数。
                        //1st参数是Context，也就是Toast要求的上下文，由于活动本身就是一个Context对象，因此这里栉jie风沐雨传入FirstActivity.this即可
                        //2nd参数是显示的文本内容
                        //3rd参数是Toast显示时长，有两个内置常量可以选择Toast.LENGTH_SHORT和Toast.LENGTH_LONG
                        Toast.makeText(FirstActivity.this, "you short pressed to click Button 1", Toast.LENGTH_SHORT).show();
                        Toast.makeText(FirstActivity.this, "you long pressed to click Button 1", Toast.LENGTH_LONG).show();
                        }
                        });
            }
    }
    ```

- 1.7 在活动中使用Menu
    - 手机屏幕有限，充分利用屏幕空间在手机界面设计中就显得非常重要。
    如果你的活动中有大量的菜单需要显示。这个时候界面设计就会比较尴尬，因为仅这些菜单就可能占用屏幕三分之一的空间，该咋办捏？
    - Android给我们提供了一种方式，可以让菜单都能得到展示的同时，还能不占用任何屏幕的空间。
    - step1 res/New->Folder，命名menu; res/menu->New->MenuResourceFile,命名main, 将Avaliable qualifiers选项改为Layout Direction(默认的是Country Code)，点击Finish完成创建。
    - step2 在res/menu/main.xml中添加如下代码：
    
    ```html
    <?xml version="1.0" encoding="utf-8"?>
    <menu xmlns:android="http://schemas.android.com/apk/res/android">
    <!-- <item>标签就是用来创建具体的菜单项，然后通过android:id给这个菜单指定一个唯一的标识符，通过android:tittle给这个菜单项指定一个名称 -->
    <item
        android:id="@+id/additem"
        android:title="AddItem"
        />
        <item
            android:id="@+id/removeitem"
            android:title="RemoveItem"
            />
    </menu>
   ```
   - step3 打开FirstActivity,重写onCreateOptionsMenu()方法：
   
   ```java
   @Override
   //重写onCreateOptioneMenu()方法
   //getMenuInflater()方法能够得到MenuInflater对象，再调用它的inflate()方法就可以给当前活动创建菜单
   //1st参数用于指定我们通过哪一个资源文件来创建菜单：R.menu.main；
   //2nd参数用于指定我们的菜单项将添加到哪一个Menu对象当中，这里直接使用onCreateOptionMenu()方法中传入的menu参数
   //然后给这个方法返回true，表示允许创建的菜单显示出来，如果返回false则创建的菜单将无法显示。
   public boolean onCreateOptionsMenu(Menu menu) {
       Log.d(TAG, "Enter create option func ("+menu+")");
       getMenuInflater().inflate(R.menu.main, menu);
       return true;
   }
   ```
   - step4 打开FirstActivity，重写onOptionsItemSelected()方法： 
   
   ```java
   //仅仅让菜单显示出来是不够的，我们定义菜单不仅是为了看的，关键是要菜单真正可用才行，因此还要再定义菜单相应事件。
   @Override
   public boolean onOptionsItemSelected(MenuItem item) {
       Log.d(TAG,"Enter option selected func ("+item+")");
       //通过item.getItemId()来判断我们点击的是哪一个菜单项，然后给每个菜单项加入自己的逻辑处理
       switch(item.getItemId()) {
           case R.id.additem:
               Log.d(TAG, "response to additem event ("+item.getItemId()+")");
               Toast.makeText(this, "you click add", Toast.LENGTH_SHORT).show();
               break;

           case R.id.removeitem:
               Log.d(TAG, "response to removeitem event");
               Toast.makeText(this, "you click remove", Toast.LENGTH_SHORT).show();
               break;
           default:

       }
       return true;
   }
   ```

- 1.8 销毁一个活动
    - 简单来讲，我们可以通过back键来销毁当前的活动；当然，也可以在程勋中通过代码来销毁活动。
    - Activity提供了一个finish()方法，我们在活动中调用一下这个方法就可以销毁当前活动了。修改按钮监听器中的代码如下：
  
    ```java
    /* 重新定义按钮监听器的方法，使之实现和back键一样的销毁活动功能
       button1.setOnClickListener(new View.OnClickListener() {
       @Override
       public void onClick(View view) {
       finish();
       }
       });
     */
    ```
    重新运行程序,这时点击一下按钮,当前的活动就被成功销毁了,效果和按下Back键是一样的


### 二、使用Intent在活动之间穿梭
只有一个活动的应用太简单了。不管你想创建多少个活动，方法都和第一章中介绍的一样。
唯一的问题在于，你在启动器中点击应用的图标只会进入到该应用的主活动，那么怎样才能由主活动跳转到其他活动呢？

- 2.1 使用显示Intent
    - step1 快速新建一个second_layout.xml布局，并定义一个按钮Button2。方法同第一章节的first_layout.xml
    - step2 新建活动SecondActivity继承自Activity，并加载布局
    - step3 在AndroidManifest.xml中为SecondActivity进行注册（因为SecondActivity不是主活动，所以不需要配置<intent-filter>）
    - step4 如何人去启动第二个活动呢？这里就需要引入一个新的概念--*Intent*
    *Intent是Android程序中各组件之间进行交互的一种重要方式，它不仅可以指明当前组件想要执行的动作，还可以在不同组件之间传递数据。
     Intent一般可被用于启动活动、启动服务、以及发送广播等场景，本章重点锁定启动活动上面。
     Intent方法大致可以分为两种：显式Intent和隐式Intent。先来看看显式：
    *
  
    ```java
    @Override
    public void onClick(View view) {
        /* Intent有多个构造函数的重载，其中一个如下是Intent(Contex packageContex, clase<?>cls)。
         * 1st参数Context是要求提供一个启动活动的上下文，即FirstActivity.this
         * 2nd参数Class是指定想要启动的目标活动,即SecondActivity.class
         * 通过这个构造函数，就可以构建出Intent的“意图”。
         * 如何使用这个Intent呢？
         * Activity类中提供了一个startActivity()方法，这个方法专门用于启动活动的，它接收一个Intent参数，
         * 这里我们将构建好的Intent传入startActivity()方法就可以启动目标活动了。
         * 即在FirstActivity这个活动的基础上打开SecondActivity这个活动，然后通过startActivity()方法来执行这个Intent.
         */
        Intent intent = new Intent(FirstActivity .this, SecondActivity.class);
        startActivity(intent);
    }
    ```
    重新运行程序，就可以成功启动SecondActivity这个活动了。

- 2.2 使用隐式Intent
    - 相比显式Intent，隐式Intent则含蓄了许多，它并不明确指出我们想要启动哪一个活动，而是指定了一系列更为抽象的action和category等信息，然后交由系统去分析这个Intent并帮忙找出合适的活动去启动。
    - 什么叫做合适的活动呢？简单来说就是可以响应我们这个隐式Inten的活动，目前SecondActivity可以响应什么样的隐式Intent呢？目前貌似什么都响应不了==。不过很快就有了。
    - step1 打开AndroidManifest.xml，通过在<activity>标签下配置<intent-filter>的内容，可以指定当前活动能够响应的action和category。
    
    ```html
    <!-- 因为SecondActivity不是主活动，所以不需要配置<intent-filter>标签里的内容 -->
    <activity android:name=".SecondActivity">
    <!-- 通过配置<intent-filter>可以指定当前活动能够响应的action和category -->
    <!-- <action>标签指明了当前活动可以响应gaojing.activitytest.ACTION_START这个action-->
    <!-- <category>标签则包含了一些附加信息，更精确地指明了当前的活动能够响应的Intent中还可能带有的category-->
    <!-- 只有<action>和<category>中的内容同时能够匹配上Intent中指定的action和Integory时，这个活动才能响应该Intent。如果一个act和cat不能响应Intent则程序崩溃-->
    <!-- 每个Intent中只能指定一个action，但却能指定多个category。 -->
        <intent-filter>
            <action android:name="gaojing.activitytest.ACTION_START" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="gaojing.activitytest.MY_CATEGORY" />
        </intent-filter>>
    </activity>
    ```

    - step2 修改FirstActivity中按钮的点击事件
    ```java
    /* 使用Intent的另一个构造函数，直接将action的字符串串了进去，表明我们想要启动能够响应...ACTION_START的这个action活动
     * 之所以没哟指定category是因为android.intent.category.DEFAULT是一种默认的category，
     * 在调用startActivity()方法的时候会自动将这个category添加到Intent中。                 *
     */
    Intent intent = new Intent("gaojing.activitytest.ACTION_START");//隐式Intent
    intent.addCategory("gaojing.activitytest.MY_CATEGORY");//添加一个自定义的category
    startActivity(intent);
    ```
    重新运行程序，点击按钮，同样能成功启动SecondActivity，证明我们在<activity>下配置的action和category的内容都已经生效了。

- 2.3 更多隐式Intent的用法
    - 使用隐式Intent，不仅可以启动自己程序内的活动，还可以启动其他程序的活动，这使得Android多个应用程序之间的功能共享成为了可能。
    - eg1: 比如你要调用一个网页，这时你不需要自己去实现一个浏览器，而是只需要调用系统的浏览器来打开这个网页就行了。
  
    ```java
    /* 首先指定Intent的action是Intent.ACTION_VIEW这是一个Android系统内置的动作，其常量值位android.intent.action.VIEW
     * 然后通过Uri.parse()方法，将一个网址字符串解析成一个Uri对象，再调用Intent的setData()方法将这个Uri对象传递进去
     * setData()方法，接收一个Uri对象，主要用于指定当前Intent正在操作的数据，
     * 而这些数据通常都是以字符串的形式传入到Uri.parse()方法中解析产生的。
     * 同时，我们还可以在<Intent-filter>标签中再配置一个<data>标签，用于更精确地指定当前活动能够响应什么类型的数据。
     * 只有当Intent中携带的Data和<data标签>中指定的内容完全一致时，当前活动才能够响应该Intent。
     * 一般不会在标签中做过多的指定，http即可。
     */
    Intent intent = new Intent(Intent.ACTION_VIEW);
    intent.setData(Uri.parse("http://www.baidu.com"));
    startActivity(intent);
    ```
    - eg2: 拨号
    
    ```java
    Intent intent = new Intent(Intent.ACTION_DIAL);
    intent.setData(Uri.parse("tel:10086"));
    startActivity(intent);
    ```

- 2.4 向下一个活动传递数据
    Intent不仅可以用来启动一个活动，还可以在启动活动的时候传递数据。Intent中提供了一系列的putExtra()方法重载，可以把我们想要传递的数据暂存在Intent中，启动了另一个活动后，只需要把这些数据再从Intent中取出就可以了。

    * FirstActivity *
    
    ```java
    /* 使用显示Intent来启动SecondActivity,并通过putExtra()方法来传递了一个字符串。
     * putExtra()：1st参数是键，用于后面从Intent中取值；2nd参数是真正要传递的数据。
     */
    String data = "Hello SecondActivity";
    Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
    intent.putExtra("extra_data", data);
    startActivity(intent);
    ```
    SecondActivity
    ```java
    setContentView(R.layout.second_layout);
    /* 通过getIntent()方法来获取到用于启动SecondActivity的Intent，
     * 然后调用getStringExtra()方法，传入相应的键值，就可以得到传递的数据了。getBooleanExtra()...
     */
    Intent intent = getIntent();
    String data1 = intent.getStringExtra("extra_data");
    Log.d("SecondActivity", data1);
    Log.d(TAG, "gaojing test intent transdata ("+data1+")");
    ```

- 2.5 返回数据给上一个活动
    只需要按一个back键就可以了，并没有一个用于启动活动Intent来传递数据。Activity中有一个方式是startActivityForResult(),即在活动销毁时能够返回一个结果给上一个活动。

    - SecondActivity
    ```java
    Button button2 = (Button) findViewById(R.id.button_2);
    button2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
            /* 先构建一个Intent,但是这个Intent没有指定任何意图，仅仅用来传递数据而已。
             * 紧接着要把传递的数据放在Intent中
             * 然后调用setResult()方法，专门用于向上一个活动返回数据
             * 1st参数用于向上一个活动返回处理结果，一般只用RESULT_OK/RESULT_CANCELED这两个值
             * 2nd参数是把带有数据的Intent传递回去
             * 最后调用finish()方法来销毁当前活动
             */
            Intent intent = new Intent();
            intent.putExtra("data_return", "Hello FirstActivity");
            setResult(RESULT_OK, intent);
            finish();
            }
            });
    ```

    - FirstActivity
    ```java
    /* 返回数据给上一个活动，方法startActivityForResult()
     * 1st参数是Intent; 2nd参数是请求码，用于在之后的回调中判断数据的来源(至uaoshi一个唯一值就好了)
     */
    Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
    startActivityForResult(intent, 1);


    /* 因为使用startActivityForResult()方法来启动SecondActivity的，
     * 在SecondActivity被销毁之后会回调上一个活动的onActivityResult()方法
     * 所以我们需要重写这个方法来得到返回的数据
     * 1st参数resquestCode,即在启动活动时传入的请求码；
     * 2nd参数resultCode,即在返回数据时传入的处理结果；
     * 3rd参数data，即携带着返回数据的Intent
     */
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        switch (requestCode) {
            case 1:
                if (resultCode == RESULT_OK) {
                    String returnedData = data.getStringExtra("data_return");
                    Log.d(TAG, "returndata ("+returnedData+")");
                }
                break;
            default:

        }
    }
    ```

### 三、活动的生命周期
- 3.1 返回栈
    - Android中的活动是可以叠层的。
    - Android是使用任务(Task)来管理活动的，一个任务就是一组存放在栈里的活动的集合，这个栈也被称作返回站(Back Stack)。
    - 栈是一种后进先出的数据结构，在默认情况下，每当我们启动了一个新的活动，它会在返回栈中入栈，并处于栈顶的位置。
    - 而每当我们按下Back键或调用finish()方法去销毁一个活动时，处于栈顶的活动会出栈，这时前一个入栈的活动就会重新处于栈顶的位置。
    - 系统总是会显示处于栈顶的活动给用户。下图展示了返回栈是如何管理活动入栈操作的。
    ![如何管理活动入栈操作](/public/img/android/如何管理活动入栈操作.png)


-3.2 活动状态
    * 每个活动在其生命周期中最多可能会有四种状态。*
    - 运行状态
        当一个活动位于返回栈的栈顶时，这时活动就处于运行状态。系统不会回收。
    - 暂停状态
        当一个活动不再处于栈顶，但仍然可见时。系统不会回收（只有在内存极低时才会考虑）
    - 停止状态
        当一个活动不再处于栈顶，并且完全不可见时，就进入了停止状态。（当其他地方需要内存时，有可能会被系统回收）
    - 销毁状态
        当一个活动从返回栈中移除后就变成了销毁状态。系统最倾向于回收处于这种状态的活动，从而保证手机的内存充足。

- 3.3 活动的生存周期
    * Activity类中定义了七个回调的方法，覆盖了活动生命周期的每一个环节。*
    - onCreate()活动第一次被创建的时候调用，在该方法中完成活动的初始化操作，比如加载布局，绑定事件等。
    - onStart() 在活动由不可见变为可见的时候调用。
    - onResume() 在活动准备好和用户进行交互的时候调用。此时活动一定位于返回栈的栈顶，并且处于运行状态。
    - onPause() 在系统准备去启动或者回复另一个活动的时候调用。通常会在这个方法中将一些枵hao腹从公CPU的资源释放掉，以及保存一些关键数据，但这个方法的执行速度一定要快，不然会影响到新的栈顶活动的使用。
    - onStop() 在活动完全不可见的时候调用。如果启动的新活动是一个对话框的活动，那么onPause()方法会得到执行，而onStop()方法并不会执行。
    - onDestroy() 在活动被销毁之前调用，之后活动的状态将变为销毁状态。
    - onRestart() 在活动由停止状态变为运行状态之前调用，也就是活动被重新启动了。
    除了onRestart()方法，其他都是两两相对，从而可以将活动分为三种生存期。
    - 1）完整生存期onCreate()完成各种初始化->onDestroy()完成释放内存操作
    - 2）可见生存期onStart()对资源进行加载->onStop()对资源进行释放，从而保证处于停止状态的活动不会占用过多内存。
    - 3）前台生存期onResum()->onPause()

- 3.4 体验活动的声明周期(code)
- 3.5 活动被回收了怎么办？
    - Activity中提供了一个onSaveInstanceState()回调方法，
    - 这个方法会保证一定在活动被回收之前调用，因此我们可以通过这个方法来解决活动被回收时临时数据得不到保存的问题。
    - onSaveInstanceState()，该方法会携带一个Bundle类型的参数，
    - Bundle提供了一系列的方法用于保存数据，比如可以使用putString()方法保存字符串，使用putInt()方法保存整形数据。。。
    - 每个保存方法需要传入两个参数，1st参数是键，用于后面从Bundle中取值；2nd参数是真正要保存的内容。
    - onCreate()方法和onSaveInstanceState()传入的参数都是Bundle类型。

- 3.6 活动的启动模式
    - 启动模式一共有四种：standard,singleTop,singleTask,singleInstance。可以在AndroidManifest.xml中通过给<activity>标签指定android:launchMode属性来选择启动模式。
- 1) standard
    ![活动启动模式standard](/public/img/android/活动启动模式-standard.png)
    在不进行显示指定的情况下，所有活动都会自动使用这种启动模式。每当启动一个新活动，它就会在返回栈中入栈，并处于栈顶的位置。
    对于这种模式的活动，系统不会在乎这个活动是否已经在返回栈中存在，每次启动都会创建该活动的一个新的实例。
    更改后重新运行程序，每点击一次按钮就会创建出新的FirstActivity实例，因此需要多按几次back才能退出程序。

- 2) singleTop
    在启动活动时，如果发现返回栈的栈顶已经是该活动，则认为可以直接使用它，不会再创建新的活动实例，仅按一次back键就能退出程序。
    但是，当FirstActivity并未处于栈顶位置时，这时再启动FirstActivity,还是会创建新的实例。
    ![活动启动模式-singleTop](/public/img/android/活动启动模式-singleTop.png)

- 3) singleTask
    使用singleTop可以很好地解决重复创建栈顶活动的问题，但是如果该活动并没有处于栈顶的位置，还是可能会创建多个活动实例的。
    若使用singleTask模式，每次启动该活动时，系统首先会在返回栈中检查是否存在该活动的实例，如果发现已经存在则直接使用该实例，并把这个活动之上的所有活动统统出栈，如果没有发现就会创建一个新的活动实例。即可以让某个活动在整个应用程序的上下文中只存在一个实例。
    ![活动启动模式-singleTask](活动启动模式-singleTask.png)

- 4) singleInstance
    该模式下的活动会启用一个新的返回栈来管理这个活动。假设我们的程序中有一个活动是允许其他程序调用的，如果我们想实现其他程序和我们的程序可以共享这个活动的实例，应该如何实现呢？
    前三种肯定做不到，因为每个应用程序都会有自己的返回栈，同一个活动在不同的返回栈中入栈时必然是创建了新的实例。而使用singleInstance模式就可以解决这个问题，该模式下hi有一个单独的返回栈来管理这个活动，不管是哪个应用程勋来访问这个活动，都共用同一个返回栈，也就解决了共享活动实例的问题。
    ![活动启动模式-singleInstance](活动启动模式-singleInstance.png)


- 3.6 活动的最佳实践
- 1）知晓当前是在哪一个活动？如何根据当前的界面判断出这是哪一个活动
    新建一个BaseActivity继承自Activity，并在BaseActivity中重写onCreate()方法去获取当前实例的类名，并通过Log.d打印出来；然后让BaseActivity成为ActivityTest项目中所有活动的父类。
    这样，每进入一个活动的界面，该活动的类名就会被打印出来，也就能够时时刻刻知晓当前界面对应的是哪个活动了。

- 2)随时随地退出程序(CODE)
- 3)启动活动的最佳写法
    首先通过Intent构建出当前的意图，然后调用startActivity()或startActivityForResult()方法将活动启动起来，如果有数据需要从一个活动传递到另一个活动，也可以借助Intent来完成。
    假设 SecondActivity 中需要用到两个非常重要的字符串参数,在启动 SecondActivity 的
    时候必须要传递过来,那么我们很容易会写出如下代码:
    ```java
    Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
    intent.putExtra("param1", "data1");
    intent.putExtra("param2", "data2");
    startActivity(intent);
    ```
    这样写是完全正确的,不管是从语法上还是规范上,只是在真正的项目开发中经常会有对接的问题出现。比如 SecondActivity 并不是由你开发的,但现在你负责的部分需要有启动
    SecondActivity 这个功能,而你却不清楚启动这个活动需要传递哪些数据。
    这时无非就有两种办法,一个是你自己去阅读 SecondActivity 中的代码,二是询问负责编写 SecondActivity的同事。你会不会觉得很麻烦呢?其实只需要换一种写法,就可以轻松解决掉上面的窘境。
    修改 SecondActivity 中的代码,如下所示:
    ```java
    public class SecondActivity extends BaseActivity {
        public static void actionStart(Context context, String data1, String data2) {
            Intent intent = new Intent(context, SecondActivity.class);
            intent.putExtra("param1", data1);
            intent.putExtra("param2", data2);
            context.startActivity(intent);
        }
       // ......
    }
    ```
    我们在 SecondActivity 中添加了一个 actionStart()方法,在这个方法中完成了 Intent 的构建,另外所有 SecondActivity 中需要的数据都是通过 actionStart()方法的参数传递过来的,然后把它们存储到 Intent 中,最后调用 startActivity()方法启动 SecondActivity。 
    这样写的好处在哪里呢?最重要的一点就是一目了然,SecondActivity 所需要的数据全部都在方法参数中体现出来了,这样即使不用阅读 SecondActivity 中的代码,或者询问负责
    编写 SecondActivity 的同事,你也可以非常清晰地知道启动 SecondActivity 需要传递哪些数据。另外,这样写还简化了启动活动的代码,现在只需要一行代码就可以启动 SecondActivity,
    如下所示:
    ```java
    button1.setOnClickListener(new OnClickListener() {
        @Override
        public void onClick(View v) {
            SecondActivity.actionStart(FirstActivity.this, "data1", "data2");
        }
    });
    ```
    养成一个良好的习惯,给你编写的每个活动都添加类似的启动方法,这样不仅可以让启
    动活动变得非常简单,还可以节省不少你同事过来询问你的时间。





