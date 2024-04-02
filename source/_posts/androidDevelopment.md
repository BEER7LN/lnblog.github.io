---
title: “山东大学移动互联网开发技术教学网站建设”项目实训日志
date: 2021-05-16
tags: 安卓开发
---

## 1. 日志一

##### 时间：

21春季学期第三周

---
##### 个人工作内容：

- 设计个人负责的3个案例App（入门Hello World；基于磁场传感器的指南针；基于陀螺仪的“晃动取消闹钟”）的功能结构
- 设计8个案例App的视觉稿

---
##### 详细记录：

- 功能结构设计：

  - Hello World：

![alt text](1.png)


  - 指南针：

![alt text](2.png)

  - “晃动取消闹钟”

![alt text](3.png)

- 视觉稿：
  
  - 指南针
  
![alt text](4.png)

  
  - “晃动取消闹钟”
  
![alt text](5.png)
![alt text](6.png)
![alt text](7.png)

  
  - GPS定位
  
![alt text](8.png)
![alt text](9.png)
  
  - 音乐播放器
  
![alt text](10.png)
![alt text](11.png)
  
  - webview
  
![alt text](12.png)
![alt text](13.png)
  
  - 手势操作游戏
  
![alt text](14.png)
![alt text](15.png)
![alt text](16.png)
  
  - 图片抗扭曲
  
![alt text](17.png)
![alt text](18.png)

  
  - 飞花令
  
![alt text](19.png)
![alt text](20.png)


##### 后期工作规划：

- 下周完成Hello World案例App的开发和教学设计

## 2. 日志二

##### 时间：

​	21春季学期第四、五周

---

##### 个人工作内容：

​	完成Hello World案例app

---

##### 详细记录：

- Hello World案例app设计说明
  - 目标用户：有一定java基础的初次接触android开发的学生。
  - 需求说明：简述项目开发流程，工程相关解析，介绍android开发基础内容。
  - 设计说明：android中常用的LinearLayout(线性布局)和RelativeLayout(相对布局)，android中常用的UI控件，其中穿插Toast（提示信息的一个控件）、基于监听的事件处理机制等内容。后续内容在其它专题案例app中借实例讲解。
##### app页面展示：

![alt text](21.jpg)

- 多个按钮可以用switch来设置监听事件
```
private void setListeners(){
        OnClick toclick = new OnClick();
        mButtonLlay.setOnClickListener(toclick);
```
```
private class OnClick implements View.OnClickListener{
        @Override
        public void onClick(View v) {
            Intent intent = null;
            switch (v.getId()){
                case R.id.btn_llay:
                    intent = new Intent(MainActivity.this, LinearLayoutActivity.class);
                    break;
```

- 线性布局：

![alt text](22.jpg)

	需要设置orientation（线性方向，水平or垂直）；
线性布局可以嵌套，从而设计出美观的布局；
weight设置View比重；
在线性布局中layout_gravity可以让组件设置靠左右/上下（垂直布局/水平布局）。
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">

    <TextView
        android:layout_width="130dp"
        android:layout_height="fill_parent"
        android:background="#84a9ac">
    </TextView>
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="fill_parent"
        android:background="#a3d2ca"
        android:orientation="vertical"
        android:paddingTop="80dp"
        android:paddingLeft="20dp"
        android:paddingRight="20dp"
        android:paddingBottom="30dp">

        <TextView
            android:layout_width="match_parent"
            android:layout_height="40dp"
            android:background="#eeeeee"/>
        <TextView
            android:layout_width="100dp"
            android:layout_height="40dp"
            android:background="#eeeeee"
            android:layout_gravity="right"
            android:layout_marginTop="20dp"
            android:layout_marginBottom="40dp"/>
        <TextView
            android:layout_weight="1"
            android:layout_width="match_parent"
            android:layout_height="fill_parent"
            android:background="#fde2e2"/>
        <TextView
            android:layout_weight="1"
            android:layout_width="match_parent"
            android:layout_height="fill_parent"
            android:background="#ffb6b6"/>
    </LinearLayout>
