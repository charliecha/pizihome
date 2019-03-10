# 网络开源库

## OKHttp

### demo代码

```
val okHttpClient = OkHttpClient()
val builder = Request.Builder()
val request = builder.url("https://baidu.com").build()
val response = okHttpClient.newCall(request).execute()
```

### api 设计

**Request -> Run -> Response**，基本设计按照请求->执行->返回结果的思路。

**核心思想是责任链模式，把网络请求这件完整的事情，按照职责划分为不同的部分，由每个部分自己决定是自己处理还是交给其他部分去处理。**

**通过ConnectionPoll，可以实现Socket复用。**具体来说Http2.0可以实现。http2.0协议使用了多路复用技术，允许同一个socket在同一个时候写入多个流数据，每个流有id，会进行组装，因此，这个时候正在写入数据的socket是可以被复用的。

[参考](https://blog.piasy.com/2016/07/11/Understand-OkHttp/index.html)

主要流程图如下。

![okhttp_full_process](https://blog.piasy.com/img/201607/okhttp_full_process.png)

## Volley

Volley Retrofit android-async-http帮你封装了具体的请求，线程切换以及数据转化。

而 Okhttp是基于http协议封装的一套请求客户端，虽然它也可以开线程，但根本上它更偏向真正的请求，跟HttpClient

,HttpUrlConnect的职责是一样的。

### Volley分三组线程分别处理缓存、网络请求、和返回结果。

[参考](https://blog.csdn.net/t12x3456/article/details/9221611)

![Volley架构设计](http://bxbxbai.github.io/img/volley.png)

## Retrofit

Retrofit是一个网络框架，底层使用Okhttp完成网络请求的发起，其主要职责是提供一个高度解耦的网络框架。

![Retrofit设计架构](https://blog.piasy.com/img/201606/retrofit_stay.png)

[参考](https://www.jianshu.com/p/45cb536be2f4)