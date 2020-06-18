# 一、携带参数安装

## 背景

**openinstall的核心价值在于，帮助Android/iOS开发者精确的获取App每一次安装的分享(或推广)来源。**

## 举个例子

假设我在玩一款游戏app，我觉得不错，我现在想分享给没有玩过这款游戏的朋友。

正常的流程可能是这样的：我需要把app的下载链接分享出去，然后把游戏房间分享给朋友。然后朋友去下载安装，并查找房间号进入房间。整个流程有些割裂，有没有办法直接一部到位呢？

**使用openInstall可以解决这个问题。**

我写了一个测试app，并部署在了openInstall平台，如图1：

![image-20200618115611602](https://raw.githubusercontent.com/charliecha/PigureBed/master/image-20200618115611602.png)

​                                                                                            图1

应用基本信息如图2：

![image-20200618115839854](https://raw.githubusercontent.com/charliecha/PigureBed/master/image-20200618115839854.png)

​                                                                                               图2

假设我现在要创建一个带房间号的推广链接，点击生成测试链接之后，就可以生成推广链接，如图3：

![image-20200618143918187](https://raw.githubusercontent.com/charliecha/PigureBed/master/image-20200618143918187.png)

​                                                                                       图3

在手机浏览器扫码或者直接打开url，就可以进入安装页面，如图4：

![image-20200618120653577](https://raw.githubusercontent.com/charliecha/PigureBed/master/image-20200618120653577.png)

​                                                                                                              图4

点击立即使用，未安装app的话，就会去下载app，如图5：

![image-20200618120900182](https://raw.githubusercontent.com/charliecha/PigureBed/master/image-20200618120900182.png)

​                                                                  

​                                                                            图5

接下来，完成安装，如图6：

![image-20200618141056738](https://raw.githubusercontent.com/charliecha/PigureBed/master/image-20200618141056738.png)

​                                                                             图6

安装之后，启动app，如图7：

![image-20200618144036378](https://raw.githubusercontent.com/charliecha/PigureBed/master/image-20200618144036378.png)

​                                                                                                   图7

我们看到，我们需要获取的房间号信息已经拿到了。接下来，我们分析一下它的实现原理：

## 原理

**来自官方**，大致原理如下，如图8：

1. 开发者在分享的h5页面上集成openinstall web sdk，发布分享链接时在url上动态的拼接任意的自定义参数（如推广渠道号，邀请码，游戏房间号等等）；
2. 当某一终端访问该h5页面时，openinstall web sdk将同时确定该设备的个性化信息和采集自定义参数，上传至openinstall服务器，  待用户通过该h5页面安装App后首次打开时（如当前设备已安装该App，将直接拉起App），使用openinstall Android/iOS  sdk从openinstall服务器再取回暂存的自定义参数；
3. 开发者根据各自的需求，在分享链接自定义拼接各种动态参数。比如通过在分享链接url中附带App邀请人的用户id，就可达到免填邀请码的效果。对战类游戏App通过在url中附带游戏房间号，新老用户都可通过该url链接直接进入邀请人的对战房间，更多使用场景均取决于开发者的需求。

![产品原理](https://www.openinstall.io/doc/resources/yuanli.svg)

​                                                                                                           图8



**针对IOS平台：**openinstall web sdk会生成一个unique value放入cookie中，当某一终端访问该h5页面时，会在服务器绑定unique value和urlInfo 。当用户安装应用后，会从cookie 获取unique value，然后把unique value传给服务器，服务器通过unique value和urlInfo之间的绑定关系，就可以获得urlInfo。

![image-20200618180351098](https://raw.githubusercontent.com/charliecha/PigureBed/master/image-20200618180351098.png)

**然而，**Android并不是这么实现的，因为Android没有一个全局的Webview，而且也没有办法通过Webview获取DeviceID。

那么，**Android**是如何实现的呢？

先让我们看下原始apk和打包以后apk的diff差异，如图9：

![image-20200618153928585](https://raw.githubusercontent.com/charliecha/PigureBed/master/image-20200618153928585.png)

​                                                                                           图9

**从两边差异指出可以看出，我们需要url传递的信息直接被写入了apk，很粗暴。**

![image-20200618180632265](https://raw.githubusercontent.com/charliecha/PigureBed/master/image-20200618180632265.png)

关于Android打包相关技术，这里不做延伸，之前坤哥也做过分享。参考链接：http://agroup.baidu.com/baiduinput/md/article/1758246#v2%E7%AD%BE%E5%90%8D%E5%8E%9F%E7%90%86

完成urlInfo获取以后，还需要向服务器绑定deviceId和urlInfo，以便在后台跟踪某个url分享出去的apk的运行情况。

针对不同平台，获取deviceId的方式不一样，IOS是通过生成随机数，并在keychain中永久存储；Android则可以通过获取系统imei/deviceid/androidId/等系统接口来实现。

# 二、渠道统计

**openinstall在精准的App分享来源跟踪的技术上，开发了免打包、跨平台的App推广渠道统计功能。**

开发者可以利用openinstall的传参规则灵活快速地手工或程序化地大量创建渠道链接，App通过某一个渠道链接安装后，在openinstall Android/iOS  sdk初始化时，将从openinstall服务器自动获取到本次安装的渠道编号，开发者可自己收集相关信息用于生成渠道报表数据。同时openinstall Android/iOS sdk 会自动完成访问量、点击量、安装量、活跃量、留存率等统计工作，可在后台查看详细报表。

优势特点：

- 仅凭渠道链接即可精确统计任意渠道效果（兼容iOS/安卓）
- 免打包，无需开发者在代码中手动设置渠道编号重新打包
- 实时报表、实时排重
- 数据安全，只包含机型，系统版本，ip等设备相关的信息，不包含任何业务相关的数据

如需统计注册事件，开发者需结合自身业务，在用户注册成功的情况下调用openinstall相应的api，发送统计事件；如需更详细评估渠道效果，可使用相应的效果点上报api，使用前需在后台的【效果点管理】页面添加效果点。

具体使用，管道管理如图10：

![image-20200618155302813](https://raw.githubusercontent.com/charliecha/PigureBed/master/image-20200618155302813.png)

​                                                                                   图10

渠道统计如图11：

![image-20200618155443130](https://raw.githubusercontent.com/charliecha/PigureBed/master/image-20200618155443130.png) 

​                                                                              图11

![image-20200618175240101](https://raw.githubusercontent.com/charliecha/PigureBed/master/image-20200618175240101.png)

![image-20200618175303771](https://raw.githubusercontent.com/charliecha/PigureBed/master/image-20200618175303771.png)

**我们可以看到，openInstall的统计维度比较多，对于接入的用户比较方便。**

# 三、快速安装与一键拉起

在App未安装的情况下，openinstall帮助您在社交平台一键下载并安装App，实现App的快速安装，从此告别“右上角打开浏览器”，大幅提升安装概率，助力用户增长；

在App已安装的情况下，openinstall通过标准的scheme，universal  links等技术，从各种浏览器（兼容国内50+主流浏览器与社交平台，包括微信、QQ、新浪微博、钉钉等主流社交软件的内置浏览器）一键拉起App并传递自定义参数，一键直达页面。

**快速安装其实和别的app安装是一样的，没什么不一样。拉起也是通过标准的scheme，universal  links协议。**

# 四、OpenInstall的接入

1、注册OpenInstall账号

2、在OpenInstall申请应用接入资源

3、在应用中集成OpenInstall sdk

4、在OpenInstall后台就可以看到各种报表数据

# 五、在输入法中的应用

一个场景，通过OpenInstall技术，我们可以验证输入法在各个渠道的投放效果。

还有一个场景，通过OpenInstall技术，让用户在输入法中快速建立关系，让用户自来水的宣传我们的产品功能。