</LinearLayout>
```

- 相对布局

![alt text](23.jpg)

相对布局即相对概念，在一个View的基础上，可以将其它View放在其周围或者其内部的上下左右等地方。

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/RelativeLayout1"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="30dp">

    <TextView
        android:id="@+id/img0"
        android:layout_width="120dp"
        android:layout_height="120dp"
        android:background="@color/orange" />

    <TextView
        android:layout_width="30dp"
        android:layout_height="30dp"
        android:layout_alignLeft="@id/img0"
        android:background="@color/teal_200"/>
    <TextView
        android:layout_width="30dp"
        android:layout_height="30dp"
        android:layout_alignRight="@id/img0"
        android:background="@color/teal_200"/>
    <TextView
        android:layout_width="30dp"
        android:layout_height="30dp"
        android:layout_alignLeft="@id/img0"
        android:background="@color/teal_200"/>
    <TextView
        android:layout_width="30dp"
        android:layout_height="30dp"
        android:layout_alignBottom="@id/img0"
        android:background="@color/teal_200"/>
    <TextView
        android:layout_width="30dp"
        android:layout_height="30dp"
        android:layout_alignBottom="@id/img0"
        android:layout_alignRight="@id/img0"
        android:background="@color/teal_200"/>

    <TextView
        android:layout_width="20dp"
        android:layout_height="20dp"
        android:layout_centerHorizontal="true"
        android:background="@color/orange" />

    <TextView
        android:layout_width="20dp"
        android:layout_height="20dp"
        android:layout_centerHorizontal="true"
        android:background="@color/orange"
        android:layout_alignParentBottom="true"/>

    <TextView
        android:layout_width="20dp"
        android:layout_height="20dp"
        android:layout_centerVertical="true"
        android:background="@color/orange" />

    <TextView
        android:layout_width="20dp"
        android:layout_height="20dp"
        android:layout_centerVertical="true"
        android:background="@color/orange" />

    <TextView
        android:layout_width="20dp"
        android:layout_height="20dp"
        android:layout_centerVertical="true"
        android:layout_alignParentRight="true"
        android:background="@color/orange" />

    <TextView
        android:layout_width="50dp"
        android:layout_height="50dp"
        android:layout_alignParentRight="true"
        android:background="@color/orange" />

    <TextView
        android:layout_width="50dp"
        android:layout_height="50dp"
        android:layout_alignParentRight="true"
        android:layout_alignParentBottom="true"
        android:background="@color/orange" />

    <TextView
        android:layout_width="50dp"
        android:layout_height="50dp"
        android:layout_alignParentLeft="true"
        android:layout_alignParentBottom="true"
        android:background="@color/orange" />
    <!-- 这个是在容器中央的 -->

    <TextView
        android:id="@+id/img1"
        android:layout_width="80dp"
        android:layout_height="80dp"
        android:layout_centerInParent="true"
        android:background="@color/blue1"/>

    <!-- 在中间图片的左边 -->
    <TextView
        android:id="@+id/img2"
        android:layout_width="80dp"
        android:layout_height="80dp"
        android:layout_toLeftOf="@id/img1"
        android:layout_centerVertical="true"
        android:background="@color/blue"/>

    <!-- 在中间图片的右边 -->
    <TextView
        android:id="@+id/img3"
        android:layout_width="80dp"
        android:layout_height="80dp"
        android:layout_toRightOf="@id/img1"
        android:layout_centerVertical="true"
        android:background="@color/gray"/>

    <!-- 在中间图片的上面-->
    <TextView
        android:id="@+id/img4"
        android:layout_width="80dp"
        android:layout_height="80dp"
        android:layout_above="@id/img1"
        android:layout_centerHorizontal="true"
        android:background="@color/red"/>

    <!-- 在中间图片的下面 -->
    <ImageView
        android:id="@+id/img5"
        android:layout_width="80dp"
        android:layout_height="80dp"
        android:layout_below="@id/img1"
        android:layout_centerHorizontal="true"
        android:src="@color/yellow"/>

</RelativeLayout>
```

- TextView

![alt text](24.jpg)

TextView可以说时最最最基础的组件，需要掌握其常用属性。

