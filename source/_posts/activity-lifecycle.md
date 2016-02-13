---
title: Android开发：Activity 生命周期详解
date: {}
categories: Android
tags: 
  - Android
  - Activity
published: true
---


## 什么是Activity
&emsp;Activity是Android SDK中`Activity`类的一个具体实例，负责管理用户与信息屏的交互。在一个应用程序中通常由多个Activity构成，在`Manifest.xml`中会指定一个主的Activity, 如下所示
`<action android:name="android.intent.action.MAIN" />`  
&emsp;当程序第一次运行时用户就会看这个Activity，这个Activity可以通过启动其他的Activity进行相关操作。当启动其他的Activity时这个当前的这个Activity将会停止，新的Activity将会压入栈中，同时获取用户焦点，这时就可在这个Activity上操作了。都知道栈是先进后出的原则，那么当用户按Back键时，当前的这个Activity销毁，前一个Activity重新恢复。
<!-- more -->
## Activity的生命周期
话不多说，先上图  
![](http://7xqoa3.com1.z0.glb.clouddn.com/images%2Factivity-life.gif)  
通过这个图我们可以很清晰的看到Activity的整个生命流程。  
Activity其实是继承了`ApplicationContext`这个类  
### 我们可以重写以下方法，如下代码:

``` java
 public class Activity extends ApplicationContext {
        protected void onCreate(Bundle savedInstanceState);
        
        protected void onStart();   
        
        protected void onRestart();
        
        protected void onResume();
        
        protected void onPause();
        
        protected void onStop();
        
        protected void onDestroy();
    }
```

### 下面我们来写一个Demo，更好地理解，新建一个项目，代码如下：

``` java
package com.waydrow.activityliftcircle;

import android.os.Bundle;
import android.support.design.widget.FloatingActionButton;
import android.support.design.widget.Snackbar;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.view.View;
import android.view.Menu;
import android.view.MenuItem;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);

        FloatingActionButton fab = (FloatingActionButton) findViewById(R.id.fab);
        fab.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Snackbar.make(view, "Replace with your own action", Snackbar.LENGTH_LONG)
                        .setAction("Action", null).show();
            }
        });

        System.out.println("on create");
    }

    @Override
    protected void onStart() {
        super.onStart();
        System.out.println("on start");
    }

    @Override
    protected void onResume() {
        super.onResume();
        System.out.println("on resume");
    }

    @Override
    protected void onPause() {
        super.onPause();
        System.out.println("on Pause");
    }

    @Override
    protected void onStop() {
        super.onStop();
        System.out.println("on stop");
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        System.out.println("on destroy");
    }

    @Override
    protected void onRestart() {
        super.onRestart();
        System.out.println("on restrat");
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();

        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            return true;
        }

        return super.onOptionsItemSelected(item);
    }
}
```

### 点击运行
显示界面如图，没有什么好说的  
![](http://7xqoa3.com1.z0.glb.clouddn.com/images%2F2016-02-13_202107.png)   

### 打开`Logcat`查看输出信息
![](http://7xqoa3.com1.z0.glb.clouddn.com/images%2F2016-02-13_202129.png)  

我们可以清楚的看到，这个Activity的创建过程为  
`create`->`start`->`resume`  

## Back键和Home键的区别
在上述运行状态下，分别点击界面的Back按钮和Home键，再次查看控制台输出  
**Back键**：  
![](http://7xqoa3.com1.z0.glb.clouddn.com/images%2F2016-02-13_202534.png)  

**Home键**：  
![](http://7xqoa3.com1.z0.glb.clouddn.com/images%2F2016-02-13_202805.png)

这两者的区别显而易见了，点击Back按钮后，此Activity会经历  
`pause`->`stop`->`destroy`   
此Activity弹出栈，程序销毁。
但是点击Home键，Activity并不会被立即销毁

## 后记
大家还可以尝试旋转屏幕、锁屏等后的Activity的状态，本文就不一一列举了
### 参考资料
[Activity详解 (生命周期、以各种方式启动Activity、状态保存，完全退出等)](http://blog.csdn.net/tangcheng_ok/article/details/6755194)  
[两分钟彻底让你明白Android Activity生命周期(图文)!](http://blog.csdn.net/android_tutor/article/details/5772285)  
[Android四大组件之——Activity的生命周期 (图文详解)](http://www.cnblogs.com/JohnTsai/p/4052676.html)  

如有问题，欢迎在直接下方讨论留言  
我的邮箱为：<Waydrow@163.com>
