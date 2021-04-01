# 网络空间搜索引擎研究

# 工作来源

17th China Annual Conference, CNCERT 2020

## 工作背景

网络空间搜索引擎可以搜索网络空间中各种类型的在线设备，如网络摄像头、路由器、智能冰箱、工控设备等。探针向远程网络设备发送探测数据包，接收并分析响应数据，得到远程设备的信息。例如开放端口和服务、操作系统、漏洞分布、设备类型、组织、地理位置等。探测主要在传输层和应用层上，**传输层的检测方法包括 SYN 扫描、TCP 连接扫描、UDP 扫描、FIN 扫描、ICMP 扫描等。应用层的检测方法包括互联网协议、特殊文件、哈希值、证书等**。

掌握网络资源对于网络空间治理和网络安全保护至关重要，工作研究了 Shodan、Censys、BinaryEdge、ZoomEye 和 Fofa 五个网络空间搜索引擎，**从支持互联网协议、检测设备总数、设备信息、扫描频率、系统架构、第三方数据、探针分布等角度对网络空间搜索引擎进行深入分析**，全面比较网络空间搜索引擎的检测能力和工作原理。

## 支持互联网协议

将所有设备分为十一类：网络设备、终端、服务器、办公设备、工控设备、智能家居、电源设备、网络摄像头、远程管理设备、区块链、数据库。

![Image](%E7%BD%91%E7%BB%9C%E7%A9%BA%E9%97%B4%E6%90%9C%E7%B4%A2%E5%BC%95%E6%93%8E%E7%A0%94%E7%A9%B6_Li2020_Chapter_ASurveyOnCyberspaceSearchEngin%E8%AE%BA%E6%96%87.assets/640-20210226140246274.png)

从各个网络空间搜索引擎的官方网站、用户手册、API 文档、技术论坛上整理得到所支持的协议，归类到十一大类中。

![Image](%E7%BD%91%E7%BB%9C%E7%A9%BA%E9%97%B4%E6%90%9C%E7%B4%A2%E5%BC%95%E6%93%8E%E7%A0%94%E7%A9%B6_Li2020_Chapter_ASurveyOnCyberspaceSearchEngin%E8%AE%BA%E6%96%87.assets/640)

## 检测设备总数

Shodan 检测设备总数来自官方查询工具 CLi.shodan.io。该工具可以查询 2009 年 1 月 1 日之后的所有数据记录，计算得到设备总数。Censys 的官方网站具备数据统计功能，将 IPv4 地址空间划分为 256 块，然后用 Censys 检索每个地址块，在返回的结果中计算检测设备总数。ZoomEye、Fofa 和 BinaryEdge 的检测设备总数来自官方网站。

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 设备信息

设备信息通常包括域名、开放端口、服务、地理位置、国家/地区、设备类型、从属关系等。故而**将设备信息分类为：设备信息、位置信息、端口信息、漏洞信息、探测点信息、标签信息、网络设备信息、WEB信息、文件传输、电子邮件协议信息、远程信息访问、数据库信息、工控协议信息、消息队列、聚类信息**。

下图以 Censys 为例，漏洞信息和探测点信息为虚线，因为 Censys 不提供此类信息。

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

由于对整个网络进行完整扫描会消耗大量的计算和存储资源，因此搜索引擎通常会设置扫描频率。**扫描频率是一个重要指标，频率越高，搜索引擎的性能就越强**。

随机选择了 130 多个 IP 地址（打开 HTTP、HTTPS、TELNET、FTP 和 SSH 服务），每天检查这些 IP 地址在各个搜索引擎的状态，以此获取每个引擎的扫描间隔。

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

注："-"表示很久没有扫描过。

## 系统架构

通常来说，搜索引擎可分为三个部分：**信息采集模块、数据存储模块和信息检索模块**。信息采集模块负责收集网络空间中各种设备的信息、数据存储模块负责存储收集的海量设备信息、信息检索模块负责提供统计和查询服务。

### Censys

Censys 的扫描模块会将检测结果保存到 Zdb 数据库，所有数据都将存储在 Google Cloud Strorage 中。使用 Elastic Search 提供全文检索、使用 Google Datastore 提供历史检索、使用 Google BigQuery 提供统计检索。

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### BinaryEdge

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 第三方数据

-   Shodan 和 Censys 的 IP 地址是随机生成的，不依赖于第三方 IP 数据
-   Censys 使用 Alexa Top 100 万提供的域名数据，而 BinaryEdge 使用 Passive DNS 数据
-   **Censys、Fofa 和 BinaryEdge 都使用 GeoIP 的定位数据，而 ZoomEye 使用 IPIP.net 的定位数据**

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 探针分布

网络空间搜索引擎通常需要部署许多探针，GreyNoise 与 BinaryEdge 在检测探针方面做的都很好。

### GreyNoise

GreyNoise 发现了 96 个搜索引擎的探针，包括 Shodan、Censys、BinaryEdge 和 ZoomEye。

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### BinaryEdge

BinaryEdge 通过蜜罐的数据来进行分析，由于蜜罐不主动与外界发生连接，因此对分析探针很有帮助。

![Image](%E7%BD%91%E7%BB%9C%E7%A9%BA%E9%97%B4%E6%90%9C%E7%B4%A2%E5%BC%95%E6%93%8E%E7%A0%94%E7%A9%B6_Li2020_Chapter_ASurveyOnCyberspaceSearchEngin%E8%AE%BA%E6%96%87.assets/640-20210226140245990)

## 工作思考

横向比较总是困难的，相信每个搜索引擎也都各有所长，结合每个人的使用体验，好坏自在人心。关于这一点，各个公司的大佬也都发过相关的文章进行了争辩与讨论，在此不加赘述。

**网络空间搜索引擎从 2009 年 Shodan 发布已经走过了十余个年头，网空测绘作为主动探测的手段已经越来越融入安全分析流程当中。**经过多年的探索，网络空间搜索引擎不止局限于当初的识别与发现网联设备的功能，玩出了很多花样：例如Shodan 联合 Recorded Future 通过 RAT 的 C&C 特征，利用探针扫描判断是否为 C&C 服务器从而检测十余个 RAT 的 C&C 节点分布。当然，Censys、ZoomEye 等网空搜索引擎都有类似的工作，包括大热的对 Cobalt Strike 等红队工具的测绘变成防守方反制的先手棋。**对攻击基础设施的测绘、对蜜罐的测绘、对暗网的测绘… …** 随着越来越多的玩家入局建设网络空间搜索引擎，由网空测绘结果数据上进行分析和挖掘产出的花样会越来越多，但抛掉这些浮华与热闹，**深耕网络空间搜索引擎的核心能力才是成事之基、动力之源。**