```
    //设置背景，drawable中shape可以设置边框，填充，圆角等,文本的下划线需要在activity中设置
    <TextView
        android:id="@+id/tv_1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@drawable/ttv_bg"
        android:text="@string/tvs_1"
        android:textColor="@color/red"
        android:textSize="32sp"
        android:textStyle="bold|italic"
        android:gravity="center"/>

    //长文本的单行显示效果，android:maxLines="1"是设置为单行，android:ellipsize="end"文本超出时显示省略号
    <TextView
        android:id="@+id/tv_2"
        android:layout_width="300dp"
        android:layout_height="wrap_content"
        android:text="@string/tvs_2"
        android:maxLines="1"
        android:ellipsize="end"
        android:layout_marginTop="30dp"/>

    //icon+文字
    <TextView
        android:id="@+id/tv_3"
        android:layout_width="wrap_content"
        android:layout_height="60dp"
        android:text="@string/tvs_3"
        android:drawableLeft="@mipmap/icon_user"
        android:drawablePadding="10dp"
        android:gravity="center"
        android:textSize="24sp"
        android:layout_marginTop="30dp"/>

    //阴影效果
    <TextView
        android:id="@+id/tv_4"
        android:layout_width="352dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="30dp"
        android:textStyle="bold"
        android:textColor="@color/blue"
        android:text="@string/tvs_4"
        android:layout_centerInParent="true"
        android:shadowColor="#F9F900"
        android:shadowDx="10.0"
        android:shadowDy="10.0"
        android:shadowRadius="3.0"
        android:textSize="30sp"
        android:gravity="center"/>

    //链接，可以从网页直接返回
    <TextView
        android:id="@+id/tv_5"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/tvs_5"
        android:textSize="24sp"
        android:autoLink="web"
        android:layout_marginTop="30dp"/>

    //跑马灯效果，常用于提示
    <TextView
        android:id="@+id/tv_6"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="@string/tvs_6"
        android:singleLine="true"
        android:scrollHorizontally="true"
        android:ellipsize="marquee"
        android:marqueeRepeatLimit="marquee_forever"
        android:focusable="true"
        android:focusableInTouchMode="true"
        android:layout_marginTop="30dp">
        <requestFocus/>
    </TextView>
```

- Button

![alt text](25.jpg)

Button的背景设置为drawable中的xml，用法同前面TextView的背景；
Button可以设置监听事件，如点击提交，登录等。
```
	<Button
        android:id="@+id/btn1"
        android:layout_width="match_parent"
        android:layout_height="64dp"
        android:text="@string/btn_1"
        android:textColor="@color/gray"
        android:textStyle="bold"
        android:textSize="20sp"
        android:background="@drawable/btn1_bg"/>

    <Button
        android:id="@+id/btn2"
        android:layout_width="match_parent"
        android:layout_height="64dp"
        android:textSize="20sp"
        android:text="@string/btn_2"
        android:layout_below="@+id/btn1"
        android:textColor="@color/blue"
        android:background="@drawable/btn2_bg"
        android:layout_marginTop="20dp"/>

    <Button
        android:id="@+id/btn3"
        android:layout_width="match_parent"
        android:layout_height="64dp"
        android:layout_below="@+id/btn2"
        android:layout_marginTop="20dp"
        android:background="@drawable/btn3_bg"
        android:onClick="showToast"
        android:text="@string/btn_3"
        android:textColor="@color/white"
        android:textSize="20sp" />
```

- EditText

![alt text](26.jpg)

EditText是用户编辑区，除了像TextView控制字体等属性外，还可以控制用户输入格式，控制输入框大小，监听输入信息等，是与用户交互的重要组件。

```
<EditText
        android:id="@+id/edt1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="16sp"
        android:textColor="@color/gray"
        android:hint="用户名"
        android:drawableLeft="@mipmap/icon_user"
        android:drawablePadding="10dp"
        android:textColorHint="@color/teal_700"
        android:singleLine="true" />

    <EditText
        android:id="@+id/edt2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="16sp"
        android:textColor="@color/gray"
        android:layout_below="@id/edt1"
        android:hint="电话，获取焦点即可全选"
        android:inputType="number"
        android:selectAllOnFocus="true"/>

    <EditText
        android:id="@+id/edt3"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="16sp"
        android:textColor="@color/gray"
        android:layout_below="@id/edt2"
        android:hint="密码"
        android:inputType="textPassword"/>

    <EditText
        android:id="@+id/edt4"
        android:layout_width="match_parent"
        android:layout_height="120dp"
        android:textSize="16sp"
        android:textColor="@color/gray"
        android:layout_below="@id/edt3"
        android:background="@drawable/btn2_bg"
        android:hint="自我介绍"
        android:paddingLeft="15dp"
        android:paddingRight="15dp"
        android:layout_marginTop="40dp"
        android:maxLines="4"/>

    <EditText
        android:id="@+id/edt5"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="16sp"
        android:textColor="@color/gray"
        android:layout_below="@id/edt4"
        android:hint="输入监听"
        android:layout_marginTop="20dp"/>
```

- RadioButton

![alt text](27.jpg)

