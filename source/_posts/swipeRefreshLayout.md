---
title: Android SwipeRefreshLayout 下拉刷新组件的使用
date: 2016-03-11 20:53:43
categories: Android
tags:
	- Android
	- SwipeRefreshLayout
	- ListView
---

### 前言
在极客学院的Android学习中，发现其下拉刷新组件用的是比较老的组件，现在Google官方出的是`SwipeRefreshLayout`，借此机会学习了一下。先附上图: 

<!-- more -->

![](http://7xrmgx.com1.z0.glb.clouddn.com/swipe.png)

### xml资源文件

```xml
<android.support.v4.widget.SwipeRefreshLayout
    android:id="@+id/swipeLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <ListView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/listView"
        android:layout_gravity="center_horizontal" />

</android.support.v4.widget.SwipeRefreshLayout>
```

只需要添加一个`SwipeRefreshLayout`, 其中的数据列表项我使用了`ListView`来显示数据

### 相应代码

```java
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        /*获取ListView*/
        listView = (ListView) findViewById(R.id.listView);
        /*为listView 添加适配器*/
        listView.setAdapter(new ArrayAdapter<>(this, android.R.layout.simple_list_item_1, getData()));

        /*获取SwipeRefreshLayout*/
        swipeLayout = (SwipeRefreshLayout) findViewById(R.id.swipeLayout);
        /*设置下拉刷新监听器*/
        swipeLayout.setOnRefreshListener(new SwipeRefreshLayout.OnRefreshListener() {
            @Override
            public void onRefresh() {
                swipeLayout.postDelayed(new Runnable() {
                    @Override
                    public void run() {
                        swipeLayout.setRefreshing(false);
                    }
                }, 3000);    //延时3秒
            }
        });
    }

    /*预先定义listView显示的列表项*/
    private List<String> getData() {
        List<String> data = new ArrayList<String>();
        for (int i = 0; i < 20; i++) {
            data.add("Item list " + (i + 1));
        }
        return data;
    }
```

是不是非常easy呢 ?　

### 后记
在创建`ListView`过程中, 我使用的是`ArrayAdapter`适配器, 还有 :  

- SimpleAdapter
- SimpleCursorAdapter
- BaseAdapter

都还不太了解, 下面准备详细学下`ListView`的各种适配器。

相应的详细代码我放在了我的github上，[这是链接](https://github.com/Waydrow/Android-Learning/tree/master/SwipeRefreshLayout)