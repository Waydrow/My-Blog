---
title: Volley使用JsonObjectRequest发送Post请求失败
date: 2016-03-25 21:04:46
categories: Android
tags:
	- Android
	- Volley
	- JsonObjectRequest
	- Http
---

## 前言
这段时间一直在忙比赛，开发一个Android应用。转眼间博客竟然这么久没更新了，罪过罪过...这两天在用`Volley`框架，但是当我使用`JsonObjectRequest`发送`Post`请求时，竟然失效了。服务器一直响应失败，搞了半天，在StackOverFlow上找到了类似的问题，终于解决掉了。
<!-- more -->

## 求真之路
### 原始代码展示

``` java
RequestQueue mqueue = Volley.newRequestQueue(this);
    JsonObjectRequest jsObjRequest = new JsonObjectRequest(Request.Method.POST,url,null,
            new Response.Listener<JSONObject>() {
                @Override
                public void onResponse(JSONObject response) {
                    System.out.println("Response: " + response.toString());
                }
            },
            new Response.ErrorListener() {
                @Override
                public void onErrorResponse(VolleyError error) {
                }
            }) {
    	@Override
        protected Map<String, String> getParams() throws AuthFailureError {
            Map<String, String> params = new HashMap<String, String>();
            params.put("num","140200");
            params.put("password", "123");
            return params;
        };
    };
mqueue.add(jsObjRequest);
```

这是我最先使用的方法，重载了 `getParams`函数来携带参数，之所以这么做是因为我在用`StringRequest`时就是这么干的，当然是成功的了。然而这次怎么都不对。

### 第一次修改

经过google一翻查找，发现遇到这个问题的不只我一个，心情莫名的激动起来2333。原来有这么多人掉进了这个坑里。看了某大神的博客，发现了原来我那种写法是错误的。改进如下: 

```java
RequestQueue mqueue = Volley.newRequestQueue(getApplicationContext());

HashMap<String, String> hashMap = new HashMap<String,String>();
hashMap.put("username",num);
hashMap.put("password",password);
hashMap.put("randnumber",captcha);

JSONObject jsonObject = new JSONObject(hashMap);

JsonObjectRequest jsObjRequest = new JsonObjectRequest(Request.Method.POST,url,jsonObject,
            new Response.Listener<JSONObject>() {
                @Override
                public void onResponse(JSONObject response) {
                    System.out.println("Response: " + response.toString());
                }
            },
            new Response.ErrorListener() {
                @Override
                public void onErrorResponse(VolleyError error) {
                }
            }) {
    };
mqueue.add(jsObjRequest);
```

`getParams`方法并能在些这样使用，需要new一个`JSONObject`，将需要发送的参数放进这里，然后Post出去。(的确是个好主意)马上去试下，结果发现还是不管用...

### done
最终在StackOverFlow上找到了解决方案。还是这个靠谱啊!!!泪奔，，，  

#### 工具类 CustomRequest.java

```java
package com.waydrow.campusbox;

import java.io.UnsupportedEncodingException;
import java.util.Map;
import org.json.JSONException;
import org.json.JSONObject;
import com.android.volley.NetworkResponse;
import com.android.volley.ParseError;
import com.android.volley.Request;
import com.android.volley.Response;
import com.android.volley.Response.ErrorListener;
import com.android.volley.Response.Listener;
import com.android.volley.toolbox.HttpHeaderParser;

public class CustomRequest extends Request<JSONObject> {

    private Listener<JSONObject> listener;
    private Map<String, String> params;

    public CustomRequest(String url, Map<String, String> params,
           				Listener<JSONObject> reponseListener, ErrorListener errorListener) {
        super(Method.GET, url, errorListener);
        this.listener = reponseListener;
        this.params = params;
    }

    public CustomRequest(int method, String url, Map<String, String> params,
                         Listener<JSONObject> reponseListener, ErrorListener errorListener) {
        super(method, url, errorListener);
        this.listener = reponseListener;
        this.params = params;
    }

    protected Map<String, String> getParams()
            throws com.android.volley.AuthFailureError {
        return params;
    };

    @Override
    protected Response<JSONObject> parseNetworkResponse(NetworkResponse response) {
        try {
            String jsonString = new String(response.data,
                    HttpHeaderParser.parseCharset(response.headers));
            return Response.success(new JSONObject(jsonString),
                    HttpHeaderParser.parseCacheHeaders(response));
        } catch (UnsupportedEncodingException e) {
            return Response.error(new ParseError(e));
        } catch (JSONException je) {
            return Response.error(new ParseError(je));
        }
    }

    @Override
    protected void deliverResponse(JSONObject response) {
        // TODO Auto-generated method stub
        listener.onResponse(response);
    }
}
```

然后在自己代码中发送POST请求时，样例如下: 

```java
RequestQueue requestQueue = Volley.newRequestQueue(getApplicationContext());

	HashMap<String, String> hashMap = new HashMap<String,String>();
	hashMap.put("username",num);
	hashMap.put("password",password);

	CustomRequest jsonObjectRequest = new CustomRequest(Request.Method.POST, 
		url, hashMap, new Response.Listener<JSONObject>() {
	    @Override
	    public void onResponse(JSONObject jsonObject) {
	        Log.d("TagJson",jsonObject.toString());
	    }
	}, new Response.ErrorListener() {
	    @Override
	    public void onErrorResponse(VolleyError volleyError) {

	    }
	});
requestQueue.add(jsonObjectRequest);
```

## 后记
终于解决了，，普天同庆，顿时觉得整个世界都变好了。。  
不过话说回来，仔细想一下，这个问题产生的原因可能不是本身代码的问题。  
我的第二个方案代码是Volley官方资料上的使用方法，按理说不应该有问题。那么既然客户端这边没有问题，只能是服务器端的事了。猜想可能是由于服务器端不支持响应json格式的请求，才出现了请求失败的问题。不过到底是不是这样还有待考量。准备有空了试一下，在服务器端做下处理。  

如有问题欢迎在正文评论留言，也可直接联系我。  
邮箱: <waydrow@163.com>