单选按钮需要在一个RadioGroup中才能实现单选效果；
可以改变样式或设置监听事件；
常用于表单设计中。

```
<RadioGroup
        android:id="@+id/radioGroup1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <RadioButton
            android:id="@+id/radiobtn1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="男"/>

        <RadioButton
            android:id="@+id/radiobtn2"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="女"/>

    </RadioGroup>

    <TextView
        android:id="@+id/rbcb_tv1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="还未选择"
        android:layout_marginTop="10dp"/>

    <RadioGroup
        android:id="@+id/radioGroup2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:layout_marginTop="40dp">

        <RadioButton
            android:id="@+id/radiobtn3"
            android:layout_width="60dp"
            android:layout_height="40dp"
            android:text="男"
            android:textColor="@color/white"
            android:textSize="24sp"
            android:button="@null"
            android:gravity="center"
            android:background="@drawable/rb_bg"/>

        <RadioButton
            android:id="@+id/radiobtn4"
            android:layout_width="60dp"
            android:layout_height="40dp"
            android:text="女"
            android:textColor="@color/white"
            android:textSize="24sp"
            android:button="@null"
            android:layout_marginLeft="20dp"
            android:gravity="center"
            android:background="@drawable/rb_bg"/>

    </RadioGroup>

    <Button
        android:id="@+id/rdidbtn1"
        android:layout_width="150dp"
        android:layout_height="wrap_content"
        android:text="提交"
        android:layout_marginTop="10dp"/>
```

- CheckBox

![alt text](28.jpg)

复选框可以多选，常用于表单设计中。
```
<CheckBox
        android:id="@+id/cb1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="西瓜"
        android:textSize="28sp"
        android:button="@drawable/cb_bg"
        android:paddingLeft="20dp"/>

    <CheckBox
        android:id="@+id/cb2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="梨"
        android:textSize="28sp"
        android:layout_marginTop="15dp"
        android:button="@drawable/cb_bg"
        android:paddingLeft="20dp"/>

    <CheckBox
        android:id="@+id/cb3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="苹果"
        android:textSize="28sp"
        android:layout_marginTop="15dp"
        android:button="@drawable/cb_bg"
        android:paddingLeft="20dp"/>

    <CheckBox
        android:id="@+id/cb4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="物理"
        android:layout_marginTop="30dp"/>

    <CheckBox
        android:id="@+id/cb5"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="化学"
        android:layout_marginTop="5dp"/>

    <CheckBox
        android:id="@+id/cb6"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="生物"
        android:layout_marginTop="5dp"/>

    <CheckBox
        android:id="@+id/cb7"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="全选"
        android:layout_marginTop="5dp"/>

    <Button
        android:id="@+id/rdidbtn2"
        android:layout_width="100dp"
        android:layout_height="wrap_content"
        android:text="确认"/>
```
在activity中监听到“全选”选中后改变同组CheckBox选中状态实现全选效果
```
@Override
    public void onCheckedChanged(CompoundButton compoundButton, boolean b) {
        if(b){
            if(compoundButton.getId() == R.id.cb7){
                cbx4.setChecked(true);
                cbx5.setChecked(true);
                cbx6.setChecked(true);
            }
            else
                Toast.makeText(this,compoundButton.getText().toString(),Toast.LENGTH_SHORT).show();
        }
        else {
            if(compoundButton.getId() == R.id.cb7){
                cbx4.setChecked(false);
                cbx5.setChecked(false);
                cbx6.setChecked(false);
            }
        }
    }
```

- ImageView

![alt text](29.jpg)

