 **前言** 







Cobalt Strike 是由Strategic Cyber公司开发的一款商业化渗透测试工具。该软件具有简单易用、可扩展性高等优点，并且具备团队协作等特点，因此被广大黑客、白帽子和安全研究人员等大量装备使用。网络空间测绘就是利用扫描发掘互联网中一切可发掘的资产和目标。Cobalt Strike 的发掘，是360 Quake团队一直以来的核心目标之一。

Clone Site是Cobalt Strike的一个克隆网站的工具，该工具可以对任意目标网站进行克隆，达到迷惑受害者的目的。该工具可以嵌入一个javascript链接进行键盘记录和嵌入一个长宽为0的IFRAME标签直接进行载荷投递，详细的使用方式不在此赘述，有兴趣可请参考Cobalt Strike文档[1]。

![Image](%E6%B5%85%E6%9E%90CobaltStrike%E9%92%93%E9%B1%BC%E7%BD%91%E7%AB%99%E6%A3%80%E6%B5%8B.assets/640)

**网络空间测绘技术能够利用网络扫描探测机制，以工程化手段，不断发掘互联网中开放的资产、目标。在本文中，我们将对比克隆网站和真实网站的差异，并通过阅读克隆网站的源码，经过分析和特征比对，尝试对该克隆工具生成的钓鱼网站进行检测和识别，最后对全网钓鱼网站分布情况进行排查。**









 **检测方法** 







工欲善其事，必先利其器。在本地搭建测试环境进行测试。如图所示，从浏览器渲染出来的页面上看，很难找到克隆网站和正常网站的区别。

![Image](%E6%B5%85%E6%9E%90CobaltStrike%E9%92%93%E9%B1%BC%E7%BD%91%E7%AB%99%E6%A3%80%E6%B5%8B.assets/640-20210610001233099-3255153.)

但是拿两个网站的响应体进行对比。可以发现克隆网站比正常网站多了几个标签。在网页头部嵌入了base标签和link标签，其中base标签的链接是真实目标网站的链接，之后在网页body尾部嵌入了IFRAME标签和script标签，IFRAME标签加载了一个远程木马，script标签加载了一个伪装成jquery的键盘记录脚本。

![Image](%E6%B5%85%E6%9E%90CobaltStrike%E9%92%93%E9%B1%BC%E7%BD%91%E7%AB%99%E6%A3%80%E6%B5%8B.assets/640-20210610001233062)

通过对Cobalt Strike进行抓包能够看到克隆网站的整个过程，可以看到克隆工具在原有网页基础上进行了标签嵌入。

![Image](%E6%B5%85%E6%9E%90CobaltStrike%E9%92%93%E9%B1%BC%E7%BD%91%E7%AB%99%E6%A3%80%E6%B5%8B.assets/640-20210610001233083)

整个流程逻辑可归纳为下图所示：

![Image](%E6%B5%85%E6%9E%90CobaltStrike%E9%92%93%E9%B1%BC%E7%BD%91%E7%AB%99%E6%A3%80%E6%B5%8B.assets/640-20210610001233080)

通过反编译出源码也可以看到整个嵌入过程，在目标网站页面头部中，如果没有”shoetcut icon”和rel=”icon”时则直接在”<head>”后面插入link标签，当网页没有”<base href=”时则在”<head>”后面插入base标签，并且href里的链接为真实网站链接。

![Image](%E6%B5%85%E6%9E%90CobaltStrike%E9%92%93%E9%B1%BC%E7%BD%91%E7%AB%99%E6%A3%80%E6%B5%8B.assets/640-20210610001233075)

为什么无论如何都要保证页面中要有base标签呢？这是因为克隆工具仅仅克隆了一个首页，一些静态资源还在真实网站上，一些静态资源如图片、css文件等链接可能是相对路径。为了保证资源正确加载，确保页面与正常网站无异，所以一定要指定一个base标签为网页中的链接提供一个正确的目标地址[2]。其实这里存在一个特征，就是在都符合条件时，根据代码实现的逻辑会出现下列内容特征和顺序特征，但是这个特征不算强特征，还需要结合其他特征信息进行甄别。

