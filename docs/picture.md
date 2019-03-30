# 图片开源库

# Glide

### 总体设计

![glide_2016427150437442](https://ws1.sinaimg.cn/large/006tKfTcly1g1glhv0fxvj30qp0lb3zn.jpg)

基本概念
RequestManager：请求管理，每一个Activity都会创建一个RequestManager，根据对应Activity的生命周期管理该Activity上所以的图片请求。
Engine：加载图片的引擎，根据Request创建EngineJob和DecodeJob。
EngineJob：图片加载。
DecodeJob：图片处理。

### 流程图

这里是大概的总体流程图， 具体的细节中流程下面继续分析。

![glide_2016427150459886](https://ws4.sinaimg.cn/large/006tKfTcly1g1glkw6kgrj30qq0xy75h.jpg)

[参考](https://www.jb51.net/article/83153.htm)

