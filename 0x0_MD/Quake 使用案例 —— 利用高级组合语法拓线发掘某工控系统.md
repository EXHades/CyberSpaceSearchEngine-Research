## 0x01 线索展开目录

近日，我们留意到一个新闻：[以色列供水设施 ICS 遭伊朗黑客入侵](https://mp.weixin.qq.com/s/WbMIq7cv-cMxjyVw1CiV1g)

原文地址如下：
`https://www.otorio.com/blog/what-we-ve-learned-from-the-december-1st-attack-on-an-israeli-water-reservoir/`

在原文中，OTORIO 团队提到了一个供水设施工业控制系统的互联网的人机接口（HMI），指出了该 HMI 系统未授权暴露在整个互联网上，直接通过 Web 界面即可访问。

其实在很多国外的安全文章中，都会使用 Shodan 给出某个具体 IP 或产品名用于佐证。安全研究人员经常会对这些文章中体到线索进行追溯、分析，下面将给出一个实际的例子。



## 0x02 分析展开目录

在 OTORIO 团队原文中截图如下：

![19270-fvirgun88gb.png](https://attacker.cc/usr/uploads/2020/12/696735486.png)

IP 被作者隐藏了，那么**我们如何通过上图信息进行挖掘找到真正的系统呢？** 我们从上图中提取出 3 点要素：

1.  ASN 为 `16116`；
2.  同时开放 80 端口、502 端口；
3.  协议分别是 http 协议和 modbus 协议。

Quake 检索系统中主要有两大部分，分别是 `服务数据` 和 `主机数据`：

-   `服务数据`是保留端口返回 response 和协议返回内容的地方，会以`累计`的形式保存全量历史数据，用于刻画一个 IP 的全部历史活动痕迹；
-   `主机数据`是删除端口返回、协议详情的地方，会以`去重`的形式保存全量数据，用于刻画一个 IP 的完整端口及其对应信息；

基于主机数据高级搜索语法（**Quake 高级会员、终身会员特有**）：

-   `ports`，语法仅能在 `Quake主机数据`中使用，表示某个 IP 同时开放了哪些端口；
-   `services`，语法仅能在 `Quake主机数据`中使用，表示某个 IP 同时使用了哪些协议；

基于上述信息，在 "主机数据" 中 使用搜索语法如下：

```
 ports:"80,502" AND services:"http,modbus" AND asn: "16116" 
```

![09670-mnvgn1lag3p.png](https://attacker.cc/usr/uploads/2020/12/96180705.png)

得到 7 个独立 IP 地址

```
188.64.203.x

91.135.106.x
91.135.105.x
91.135.106.x

85.159.161.x
85.159.163.x
85.159.162.x
```

搜索这些 IP 在` Quake服务数据 `中的 502 端口，可以看到 modbus 协议解析内容和 Shodan 的一致：

```
(ip:"188.64.203.x" OR ip:"91.135.106.x" OR ip:"91.135.105.x" OR ip:"91.135.106.x" OR ip:"85.159.161.x" OR ip:"85.159.163.x" OR ip:"85.159.162.x") AND port: "502"
```

![33577-z5si225ajo.png](Quake%20%E4%BD%BF%E7%94%A8%E6%A1%88%E4%BE%8B%20%E2%80%94%E2%80%94%20%E5%88%A9%E7%94%A8%E9%AB%98%E7%BA%A7%E7%BB%84%E5%90%88%E8%AF%AD%E6%B3%95%E6%8B%93%E7%BA%BF%E5%8F%91%E6%8E%98%E6%9F%90%E5%B7%A5%E6%8E%A7%E7%B3%BB%E7%BB%9F.assets/4213955073.png)

分别访问 80 端口发现，有 3 个无法访问，2 个系统加了 HTTP 认证（通过服务数据交叉验证，就是文章中体到的系统）。

同时我们发现了另一个新的供水设施工业控制系统存在未授权访问：

![28083-6wyu7wxufxo.png](Quake%20%E4%BD%BF%E7%94%A8%E6%A1%88%E4%BE%8B%20%E2%80%94%E2%80%94%20%E5%88%A9%E7%94%A8%E9%AB%98%E7%BA%A7%E7%BB%84%E5%90%88%E8%AF%AD%E6%B3%95%E6%8B%93%E7%BA%BF%E5%8F%91%E6%8E%98%E6%9F%90%E5%B7%A5%E6%8E%A7%E7%B3%BB%E7%BB%9F.assets/2261959125.png)



## 0x03 拓线展开目录

另外，OTORIO 安全团队通过分析判定该系统为 Ovarro 公司的 `T-Box` 系统。基于上述相关线索，我们进行扩展发现，该系统对应的指纹如下：

```
 response:"<body style=\"margin:0;padding:0\">" AND response:"<iframe src=\"index.xhtml\"" AND response:"content=\"text/html; charset=utf-8\"/>" 
```

对分布情况进行统计如下：

![48955-rhtp27394vr.png](https://attacker.cc/usr/uploads/2020/12/3989303156.png)

可以看出该系统主要是欧洲各国使用较多，国内不存在该系统。

访问后可以看出该工控系统涉及电力、能源、水利等等领域：

![46532-v0b804sidzk.png](https://attacker.cc/usr/uploads/2020/12/3465725832.png)

![51494-emjl281qcde.png](https://attacker.cc/usr/uploads/2020/12/3203981544.png)

![05691-3ifautshdf9.png](https://attacker.cc/usr/uploads/2020/12/3408235068.png)

最后：Happy Hunting by 360-Quake.