![Image](%E6%B5%85%E6%9E%90CobaltStrike%E9%92%93%E9%B1%BC%E7%BD%91%E7%AB%99%E6%A3%80%E6%B5%8B.assets/640-20210610001233075-3255153.)

接下来继续看其他两个标签的嵌入，这一步是在客户端完成的。在Teamserver将处理过的网页返回给客户端，之后会根据用户的选项选择性的插入两个标签，并将页面返回给Teamserver创建钓鱼网站。如下图所示。

![Image](%E6%B5%85%E6%9E%90CobaltStrike%E9%92%93%E9%B1%BC%E7%BD%91%E7%AB%99%E6%A3%80%E6%B5%8B.assets/640-20210610001233095)

这里同样存在内容特征和顺序特征。

**内容特征：**

\1. IFRAME标签为大写，且长宽为0。

\2. script标签加载了js路径为”/jquery/jquery.min.js”

**顺序特征:**

\1. IFRAME标签和script标签同时出现时，一定是IFRAME标签、script标签和body标签这个顺序。

\2. IFRAME标签和script标签只出现一个时，一定在body标签之前。

![Image](%E6%B5%85%E6%9E%90CobaltStrike%E9%92%93%E9%B1%BC%E7%BD%91%E7%AB%99%E6%A3%80%E6%B5%8B.assets/640-20210610001233083-3255153.)

同时还存在一个强特征，jquery.min.js不是一个真正的jquey脚本，而是由keylogger.js为模板修改而来，如图所示。

![Image](%E6%B5%85%E6%9E%90CobaltStrike%E9%92%93%E9%B1%BC%E7%BD%91%E7%AB%99%E6%A3%80%E6%B5%8B.assets/640-20210610001233119)

![Image](%E6%B5%85%E6%9E%90CobaltStrike%E9%92%93%E9%B1%BC%E7%BD%91%E7%AB%99%E6%A3%80%E6%B5%8B.assets/640-20210610001233081)



这个文件里的变量名等也可以做一个特征，来验证是否为该工具生成的钓鱼网站。

最后，我们结合前面所说的特征，生成了两条搜索语法。

**语法一：**

**response:"<head> <base href=" AND response:"<link rel=\"shortcut icon\" type=\"image/x-icon\" href=\"/favicon.ico\">" AND response:"jquery/jquery.min.js\"></script> </body>"**



**语法二：**

**response:"<head> <base href=" AND response:"<link rel=\"shortcut icon\" type=\"image/x-icon\" href=\"/favicon.ico\">" AND response:"WIDTH=\"0\" HEIGHT=\"0\"></IFRAME>"**









 **全****网探测** 







根据之前搜索语法搜索到了44个独立IP，经确认其中40个IP为钓鱼网站，部分数据如图所示。

![Image](%E6%B5%85%E6%9E%90CobaltStrike%E9%92%93%E9%B1%BC%E7%BD%91%E7%AB%99%E6%A3%80%E6%B5%8B.assets/640-20210610001233107)

完整数据可以在

https://github.com/360quake/CobaltStrike_Phishing_Website找到。

全球分布如图所示。

![Image](%E6%B5%85%E6%9E%90CobaltStrike%E9%92%93%E9%B1%BC%E7%BD%91%E7%AB%99%E6%A3%80%E6%B5%8B.assets/640-20210610001233099)

找了两个IP访问效果如下图所示。

![Image](%E6%B5%85%E6%9E%90CobaltStrike%E9%92%93%E9%B1%BC%E7%BD%91%E7%AB%99%E6%A3%80%E6%B5%8B.assets/640-20210610001233096)

![Image](%E6%B5%85%E6%9E%90CobaltStrike%E9%92%93%E9%B1%BC%E7%BD%91%E7%AB%99%E6%A3%80%E6%B5%8B.assets/640-20210610001233110)









 **结论** 







**网络空间测绘，始于资产，但不止于资产。**

我们认为，主动测绘数据将会与终端行为样本数据、网络流量通信数据一样，是未来网络安全大数据&威胁情报数据的重要源头。主动测绘数据和基于测绘数据分析后形成的知识将能够极大补充我们的视野，从而开拓出更多的攻击面和领域。更多网络空间测绘领域研究内容，敬请期待~