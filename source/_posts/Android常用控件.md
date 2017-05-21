---
title: Android常用控件(下拉列表，日期时间选择器，多选单选框)
date: 2016-03-10 21:47:18
toc: true
categories: Android
tags:
- android
---

### 前言
忽然间就开学了，突然有些不知所措，刚开学的事情乱糟糟的堆在一块，也没有什么心思学习了。
今天课比较少，看了些关于Android的常用控件的知识，整理下来。

### 下拉列表
在布局文件中使用`Spinner`控件

<!-- more -->

```xml
<Spinner
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/spinner"
        android:layout_gravity="center_horizontal" />
```
相应程序代码:

```java
public class MainActivity extends AppCompatActivity {

    private Spinner s; //声明控件
    private String[] dataSource = new String[]{"IT STUDIO", "waydrow", "taylor"};  //列表数组

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        s = (Spinner) findViewById(R.id.spinner);  //引用到该控件
        s.setAdapter(new ArrayAdapter<String>(this,android.R.layout.simple_list_item_1,dataSource));  //生成下拉列表

        /*添加列表选择监听器*/
        s.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
                System.out.println("用户选择的是 "+ dataSource[position]);
            }
            @Override
            public void onNothingSelected(AdapterView<?> parent) { }
        });
    }
}
```

展示如下图:
![](http://7xrmgx.com1.z0.glb.clouddn.com/2016-03-10_210102.png)

### 日期选择器

```java
new DatePickerDialog(ChooseADate.this, new DatePickerDialog.OnDateSetListener() {
	@Override
	public void onDateSet(DatePicker view, int year, int monthOfYear, int dayOfMonth) {
		/*月份从0计数*/
		String theDate = String.format("%d-%d-%d",year,monthOfYear+1,dayOfMonth);
		System.out.println(theDate);
		btnChooseDate.setText(theDate);
	}
},2016,2,30).show();
```

![](http://7xrmgx.com1.z0.glb.clouddn.com/date.png)

非常好看的一个日历控件

### 时间选择器
和日期选择器类似

```java
new TimePickerDialog(ChooseTime.this, new TimePickerDialog.OnTimeSetListener() {
	@Override
	public void onTimeSet(TimePicker view, int hourOfDay, int minute) {
		String time = String.format("%d:%d",hourOfDay,minute);
		System.out.println(time);
	}
},0,0,true).show();
```

![](http://7xrmgx.com1.z0.glb.clouddn.com/time.png)


### 单项选择
实现起来非常容易，使用`RadioGroup`和`RadioButton`

```xml
<RadioGroup
	android:layout_width="wrap_content"
	android:layout_height="wrap_content"
	android:layout_gravity="center_horizontal">

	<RadioButton
	    android:layout_width="wrap_content"
	    android:layout_height="wrap_content"
	    android:text="A"
	    android:id="@+id/rbA"
	    android:layout_gravity="center_horizontal" />
	<RadioButton
	    android:layout_width="wrap_content"
	    android:layout_height="wrap_content"
	    android:text="B"
	    android:id="@+id/rbB"
	    android:layout_gravity="center_horizontal" />
	<RadioButton
	    android:layout_width="wrap_content"
	    android:layout_height="wrap_content"
	    android:text="C"
	    android:id="@+id/rbC"
	    android:layout_gravity="center_horizontal" />
	<RadioButton
	    android:layout_width="wrap_content"
	    android:layout_height="wrap_content"
	    android:text="D"
	    android:id="@+id/rbD"
	    android:layout_gravity="center_horizontal" />
</RadioGroup>
```

![](http://7xrmgx.com1.z0.glb.clouddn.com/singleChoice.png)

### 多项选择
使用`CheckBox`控件即可

![](http://7xrmgx.com1.z0.glb.clouddn.com/mulChoice.png)