ImageView是插入图片的组件，可以设置图片大小和布局，常用属性如下：
```
<LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:padding="20dp"
        android:gravity="center">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="fitCenter(fitStart,fitEnd):保持图片的横纵比缩放,直到该图片能够显示在ImageView组件上,并将缩放好的图片显示在imageView的中间(左上，右下)"/>

        <ImageView
            android:layout_width="200dp"
            android:layout_height="100dp"
            android:scaleType="fitCenter"
            android:background="@color/teal_200"
            android:src="@drawable/picture1"
            android:layout_marginTop="5dp"/>

        <ImageView
            android:layout_width="200dp"
            android:layout_height="100dp"
            android:layout_marginTop="10dp"
            android:background="@color/teal_200"
            android:scaleType="fitStart"
            android:src="@drawable/picture1" />

        <ImageView
            android:layout_width="200dp"
            android:layout_height="100dp"
            android:background="@color/teal_200"
            android:scaleType="fitEnd"
            android:src="@drawable/picture1"
            android:layout_marginTop="10dp"/>

        <View
            android:layout_width="match_parent"
            android:layout_height="1px"
            android:background="#000000"
            android:layout_marginTop="15dp"/>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="15dp"
            android:text="matrix(center):默认值，不改变原图的大小，从ImageView的左上角（中间）开始绘制原图，原图超过ImageView的部分作裁剪处理"/>

        <ImageView
            android:layout_width="40dp"
            android:layout_height="40dp"
            android:background="@color/teal_200"
            android:scaleType="matrix"
            android:src="@drawable/picture1"
            android:layout_marginTop="5dp"/>

        <ImageView
            android:layout_width="200dp"
            android:layout_height="100dp"
            android:background="@color/teal_200"
            android:scaleType="center"
            android:src="@drawable/amy"
            android:layout_marginTop="10dp"/>

        <View
            android:layout_width="match_parent"
            android:layout_height="1px"
            android:background="#000000"
            android:layout_marginTop="15dp"/>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="15dp"
            android:text="fitXY:对图像的横向与纵向进行独立缩放,使得该图片完全适应ImageView,但是图片的横纵比可能会发生改变" />

        <ImageView
            android:layout_width="200dp"
            android:layout_height="100dp"
            android:background="@color/teal_200"
            android:scaleType="fitXY"
            android:src="@drawable/amy"
            android:layout_marginTop="5dp"/>

        <View
            android:layout_width="match_parent"
            android:layout_height="1px"
            android:background="#000000"
            android:layout_marginTop="15dp"/>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="15dp"
            android:text="centerCrop:保持横纵比缩放图片,直到完全覆盖ImageView,可能会出现图片的显示不完全"/>

        <ImageView
            android:layout_width="200dp"
            android:layout_height="100dp"
            android:background="@color/teal_200"
            android:src="@drawable/amy"
            android:scaleType="centerCrop"
            android:layout_marginTop="5dp"/>

        <View
            android:layout_width="match_parent"
            android:layout_height="1px"
            android:background="#000000"
            android:layout_marginTop="15dp"/>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="15dp"
            android:text="centerInside:保持横纵比缩放图片,直到ImageView能够完全地显示图片"/>

        <ImageView
            android:layout_width="200dp"
            android:layout_height="100dp"
            android:background="@color/teal_200"
            android:src="@drawable/amy"
            android:scaleType="centerInside"
            android:layout_marginTop="5dp"/>
    </LinearLayout>
```

- ScrollView

![alt text](30.jpg)

ScrollView可以是垂直滚动条或水平滚动条；
重写onClick方法实现滚动条滚动效果；
常用于信息浏览时返回顶部刷新。

xml：
```
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/scroll">
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:padding="20dp">

        <Button
            android:id="@+id/btn_down"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="最下方"
            android:textSize="20dp"
            android:textAllCaps="false"
            android:layout_marginTop="20dp"/>
        <Button
            android:id="@+id/btn_up"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="最上方"
            android:textSize="20dp"
            android:textAllCaps="false"
            android:layout_marginTop="400dp"/>
        <HorizontalScrollView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content">
            <LinearLayout
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:orientation="horizontal">
                <TextView
                    android:layout_width="200dp"
                    android:layout_height="300dp"
                    android:text="内容1"
                    android:padding="10dp"/>
                <View
                    android:layout_width="1dp"
                    android:layout_height="300dp"
                    android:background="@color/black"/>
                <TextView
                    android:layout_width="200dp"
                    android:layout_height="300dp"
                    android:text="内容2"
                    android:padding="10dp"/>
                <View
                    android:layout_width="1dp"
                    android:layout_height="300dp"
                    android:background="@color/black"/>
                <TextView
                    android:layout_width="200dp"
                    android:layout_height="300dp"
                    android:text="内容3"
                    android:padding="10dp"/>

            </LinearLayout>
        </HorizontalScrollView>

    </LinearLayout>

</ScrollView>
```
```
@Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.btn_down:
                scrollView.fullScroll(ScrollView.FOCUS_DOWN);
                break;
            case R.id.btn_up:
                scrollView.fullScroll(ScrollView.FOCUS_UP);
                break;
        }
    }
```
---

##### 后期工作规划：

- 下周完成指南针案例App的开发和教学设计


## 3. 日志三

##### 时间：
​	21春季学期第六周

---

##### 个人工作内容：
​	完成指南针案例app

​	全部视觉稿的组件和配色导出

