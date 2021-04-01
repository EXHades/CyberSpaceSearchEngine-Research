# 空间测绘系列之 - Shodan 工具、研发类板块分析 (未完)

上接[空间测绘系列之 - Shodan 功能总览](https://attacker.cc/index.php/archives/175/)，本文是对 Shodan4 大版块中**工具、研发类子系统**的分析。



## 1. [Images](https://images.shodan.io/) 子系统展开目录

![shodan-images.jpg](https://attacker.cc/usr/uploads/2020/02/4251762946.jpg)

**官方介绍描述**

>   A stream of screenshots from crawled devices.

其实就是把相关网络协议测绘过程中如果有图形化界面的，就进行截屏。



### 特色功能展开目录



#### 1. 数据标签展开目录

每个图片都会被打上标签，可以跟进标签进行检索，检索语法是 `screenshot.label`。

-   windows
-   login
-   desktop
-   terminal
-   webcam
-   ......
    ![shodan-images-2.jpg](https://attacker.cc/usr/uploads/2020/02/2180809058.jpg)![shodan-images-3.jpg](https://attacker.cc/usr/uploads/2020/02/4016266231.jpg)



#### 2. 标签检索展开目录

上述每个标签均可通过检索语法 `screenshot.label` 进行检索。



### 设计意图展开目录

这个系统实际上这是对空间测绘的数据进行更深度的研究和分析。

首先做这个功能的前提是要对全网 RDP 协议进行识别，但 RDP 协议的特点就是加密的，因此通过端口 Response 响应和端口指纹识别出的数据是很有限的，只能是识别操作系统型号、版本，以及 SSL/TLS 相关的证书信息。

![shodan-rdp.jpg](https://attacker.cc/usr/uploads/2020/02/3678768583.jpg)

但是如果完成建立连接以后，即使没有登陆密码，能看到的信息也是非常有价值的：

-   系统使用的语言
-   用户账号名称
-   用户头像
-   当前在线的用户
-   系统更新状态（Win2012 右下角会提示需要更新）

其他如终端登陆页面、摄像头实时数据就会包含更多更为丰富的信息，如果能够结合以下功能：

-   人脸识别技术
-   图片文字识别技术
-   图片类型分类（根据场景、或者根据包含的内容）

那么可以玩的更深入。（PS：其实某家已经完成了相关的系统）

另外 [Binaryedge](https://app.binaryedge.io/services/images) 也在里有相关的功能，甚至是更 “邪恶” 的检索语法：`has_faces:"true"`

![Binaryedge-images.jpg](https://attacker.cc/usr/uploads/2020/02/492364124.jpg)

![Binaryedge-images2.jpg](https://attacker.cc/usr/uploads/2020/02/2916954295.jpg)

(To be continued)