# 空间测绘系列之 - Shodan 云监测功能分析

上接[空间测绘系列之 - Shodan 功能总览](https://attacker.cc/index.php/archives/175/)，本文是对 Shodan 四大版块中**相对独立板块**中的**云监测模块**的分析。

**官方介绍描述**

>   Keep track of the devices that you have exposed to the Internet. Setup notifications, launch scans and gain complete visibility into what you have connected.

可以简述为：通过扫描了解设备在互联网上的暴露面。

![shodan-images.jpg](%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E7%B3%BB%E5%88%97%E4%B9%8B%20-%20Shodan%20%E4%BA%91%E7%9B%91%E6%B5%8B%E5%8A%9F%E8%83%BD%E5%88%86%E6%9E%90.assets/3885382324.jpg)

首先将首页的介绍全部过一遍：



## 0x01 功能特点介绍展开目录



### 1. 联网设备快速发现、变动监测与推送展开目录

>   **Network Monitoring Made Easy.**
>
>   Within 5 minutes of using Shodan Monitor you will see what you currently have connected to the Internet within your network range and be setup with real-time notifications when something unexpected shows up.

-   联网设备快速发现：`5分钟`内发现互联网上暴露的设备
-   变动监测
-   实时消息推送



### 2. 监控规模自适应展开目录

>   **Built to Scale**
>
>   Whether you want to monitor 1 IP or you're an ISP with millions of customers - the Shodan platform was built to handle networks of all sizes without breaking a sweat.

-   一个 IP 也好、百万 IP 也罢，都能监测



### 3. 安全监测展开目录

>   **Security Beyond the Perimeter**
>
>   The Shodan platform helps you monitor not just your known network but also find your devices across the Internet. Detect data leaks to the cloud, phishing websites, compromised databases and more. Shodan gives you the tools to monitor all your connected devices on the Internet.

不仅提供对设备的暴露的监测，还提供一些安全功能：

-   云端数据泄漏
-   钓鱼网站
-   被攻击的数据库
-   ......

其实从个人角度讲，这些功能真的都是小儿科的，比起国内成熟的云监测、云扫描，甚至是拿着漏扫去扫都会比它效果好。

因为 Shodan 的监测太单薄了，“被攻击的数据库、云端数据泄漏” 说白了仅仅只是各种未授权访问，加上获取数据库里面的一些勒索信息而已。

17 年写这个 [MongoDB 勒索事件现状调查报告](https://cert.360.cn/static/files/MongoDB勒索事件现状调查报告.pdf)报告的时候，发现了各类非关系型数据库未授权访问的问题，后续 Shodan、BinaryEdge 也加入了相关的监测，并实现了相应的功能。



### 4. 精简化 Dashboard展开目录

>   **Just the Facts**
>
>   Tired of busy dashboards with too much unnecessary information? Shodan Monitor is designed to help you quickly hone in the most important issues. Take advantage of our years of experience crawling the Internet to provide context and filter out the noise.

-   直击重点的精简大屏

这个简直对现在国内厂商来说简直是暴击啊。从 2017 年到 2020 年这三年间接触到的 To B 业务来看，可视化大屏在一个完整的系统里面真的是太抓人眼球了，在领导参观、重保等场景下必不可缺。一块屏动辄 10w、20w 甚至能更高。

但很多系统中的可视化大屏实际的意义并不大，既不能产出真实、有意义的成果，又不能真正刻画出数据的内涵，仅仅是为了在某些场景下、某些时间内当个摆设，而刻意的炫酷和好看。要知道，炫酷好看！= 数据可视化。我个人认为的数据可视化重点在对数据的理解、刻画和抽象表达，做出的东西并不一定好看，但是一定是直观且深入人心的。

其实关于可视化相关的可以单独再写一个系列了，这里就不再展开。



## 0x02 辅助组件展开目录

![shodan-MONITOR-COMPONENTS.jpg](%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E7%B3%BB%E5%88%97%E4%B9%8B%20-%20Shodan%20%E4%BA%91%E7%9B%91%E6%B5%8B%E5%8A%9F%E8%83%BD%E5%88%86%E6%9E%90.assets/2020177758.jpg)



### 1. 友好的 API展开目录

>   **Developer-friendly API**
>
>   All features of the Shodan Monitor website are also available via the Shodan API and command-line interface.

-   所有的监测功能都可以用 API 和 CLI 的方式调用

要知道，现在市面上的各类空间测绘引擎只有 Shodan 的 API、SDK、CLI 是最多、最全的，算上是自称的 `Developer-friendly`



### 2. 按需扫描展开目录

>   **On-Demand Scanning**
>
>   Use Shodan's global infrastructure to scan networks to confirm that an issue has been fixed.

-   能够调用 Shodan 分布在全世界的扫描引擎进行扫描



### 3. API 订阅展开目录

>   **Batteries Included**
>
>   A subscription to our API plans gives access to Shodan Monitor, the search engine, API and a whole range of websites.

-   通过 API 订阅访问监控系统



### 4. FAQ、操作视频展开目录

![shodan-MONITOR-FAQ-Video.jpg](%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E7%B3%BB%E5%88%97%E4%B9%8B%20-%20Shodan%20%E4%BA%91%E7%9B%91%E6%B5%8B%E5%8A%9F%E8%83%BD%E5%88%86%E6%9E%90.assets/1232964758.jpg)



#### 1. 常见问题展开目录

**能监控多少 IP**

这里让你去充钱后参考开发者页面，其实直接看价格表即可（自己花了多少钱，心里没点数吗）：

![shodan-price-network-monitor.jpg](%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E7%B3%BB%E5%88%97%E4%B9%8B%20-%20Shodan%20%E4%BA%91%E7%9B%91%E6%B5%8B%E5%8A%9F%E8%83%BD%E5%88%86%E6%9E%90.assets/3878814978.jpg)

-   5120 个到 30w 个 IP

**需要手工提交扫描任务吗？**
不需要，Shodan 会自动化持续监测。但是也支持手工创建扫描。

**获取事件原始信息**
可以，在网络告警流（Network Alerts stream）中包含原始信息。



#### 2. 操作视频展开目录

由于 Shodan 这个功能是付费的无法看的，因此这个视频提供了全部的界面和真实使用流程。



## 0x03 云监测流程展开目录



### 1. 添加监测资产展开目录

![shodan-network-monitor-step0.jpg](%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E7%B3%BB%E5%88%97%E4%B9%8B%20-%20Shodan%20%E4%BA%91%E7%9B%91%E6%B5%8B%E5%8A%9F%E8%83%BD%E5%88%86%E6%9E%90.assets/1128404109.jpg)

这里一共有两种，一种是 IP 类型的，另一种是域名。

**网段类型**

这一步主要输入以下信息：

-   监测任务名称
-   IP 地址段（注意，只支持 IP，不支持域名）

![shodan-network-monitor-step1.jpg](%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E7%B3%BB%E5%88%97%E4%B9%8B%20-%20Shodan%20%E4%BA%91%E7%9B%91%E6%B5%8B%E5%8A%9F%E8%83%BD%E5%88%86%E6%9E%90.assets/4243340147.jpg)

在页面右上角可以看到当前账号剩余的 IP 数量。

**域名类型**

这一步主要输入以下信息：

-   域名

![shodan-network-monitor-step1-domain.jpg](%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E7%B3%BB%E5%88%97%E4%B9%8B%20-%20Shodan%20%E4%BA%91%E7%9B%91%E6%B5%8B%E5%8A%9F%E8%83%BD%E5%88%86%E6%9E%90.assets/1922917119.jpg)

注意页面上有一段提示：

>   Shodan Monitor automatically discovers the subdomains and associated IPv4 addresses (A records) of the domain you enter. A network alert is managed based on the DNS information. Anytime your DNS information changes the network monitoring is automatically updated

可以看出，包含如下功能：

-   子域名枚举
-   关联所有域名、子域名对应的 A 记录 IP 地址
-   自动监控 DNS 解析
-   监测跟随 DNS 解析变动

另外当完成输入域名后，在输入框下有一段蓝色的超链接：



### 2. 选择告警推送邮箱展开目录

-   选择消息通知的邮箱

在配置里可以设置多个接收告警的邮箱



### 3. 查看资产监控列表展开目录

![shodan-network-monitor-step2-manage-assets.jpg](%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E7%B3%BB%E5%88%97%E4%B9%8B%20-%20Shodan%20%E4%BA%91%E7%9B%91%E6%B5%8B%E5%8A%9F%E8%83%BD%E5%88%86%E6%9E%90.assets/1570747041.jpg)

可以看出列表主要是三个部分：

-   资产本身的信息（名称、域名；IP、网段；IP 地址数量）
-   监测模块
-   操作按钮（`edit` 编辑、`Rescan Network` 重新扫描、`Remove` 删除）

可以看到监测的模块默认支持：

-   malware
-   open_database
-   iot
-   internet_scanner
-   industrial_control_system
-   new_service
-   ssl_expired
-   vulnerable

其实这些在首页的介绍里面已经出现了大部分，但是在编辑里面还会看到一个默认未启用的模块 `uncommon`，每个选项后面还自带一个解释说明：

![shodan-network-monitor-step3-edit.jpg](%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E7%B3%BB%E5%88%97%E4%B9%8B%20-%20Shodan%20%E4%BA%91%E7%9B%91%E6%B5%8B%E5%8A%9F%E8%83%BD%E5%88%86%E6%9E%90.assets/169550883.jpg)

总结如下：

| 模块名称                  | 原始说明                                                     | 解释翻译                           |
| ------------------------- | ------------------------------------------------------------ | ---------------------------------- |
| industrial_control_system | aria-label="Services associated with industrial control systems" | 工控系统                           |
| malware                   | Compromised or malware-related services                      | 已被攻破的、或者恶意软件相关的服务 |
| open_database             | Database service that does not require authentication        | 未授权访问的数据库                 |
| iot                       | Service associated with Internet of Things devices           | IoT 相关设备开放的服务             |
| internet_scanner          | Device has been seen scanning the Internet and exposes a service | 公网开放的服务                     |
| new_service               | New open port/ service discovered                            | 新发现的端口或服务                 |
| ssl_expired               | Expired SSL certificate is used by the service               | 过期的 SSL 证书                    |
| vulnerable                | Service is vulnerable to a known issue                       | 存在已知漏洞                       |
| uncommon                  | Services that generally shouldn't be publicly available      | 公网上不常见（不应该出现）的服务   |



### 4. 查看 dashboard展开目录

配置好多个列表以后：
![shodan-network-monitor-step5-1.jpg](%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E7%B3%BB%E5%88%97%E4%B9%8B%20-%20Shodan%20%E4%BA%91%E7%9B%91%E6%B5%8B%E5%8A%9F%E8%83%BD%E5%88%86%E6%9E%90.assets/4183187401.jpg)

可以看到 Dashboard：

![shodan-network-monitor-step5-2.jpg](%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E7%B3%BB%E5%88%97%E4%B9%8B%20-%20Shodan%20%E4%BA%91%E7%9B%91%E6%B5%8B%E5%8A%9F%E8%83%BD%E5%88%86%E6%9E%90.assets/1707904824.jpg)

看上去还真的和功能特点中描述的一样：`hone in the most important issues` 简单直白

主要分为三大模块 6 小部分：

1.  数量总计
2.  服务信息
    -   Top Open Ports：Top10 开放端口统计
    -   Notable Ports：Top10 不常见开放端口统计
    -   Top Vulnerabilities：Top5 漏洞
    -   Potential Vulnerabilities：Top5 可能存在的漏洞
3.  网段资产地图

>   A heat map of open ports by IP address. Light red means fewer ports, bright red means more ports and grey means there's no information available.

-   即 IP 网段的热力图，颜色越深，开放端口数量越多

点击漏洞就会进入检索页面进行搜索：`net:xx.xx.xx.xx/24 vuln:MS15-034`

![shodan-network-monitor-vul.jpg](%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E7%B3%BB%E5%88%97%E4%B9%8B%20-%20Shodan%20%E4%BA%91%E7%9B%91%E6%B5%8B%E5%8A%9F%E8%83%BD%E5%88%86%E6%9E%90.assets/3401670223.jpg)



### 5. 告警邮件展开目录

![shodan-network-monitor-email.jpg](%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E7%B3%BB%E5%88%97%E4%B9%8B%20-%20Shodan%20%E4%BA%91%E7%9B%91%E6%B5%8B%E5%8A%9F%E8%83%BD%E5%88%86%E6%9E%90.assets/751776608.jpg)



## 0x04 总结展开目录

通过上述过程，就将 Shodan 云监测相关的全部功能分析完了，最后画个脑图做下梳理：

![Shodan Network Monitor.png](%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E7%B3%BB%E5%88%97%E4%B9%8B%20-%20Shodan%20%E4%BA%91%E7%9B%91%E6%B5%8B%E5%8A%9F%E8%83%BD%E5%88%86%E6%9E%90.assets/3314176722.png)