---

##### 详细记录：

- 指南针案例app设计说明
  - 目标用户：android开发入门阶段的学生。
  - 需求说明：说明传感器的使用方法，动画使用简单教学
  - 设计说明：用一个指针盘表示朝向北的指针，传感器获取偏离角度后使得指针盘旋转并标出偏差角度，接近北方后字体变红以强调方位正确性
#####  要点说明
- 指南针页面

![alt text](31.jpg)

xml文件：
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical"
    android:gravity="center"
    android:background="#211E1E">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="NORTH"
        android:textColor="#FFFFFF"
        android:textSize="50sp" />
    <ImageView
        android:id="@+id/imageView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/compass3"
        android:layout_marginTop="60dp"/>
    <TextView
        android:id="@+id/degree"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="0°"
        android:textColor="#FFFFFF"
        android:textSize="50sp"
        android:layout_marginTop="60dp"/>

</LinearLayout>
```
- 当接近北方时，角度指示会变红，在传感器监测方向改变的方法中判断度数即可实现

![alt text](32.jpg)

SensorListener内部类：
```
 		//通过getSystemService获得SensorManager实例对象
        manager = (SensorManager) getSystemService(Context.SENSOR_SERVICE);
        
		private final class SensorListener implements SensorEventListener {
        private float predegree = 0;

        //传感器监测方向改变
        public void onSensorChanged(SensorEvent event) {
            float degree = event.values[0];// 存放了方向值 90
```
动画，在上传感器监测方向改变的方法中写：
```
RotateAnimation animation = new RotateAnimation(predegree, -degree,
                    Animation.RELATIVE_TO_SELF, 0.5f,
                    Animation.RELATIVE_TO_SELF, 0.5f);
            //旋转过程持续时间
            animation.setDuration(200);
            //罗盘图片使用旋转动画
            imageView.startAnimation(animation);
            predegree = -degree;
```


---



##### 后期工作规划：

- 下周完成摇摇闹钟案例App的开发和教学设计

## 4. 日志四

##### 时间：
​	21春季学期第八周

---

##### 个人工作内容：
​	完成摇摇闹钟案例app

---

##### 详细记录：

- 指南针案例app设计说明
  - 目标用户：android开发入门阶段的学生。
  - 需求说明：说明加速度传感器的使用方法，讲解AlarmManager闹钟服务基础，讲解activity间信息传递，还有播放背景音乐、震动。
  - 设计说明：用户可以自己设置闹钟，到点后闹钟响铃并震动，需要摇晃手机30次才能关闭闹钟。


##### 要点说明：
页面绘制、细节判断等内容将略过，只简述要点。

- 首页

![alt text](33.jpg)

对“07:00”TextView设置点击事件，调转到子activity（设置闹钟时间）：
```
thetime.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent1 = new Intent(MainActivity.this, selecttime.class);
//                startActivity(intent1);
                startActivityForResult(intent1,10);
            }
        });
```
- 设置闹钟时间

![alt text](34.jpg)

xml文件中TimePicker组件：
```
<TimePicker
        android:id="@+id/ttime"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
```
activity中监听TimePicker取值变化：
```
		//设置是否24小时制显示
        mTimepicker.setIs24HourView(true);
        //禁用键盘输入
        mTimepicker.setDescendantFocusability(TimePicker.FOCUS_BLOCK_DESCENDANTS);
        mTimepicker.setOnTimeChangedListener(new TimePicker.OnTimeChangedListener() {
            @Override
            public void onTimeChanged(TimePicker view, int hourOfDay, int minute) {
//                Toast.makeText(selecttime.this,hourOfDay+"时"+minute+"分",Toast.LENGTH_SHORT).show();
                thehour = hourOfDay;
                themin = minute;
            }
        });
```
至此可以获取到用户选择的闹钟时间，下一步是将时间传递给首页的activity
- 父子activity数据传递

主页activity中代码（同时注意改变主页时间TextView）：
```
@Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if(10 == requestCode){
            Bundle bundle=data.getExtras();
            if(bundle.getInt("flag") == 1){
                cgdhour = bundle.getInt("hour");
                cgdmin = bundle.getInt("minute");}}}
```
子activity中代码：
```
changetm.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent();
                Bundle bundle=new Bundle();
                bundle.putInt("flag", 1);
                bundle.putInt("hour", thehour);
                bundle.putInt("minute", themin);
                intent.putExtras(bundle);
                setResult(RESULT_OK, intent);
                finish();
            }
        });
