
AndroidManifest.xml 文件，是Android项目的系统清单文件，即整个Android应用的全局描述文件。用于控制Android应用的名称、图标、访问权限、包含的组件等整体属性。
R.java 该文件是由aapt工具根据应用中的资源文件来生成的，因此可将其理解成Android应用的资源词典。

layout
- android:id 给当前控件定义了一个唯一标识符
- android:layout_width 指定控件的宽度
- android:layout_heigth 指定控件的高度
    - match_parent 推荐。表示让当前控件的大小和父布局的大小一样，也就是由父布局来决定当前控件的大小。
    - fill_parent
    - wrap_content 表示让当前的控件大小能够刚好包含住里面的内容，也就是由控件内容决定当前控件的大小。
    - 也可自行指定宽高，但是可能会在不同手机屏幕的适配方面出现问题。
- android:text 指定了TextView中显示的文本内容
- <TextView /> 控件，用于在界面上显示一段文本信息
- android:gravity 指定文字的对齐方式，可用“|”来同时指定多个值
- anddroid:textSize 指定文字大小
- android:textColor 指定文字颜色

- <EditText />
- android:hint 指定一段提示性文本（一旦输入就消失）
- android:maxLines 指定EditText的最大行数（比如2，两行,超过两行就会自动向上滚动）

- findByViewId()方法 获取EditText
- getText()方法 获取到输入的内容
- toString()方法转换成字符串
- Toast
- editText = (EditText) findViewById(R.id.edit_text);
- String inputText = editText.getText().toString();
- Toast.makeText(MainActivity.this, inputText, Toast.LENGTH_SHORT).show();

- <ImageView />
- android:src 给ImageView指定一张图片
- imageView.setImageResource(R.drawable.expand); 将显示的图片改为expand


- <ProgressBar />


- android:visibility 安卓控件可见属性，可选值visible,invisible,gone
- visible:表示控件可见，默认值，即不指定时控件都是可见的
- invisible:表示控件不可见，但是它仍然占据着原来的位置和大小，可理解成控件变成透明装填了。
- gone:表示控件不仅不可见，而且不再占用任何屏幕空间。
- setVisibility()方法，可以设置控件的可见性，传入参数View.VISIBLE/View.INVISIBLE/View.GONE
- getVisibility()方法，获取控件属性
- if (progressBar.getVisibility() == View.GONE)
- progressBar.setVisibility(View.VISIBLE);
- android:style 指定进度条属性，默认圆形，可成水平
- style="?android:attr/progressBarStyleHorizontal"
- android:max="100" 给进度条设置一个最大值
    ```java
    int progress = progressBar.getProgress();
    progress = progress + 10;
    progressBar.setProgress(progress);
    ```

- AlertDialog相关
```java
AlertDialog.Builder dialog = new AlertDialog.Builder(MainActivity.this);
dialog.setTitle("This is Dialog");
dialog.setMessage("somthing important");
dialog.setCancelable(false);
dialog.setPositiveButton("OK", new DialogInterface.OnClickListener() {
        @Override
        public void onClick(DialogInterface dialogInterface, int i) {

        }
        });
dialog.setNegativeButton("cancle", new DialogInterface.OnClickListener() {
        @Override
        public void onClick(DialogInterface dialogInterface, int i) {

        }
        });
dialog.show();
```

- ProgressDialog相关



- android:layout_gravity="top" "center_vertical" "bottom" 用于指定控件在布局中的对齐方式
- android:layout_weigth 可以使用比例的方式来指定控件的大小，在手机屏幕的是配型方面可以起到非常重要的作用
- android:layout_alignParentBottom alignParentTop alignParentRight alignParentLeft
- android:layout_ablove below toLeftOf toRightOf
- android:layout_alignLeft alignRight alignTop alignBottom 让一个控件的左边缘和另一个控件的左边缘对其

- android:stretchColumns="1" 表示如果表格不能完全占满屏幕宽度，就将第二行拉伸。指定0就拉伸第一列，指定2就拉伸第三列。

- adndroid:orientation 排列方向
- <include layout="@layout/tittle" /> 将标题栏布局引进来