```
- AlarmManager闹钟服务

![alt text](35.jpg)

```
		am = (AlarmManager) getSystemService(Service.ALARM_SERVICE);
        intent = new Intent(MainActivity.this, ring.class);
        pendingIntent = PendingIntent.getActivity(this,0,intent,0);
```
对Switch组件设置监听：
```
thebtn.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
                if (isChecked){
                    if(isoutdue(cgdhour,cgdmin) == 1)
                        Toast.makeText(MainActivity.this,"闹钟已开启,将于明日"+thetime.getText()+"响铃",Toast.LENGTH_SHORT).show();
                    else
                        Toast.makeText(MainActivity.this,"闹钟已开启,将于今日"+thetime.getText()+"响铃",Toast.LENGTH_SHORT).show();
                    setAlarm();
                }else {
                    Toast.makeText(MainActivity.this,"闹钟已关闭",Toast.LENGTH_SHORT).show();
                    cancelAlarm();
                }
            }
        });
```
开启闹钟：
```
private void setAlarm(){
        //android Api的改变不同版本中设置有所不同
        if(Build.VERSION.SDK_INT<19){
            am.set(AlarmManager.RTC_WAKEUP,getTimeDiff(cgdhour,cgdmin),pendingIntent);
        }else{
            am.setExact(AlarmManager.RTC_WAKEUP,getTimeDiff(cgdhour,cgdmin),pendingIntent);
        }
    }
```
关闭闹钟：
```
 private void cancelAlarm(){ am.cancel(pendingIntent); }
```
获取set(int type,long startTime,PendingIntent pi)中参数startTime：
```
public long getTimeDiff(int a, int b){
        //获得Calendar这个类的实例
        Calendar ca=Calendar.getInstance();
        //注意精确到秒，时间可以自由设置
        //设置为明日
        if(isoutdue(a,b) == 1){
            ca.set(Calendar.DATE,ca.get(Calendar.DATE)+1);
            ca.set(Calendar.HOUR_OF_DAY,a);
            ca.set(Calendar.MINUTE,b);
            ca.set(Calendar.SECOND,0);
        }
        else {
            ca.set(Calendar.HOUR_OF_DAY,a);
            ca.set(Calendar.MINUTE,b);
            ca.set(Calendar.SECOND,0);
        }
        return ca.getTimeInMillis();
    }
```
- 响铃页面

![alt text](36.jpg)

设置背景音乐：
```
		intent = new Intent(ring.this, MyIntentService.class);
        String action = MyIntentService.ACTION_MUSIC;
        intent.setAction(action);
        startService(intent);
```
震动：
```
vibrator = (Vibrator)this.getSystemService(this.VIBRATOR_SERVICE);
        long[] patter = {0, 1000, 0, 1000};
        vibrator.vibrate(patter, 1);
```

加速度传感器相关设置：

```
  mSensorManager.registerListener(this,   
  mSensorManager.getDefaultSensor(Sensor.TYPE_ACCELEROMETER),SensorManager.SENSOR_DELAY_NORMAL); 
  mSensorManager = (SensorManager) getSystemService(SENSOR_SERVICE);
```
继承SensorEventListener接口时需要实现方法：
```
    @Override
    public void onSensorChanged(SensorEvent event) {
        // TODO Auto-generated method stub
        int sensorType = event.sensor.getType();
        //values[0]:X轴，values[1]：Y轴，values[2]：Z轴
        float[] values = event.values;
        if(sensorType == Sensor.TYPE_ACCELEROMETER){
            /*因为一般正常情况下，任意轴数值最大就在9.8~10之间，只有在你突然摇动手机
             *的时候，瞬时加速度才会突然增大或减少。
             *所以，经过实际测试，只需监听任一轴的加速度大于14的时候，改变你需要的设置就OK了~~~
             */
            if((Math.abs(values[0])>14||Math.abs(values[1])>14||Math.abs(values[2])>14)){
                //事件
                i--;
                changei();
                if(i == 0)
                	//摇晃完成，关闭页面，同时关闭震动和背景音乐
                    finish();
            }
        }
    }
```

---


##### 后期工作规划：

- 下周开始教学网站的建设

## 5. 日志五

##### 时间：
​	21春季学期第九周



---



##### 个人工作内容：
​	网站页面设计



---



##### 详细记录：
- 网页设计说明：
目录分为：前言（包括网站的适用对象，网站内容和使用说明），Android简介（包括Android简介，Android特性，Android架构），环境搭建（包括Android Studio安装配置和初步认识开发工具），案例教学（包括之前所开发的十个案例app的详细教学），核心主题（包括Android重点知识点）
- 具体页面：

![alt text](37.png)
![alt text](38.png)
![alt text](39.png)


环境搭建与核心主题视觉稿与案例教学相同。
---



##### 后期工作规划：

- 下周完成3个案例App的详细教学内容


## 6. 日志六

##### 时间：

​	21春季学期第十三周



---



##### 个人工作内容：

​	完成详细案例教学并写好对应网页

![alt text](40.png)

---

##### 详细记录：

​	用Typora直接导出html，之后再插入网站中。

---



##### 后期工作规划：

- 下周开始docker虚拟环境配置

## 7. 日志七

#### 时间：

​	21春季学期第十四周

---

#### 个人工作内容：

​	vnc远程连接相关

---

#### 前提知识：

-  远程控制

​		远程控制是指管理人员在异地通过计算机网络异地拨号或双方都接入Internet等手段，连通需被控制的计算机，将被控计算机的桌面环境显示到自己的计算机上，通过本地计算机对远方计算机进行配置、软件安装程序、修改等工作。

- SSH

​		SSH 是一种加密的网络传输协议，可在不安全的网络中为网络服务提供安全的传输环境，SSH 通过在网络中创建安全隧道来实现 SSH 客户端与服务器之间的连接。SSH 最常见的用途是远程登录系统，人们通常利用 SSH 来传输命令和远程执行命令。

- 容器

​		容器就是将软件打包成标准化单元，以用于开发、交付和部署。容器镜像是轻量的、可执行的独立软件包 ，包含软件运行所需的所有内容：代码、运行时环境、系统工具、系统库和设置。

- 镜像

​		镜像是一种文件存储形式，是冗余的一种类型，一个磁盘上的数据在另一个磁盘上存在一个完全相同的副本即为镜像。

- Docker

​		Docker是世界领先的软件容器平台。Docker属于操作系统层面的虚拟化技术，由于隔离的进程独立于宿主和其它的隔离的进程，因此也称其为容器。

![alt text](41.png)



#### 详细内容：



> VNC简介

​		VNC (Virtual Network Console)，即虚拟网络控制台，它是一款基于 UNIX 和 Linux 操作系统的优秀远程控制工具软件。

​		VNC基本上是由两部分组成：一部分是客户端的应用程序(vncviewer)；另外一部分是服务器端的应用程序(vncserver)。在任何安装了客户端的应用程序(vncviewer)的计算机都能十分方便地与安装了服务器端的应用程序(vncserver)的计算机相互连接。



##### vnc工作流程：

（1）在服务器端启动 VNC Server。
（2）VNC客户端通过浏览器或 VNC Viewer 连接至VNC Server；
（3）VNC Server传送一对话窗口至客户端，要求输入连接密码， 以及存取的VNC Server显示装置。
（4）在客户端输入联机密码后，VNC Server验证客户端是否具有存取权限。
（5）若是客户端通过 VNC Server 的验证，客户端即要求VNC Server显示桌面环境。
（6）VNC Server通过X Protocol 要求X Server将画面显示控制权交由VNC Server负责。
（7）VNC Server将来由 X Server 的桌面环境利用VNC通信协议送至客户端， 并且允许客户端控制VNC Server的桌面环境及输入装置。

##### docker中安装ubuntu:
参考：[https://www.runoob.com/docker/docker-install-ubuntu.html](https://www.runoob.com/docker/docker-install-ubuntu.html)


##### ubuntu配置图形界面Xfce4：

​		参考：[https://www.cnblogs.com/Nick-Hu/p/8435602.html](https://www.cnblogs.com/Nick-Hu/p/8435602.html)

![alt text](42.png)

##### 安装配置 VNC Server：

​		参考：
[https://blog.csdn.net/han609768249/article/details/78759590](https://blog.csdn.net/han609768249/article/details/78759590)
[https://www.linuxidc.com/Linux/2017-07/145552.htm](https://www.linuxidc.com/Linux/2017-07/145552.htm)
##### 安装Android Studio：

​		参考：[https://blog.csdn.net/qq_22948593/article/details/109957099](https://blog.csdn.net/qq_22948593/article/details/109957099)



##### 在vnc viewer里查看：

![alt text](43.png)


## 总结
主要学到了：安卓开发的基础知识、搭建网站的基础知识