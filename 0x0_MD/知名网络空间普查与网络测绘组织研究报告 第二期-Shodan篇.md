# 知名网络空间普查与网络测绘组织研究报告 第二期-Shodan篇

[2020/11/16](http://plcscan.org/blog/2020/11/research-report-of-renowned-internet-census-organization-shodan/)  [Z-0ne](http://plcscan.org/blog/author/admin/)

Shodan（[Shodan.io](https://www.shodan.io/)）是一个主机、联网设备搜索引擎，被誉为“黑暗谷歌”和“互联网上最可怕的搜索引擎”，Shodan由John Matherly开发，John Matherly在瑞士出生并长大，17岁时移居美国的圣地亚哥，后在加州大学圣地亚哥分校获得生物信息学学位，曾在圣地亚哥超级计算机中心帮助管理世界上最重要的蛋白质数据库，毕业后，创业独立开发了多款产品，John Matherly于2009发布Shodan，美国安全研究人员Michael Schearer于2010年的DEFCON 18黑客大会，发表了基于Shodan进行渗透测试的主题演讲后，让Shodan逐渐为更多人所熟知。

利用Shodan使用特定的语法即可搜索出海量的物联网设备、摄像头、路由器、打印机、SCADA系统、PLC等。Shodan主要通过收集Web服务器（[HTTP/HTTPS，端口80、8080、443、8443](https://www.shodan.io/search?query=port%3A80)）以及FTP（[端口21](https://www.shodan.io/search?query=port%3A21)），SSH（端口22），Telnet（端口23），SNMP（端口161），IMAP（端口993），SIP（端口5060），实时流协议（RTSP，端口554）等常见应用服务超过300个TCP/UDP网络端口的服务Banner标识（通过Shodan的扫描器通过协议读取开放端口上服务的数据，从而由开放端口返回扫描器的元数据），使用者可通过匹配端口和服务的特征关键字即可搜索到和特征相似的全球联网设备。

Shodan于2013年借鉴[Digitalbond](https://www.digitalbond.com/)开发的工控设备识别开源项目[Redpoint](https://github.com/digitalbond/Redpoint)发布的多个PLC和SCADA系统的指纹特征，从而增加了针对工控协议的探测，用户可以直接使用工控协议的端口直接检索该协议的所有数据，达到了精准识别工控软硬件版本和模块信息，在此之前用户只可以使用特征Dork（一种用于精确匹配设备的关键字）直接搜索对应设备数据。

Shodan于2015年对网站整站进行了一次重大革新，在普通用户方面优化了UI体验，并在主站开放了数据搜索的可视化报告分析功能，从不同纬度分析搜索语法过滤到的数据，另外强化了企业和API功能。

## **功能模块**

### Shodan（https://www.shodan.io/）

Shodan主站对所有互联网用户开放，任何用户均可使用Shodan查询数据，未登录的情况下仅限于字符串搜索，不支持语法和过滤器方式搜索数据。

主站主要由搜索功能和数据分析功能组成：

其搜索功能支持多种语法，并且语法可以相互组合，使用者可用根据自己需求，查询城市，省份，运营商，国家，端口，服务名，关键字等，也可自行根据需求组合来达到地区数据搜索等数据查询需求。

主站同时还支持将搜索语法查询到的数据，自动化通过聚合统计可以生成数字化的图形数据分析报表，按照城市，国家，运营商，端口等维度使用饼图，曲线图等方式对数据进行科学统计和可视化展示。

### **Shodan Scanhub****（https://scanhub.shodan.io/）（目前已停止服务）**

Shodan Scanhub是提供给注册用户的一套“在线版私有Shodan”，Scanhub可以使用Nmap扫描的XML结果，可通过多种方式上传Nmap的扫描结果，另外可解析Nmap的扫描结果，并支持Nmap扫描结果的可视化展示。

用户可使用Nmap扫描网络，将Nmap的XML扫描报告上传至Scanhub即可搭建一个类Shodan一样的数据搜索和展示系统，Scanhub支持和Shodan主站一样的搜索语法与地图可视化展示、数据分析报表。

Shodan Scanhub是一个收费功能，根据空间的大小不同分别为9、49、199、499/美元每月。

### Shodan Maps（https://maps.shodan.io/）

Shodan Maps是Shodan提供的一种基于可视化地图对联网系统进行数据展示的可视化搜索系统，通过搜索语法数据将在全球地图上进行可视化的数据展示。

### **Shodan Images（https://images.shodan.io/）**

Shodan Images是Shodan创新的一个图片流检索系统，Shodan的扫描节点除了常规的服务识别扫描外，还对支持对如：RDP，VNC，HTTP等服务和存在未授权访问的摄像头进行快照截图抓取的功能，使用该功能可以在Shodan Images浏览到海量的系统截图和摄像头画面截图的图片流展示。

### **Shodan Blog（https://blog.shodan.io/）**

Shodan Blog是John Matherly用于发布研究文章的一个博客服务，该博客对互联网所有用户均开放。

### **Shodan Exploits**

Shodan Exploits是Shodan提供的一个漏洞利用库和漏洞信息库，用户可以通过搜索任意关键字，即可查询到漏洞的信息与可能相关的漏洞利用代码，Shodan Exploits数据库集合了Exploitdb、CVE、Metasploit的所有数据，用户可用通过搜索关键字查询3个库中的数据，方便用户直接从海量的漏洞和漏洞利用代码中过滤出对应的漏洞利用代码用于测试。

### **Shodan 3D**

Shodan 3D是Shodan的一套基于WEBGL可视化的3D地球数据搜索与展示系统。

### **Shodan API**

为了让个人用户和企业用户更好的接入Shodan的海量数据和调用Shodan分布式的监测节点，进行企业自建监控服务，Shodan API为个人用户和企业用户提供了RESTful和Streaming流式API接口，让第三方应用与系统可通过接口接入Shodan数据，Shodan API同样是一个收费功能，对于普通1次付费49美元成为会员的用户，每月仅有10000条数据的调用次数，而API根据调用次数的不同分别有59、299、899美元每月的API套餐，可查询的数据结果分别为1000000、20000000、无限制。

## **商业化情况**

Shodan平台经过多年的社区化运作，目前已有成熟的在线运营商业模式，对于任何人均可通过邮箱注册成为Shodan的免费会员用户，但免费用户仅可返回查看查询结果的前20条数据，并且无法使用特定语法和过滤器，任何免费用户可通过在线支付49美元永久成为Shodan的个人收费会员。

目前，Shodan广泛被安全研究人员，安全从业人员，大型企业，CERT使用，拥有1000多所大学教育用户，全球注册用户超过300万，根据公开数据显示Shodan从发布到2015年大约有超过10000名用户一次性支付超过20美元的会员费用成为会员，来获得额外的搜索结果。Shodan拥有超过几十家机构用户，甚至网络安全公司每年支付超过五位数来访问Shodan的整个数据库。

## **引擎架构**

Shodan是一个全球性质的联网主机与设备的搜索监测引擎，其数据库内存储了超过1亿个活跃IP地址开放端口与服务的标识信息，Shodan通过分布式的扫描引擎7*24小时不间断的扫描搜集全球40亿IP地址开放端口的标识信息。

Shodan的扫描引擎基于分布式架构，考虑到不同洲存在的网络差异可能带来扫描过程因为延迟导致的数据丢包情况，以及不同运营商对网络扫描的宽容度，Shodan在全球部署了大量扫描探测节点，广泛分布于北美，欧洲等地，欧洲节点则以网络中立的荷兰数据中心高速dedicated servers为主，数量超过30个，根据[灯塔实验室-物联网威胁情报系统监测](https://iotsec.io/#/detailList?list=org%3Ashodan.io)数据显示（https://iotsec.io/#/detailList?list=org%3Ashodan.io），部分知名节点与节点关注的扫描探测行为如下：

**注：本研究报告部分数据基于实验室监测数据分析得出，不排除与实际运行情况存在差异，如果您对本研究报告的任何细节感兴趣，亦或技术交流、数据或信息更正添加，均可和我们取得联系，邮箱：\*[root@plcscan.org](mailto:root@plcscan.org)*

| 扫描探测节点IP | 节点IP位置 | 关注协议                                                     | 协议行为                                                     |
| -------------- | ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 71.6.146.185   | 美国       | TCP、TNS、DATA、TLS、HTTP、RTSP、X11、EPMD、DNS、GTP、OPCUA、CHARGEN、HART_IP、COTP | SYN、ACK、PSH, ACK、RST、FIN, ACK、FIN, PSH, ACK、GET、HEL、CR Connect Request |
| 71.6.167.142   | 美国       | TCP、DATA、JSON、HTTP、TLS、PGSQL、TFTP、XMPP、IEC60870_104、X11、BACAPP、OPCUA、KERBEROS、STEAM_IHS_DISCOVERY、GOPHER、TACPLUS、DTLS、DRDA、PPTP | SYN、RST、FIN, ACK、PSH, ACK、FIN, PSH, ACK、ACK、POST、GET、RST, ACK、HEL |
| 89.248.167.131 | 荷兰       | TCP、TLS、HTTP、GTP、DATA、MQTT、PORTMAP、PCOMASCII、SSDP、KERBEROS | SYN、FIN, PSH, ACK、RST、ACK、PSH, ACK、GET、FIN, ACK、RST, ACK |
| 185.181.102.18 | 罗马尼亚   | TCP、BFD、TLS、HTTP、MEMCACHE、DATA、LPD、MDNS、LDAP、MQTT、ESP、SNMP、ENIP、PPTP | SYN、ACK、FIN, ACK、RST、PSH, ACK、GET、FIN, PSH, ACK、List Identity |
| 185.142.236.34 | 荷兰       | TCP、DATA、HTTP、TIBIA、VNC、X11、LPD、TLS、DNS、RMI、MDNS、JSON、XMPP、SSDP、MQTT、AJP13、MEMCACHE、DICOM、SIP、OPENVPN | SYN、PSH, ACK、FIN, PSH, ACK、RST、ACK、FIN, ACK、GET、POST  |
| 66.240.205.34  | 美国       | TCP、IEC60870_104、DATA、TLS、HTTP、PMPROXY                  | SYN、ACK、PSH, ACK、RST、FIN, ACK、FIN, PSH, ACK、GET        |
| 195.144.21.56  | 乌克兰     | TCP、TLS、HTTP、NBNS、DATA、ESP、TDS、IEC60870_104、EPMD、RIP、BITCOIN、DSI、DTLS、JSON、KIP、SIP、IPSICTL、STEAM_IHS_DISCOVERY、OPCUA、GEARMAN、BACAPP、KERBEROS、DICOM | SYN、RST、ACK、GET、PSH, ACK、FIN, ACK、FIN, PSH, ACK、POST、HEL |
| 66.240.236.119 | 美国       | TCP、HTTP、TLS、DICOM、DATA、DNS、STEAM_IHS_DISCOVERY、GOPHER、PPTP、COTP、GEARMAN、RMI、SNMP、ENIP、RIP、RTSP、WHOIS、FINGER | SYN、RST、ACK、GET、FIN, ACK、PSH, ACK、FIN, PSH, ACK、CR Connect Request、List Identity |
| 80.82.77.139   | 荷兰       | TCP、TIBIA、DATA、HTTP、DNS、GTP、BACAPP、TLS、TFTP、STEAM_IHS_DISCOVERY、JSON、SSDP、DTLS、X11、CHARGEN、ENIP、COTP、KERBEROS、DICOM、LPD、RTSP、BITCOIN、MEMCACHE、PGSQL、COAP、LDAP、TNS、PCOMASCII、AJP13、SIP、IEC60870_104、XMPP、ECHO、WHOIS、DRDA、GIT、RMI | SYN、FIN, ACK、FIN, PSH, ACK、PSH, ACK、ACK、RST、GET、POST、List Identity、CR Connect Request、RST, ACK |
| 71.6.199.23    | 美国       | TCP、DATA、HTTP、FINGER、TDS、DNP3、OMRON、TLS、ENIP、SMTP、DRDA、RTSP、XMPP、DSI、AJP13、EPMD、JSON、GTP、BGP、SNMP、MEMCACHE、PCOMASCII、GEARMAN、DICOM、CLDAP、OPENVPN、WHOIS、KIP、IEC60870_104、PORTMAP、TIBIA、_WS.MALFORMED | SYN、ACK、FIN, ACK、PSH, ACK、FIN, PSH, ACK、RST、GET、Request Link Status、List Identity、DATA、POST、RST, ACK |
| 185.142.239.16 | 荷兰       | TCP、ENIP、HTTP、CLDAP、DATA、TLS、AJP13、IPSICTL、COTP、TFTP、SNMP、GTP、SSDP、GIT、COAP、DNP3、DNS、AMQP、SMTP、BGP、BFD、MQTT、DTLS、JSON、XMPP | SYN、List Identity、FIN, ACK、ACK、RST、FIN, PSH, ACK、GET、PSH, ACK、CR Connect Request、Request Link Status、DATA、POST |
| 94.102.49.193  | 荷兰       | TCP、DICOM、DATA、STEAM_IHS_DISCOVERY、TACPLUS、GOPHER、SNMP、RMI、GEARMAN、HTTP、TNS、DTLS、EPMD、JSON、KIP、X11、TLS、SSDP、TIBIA、ESP、COTP、ECHO、PCOMASCII、GTP、IPSICTL、PGSQL、OPENVPN、CHARGEN、DSI、GIT、OMRON、WHOIS、DNP3、DRDA、TDS、MEMCACHE | SYN、FIN, PSH, ACK、PSH, ACK、ACK、RST、FIN, ACK、GET、POST、CR Connect Request、RST, ACK、Request Link Status |
| 93.174.95.106  | 荷兰       | TCP、HTTP、DATA、COTP、KIP、ECHO、DRDA、DICOM、SSDP、TLS、MEMCACHE、AJP13、OMRON、JSON、HART_IP、CLDAP、LPD、TIBIA、BFD、GTP、X11、OPCUA、DNP3、BGP、MQTT、ECATF、DSI、PPTP、RTSP、RMI | SYN、RST、ACK、GET、FIN, ACK、PSH, ACK、FIN, PSH, ACK、CR Connect Request、POST、RST, ACK、HEL、Request Link Status |
| 89.248.172.16  | 荷兰       | TCP、HTTP、GTP、SIP、NTP、DNS、TLS、DATA、COTP、DNP3、GIT、PPTP、GEARMAN、ENIP、SMTP、CLDAP | SYN、RST、FIN, ACK、ACK、GET、FIN, PSH, ACK、PSH, ACK、CR Connect Request、Request Link Status、List Identity、DATA、RST, ACK |
| 198.20.70.114  | 美国       | TCP、TLS、HTTP、DATA、EPMD、SNMP、STEAM_IHS_DISCOVERY、LDAP、TACPLUS、LPD、COTP、ENIP、SSDP、X11、JSON | SYN、RST、ACK、PSH, ACK、FIN, ACK、FIN, PSH, ACK、GET、CR Connect Request、List Identity、POST |
| 185.142.236.35 | 荷兰       | TCP、TLS、HTTP、DATA、IPSICTL、RMI、AMQP、ENIP、BGP、ECHO、X11、GEARMAN、HART_IP、DTLS、MQTT、JSON、RIP、SSDP、TDS、RTSP、COAP、TNS | SYN、ACK、PSH, ACK、GET、FIN, PSH, ACK、RST、FIN, ACK、List Identity、RST, ACK、POST |
| 66.240.192.138 | 美国       | TCP、HTTP、TLS、MQTT、MEMCACHE、DATA、CHARGEN、BGP、SMTP、TNS、KIP、COTP、GIT、TFTP、GTP、DICOM、TDS、RTSP、RIP、DNS、OMRON、JSON、TACPLUS | SYN、RST、GET、ACK、PSH, ACK、FIN, ACK、FIN, PSH, ACK、DATA、RST, ACK、CR Connect Request、POST |
| 80.82.77.33    | 荷兰       | TCP、DTLS、HTTP、ECHO、COTP、TLS、VNC、ESP、BFD、DATA、DICOM、LDAP、RTSP、DNS、GEARMAN、TDS、GTP、KIP、DNP3、KERBEROS、OPENVPN、NTP、XMPP、OMRON、ENIP、SSDP、GOPHER、AJP13、TFTP、OPCUA、CHARGEN、JSON、MEMCACHE、IPSICTL、TIBIA | SYN、GET、FIN, ACK、ACK、RST、PSH, ACK、FIN, PSH, ACK、CR Connect Request、Request Link Status、List Identity、HEL、POST |
| 71.6.158.166   | 美国       | TCP、DATA、HTTP、OPCUA、AJP13、JSON、TIBIA、SMTP、DRDA、DICOM、TLS、X11、DNS、MODBUS、RIP、GEARMAN、PPTP、FINGER、DTLS、PCOMASCII、LPD | SYN、RST、ACK、FIN, ACK、PSH, ACK、FIN, PSH, ACK、GET、HEL、POST、DATA、Report Slave ID |
| 185.165.190.34 | 荷兰       | TCP、DATA、TLS、ENIP、HTTP、GTP、TACPLUS、XMPP、DTLS、MEMCACHE、BGP、PPTP、KIP、DRDA、IPSICTL、DICOM、BACAPP、GIT、SMTP | SYN、ACK、PSH, ACK、RST、FIN, ACK、FIN, PSH, ACK、List Identity、GET、DATA |
| 71.6.199.23    | 美国       | TCP、DATA、HTTP、FINGER、TDS、DNP3、OMRON、TLS、ENIP、SMTP、DRDA、RTSP、XMPP、DSI、AJP13、EPMD、JSON、GTP、BGP、SNMP、MEMCACHE、PCOMASCII、GEARMAN、DICOM、CLDAP、OPENVPN、WHOIS、KIP、IEC60870_104、PORTMAP、TIBIA、_WS.MALFORMED | SYN、ACK、FIN, ACK、PSH, ACK、FIN, PSH, ACK、RST、GET、Request Link Status、List Identity、DATA、POST、RST, ACK |
| 185.142.239.16 | 荷兰       | TCP、ENIP、HTTP、CLDAP、DATA、TLS、AJP13、IPSICTL、COTP、TFTP、SNMP、GTP、SSDP、GIT、COAP、DNP3、DNS、AMQP、SMTP、BGP、BFD、MQTT、DTLS、JSON、XMPP | SYN、List Identity、FIN, ACK、ACK、RST、FIN, PSH, ACK、GET、PSH, ACK、CR Connect Request、Request Link Status、DATA、POST |
| 94.102.49.193  | 荷兰       | TCP、DICOM、DATA、STEAM_IHS_DISCOVERY、TACPLUS、GOPHER、SNMP、RMI、GEARMAN、HTTP、TNS、DTLS、EPMD、JSON、KIP、X11、TLS、SSDP、TIBIA、ESP、COTP、ECHO、PCOMASCII、GTP、IPSICTL、PGSQL、OPENVPN、CHARGEN、DSI、GIT、OMRON、WHOIS、DNP3、DRDA、TDS、MEMCACHE | SYN、FIN, PSH, ACK、PSH, ACK、ACK、RST、FIN, ACK、GET、POST、CR Connect Request、RST, ACK、Request Link Status |
| 185.142.236.35 | 荷兰       | 协议：TCP、TLS、HTTP、DATA、IPSICTL、RMI、AMQP、ENIP、BGP、ECHO、X11、GEARMAN、HART_IP、DTLS、MQTT、JSON、RIP、SSDP、TDS、RTSP、COAP、TNS | SYN、ACK、PSH, ACK、GET、FIN, PSH, ACK、RST、FIN, ACK、List Identity、RST, ACK、POST |
| 71.6.147.254   | 美国       | TCP、HTTP                                                    | SYN、ACK、RST、GET、FIN, ACK、FIN, PSH, ACK、PSH, ACK        |
| 66.240.219.146 | 美国       | TCP、TLS、HTTP、FINGER、AMQP、XMPP、DATA、DICOM、ENIP、SMTP、GTP、ESP、WHOIS、NBNS、DTLS、RMI、MEMCACHE、IEC60870_104 | SYN、ACK、RST、FIN, ACK、PSH, ACK、GET、FIN, PSH, ACK、List Identity、DATA、RST, ACK |
| 198.20.87.98   | 美国       | TCP、HTTP、SOCKS、ELASTICSEARCH                              | SYN、ACK、RST、FIN, ACK、GET、FIN, PSH, ACK、PSH, ACK        |
| 185.142.236.40 | 荷兰       | TCP、HTTP                                                    | SYN、GET、ACK、FIN, ACK、FIN, PSH, ACK                       |
| 185.142.236.43 | 荷兰       | TCP、TLS、HTTP                                               | SYN、ACK、GET、PSH, ACK、FIN, ACK、FIN, PSH, ACK             |
| 82.221.105.6   | 冰岛       | TCP                                                          | ACK、SYN                                                     |
| 82.221.105.7   | 冰岛       | TCP、TLS、HTTP                                               | SYN、PSH, ACK、GET、ACK、FIN, PSH, ACK                       |
| 107.6.151.194  | 荷兰       | TCP                                                          | SYN                                                          |

## **核心能力**

Shodan的核心能力主要来自于如下几个方面：
1、对于设备、应用、服务的通信协议识别探测支持的数量多、维度覆盖广，从协议层面覆盖了TCP/UDP类型，从监测资产类型方面覆盖了传统IT类型资产、IoT物联网资产和ICS工控资产，探测协议超过200种，其中工控类型探测协议支持超过30种；（详见附录一、Shodan扫描探测协议支持情况）
2、协议扫描识别过程中对于TCP/UDP端口覆盖广，Shodan目前支持对全球超过1000个TCP/UDP端口进行识别；（详见附录二、Shodan覆盖端口情况）
3、在网络中立区域、对网络扫描的宽容度高的数据中心，具有稳定的高速、独立的网络监测节点，保证了监测端口的数量覆盖和监测数据的实时性。（Shodan扫描节点网络活动详见：由灯塔实验室出品的物联网安全威胁情报搜索引擎监测数据https://www.iotsec.io/#/detailList?list=org%3Ashodan.io）

## 附录一、Shodan扫描探测协议支持情况

| 协议类型                    | 协议描述                                                     | 是否具有工控属性 |
| --------------------------- | ------------------------------------------------------------ | ---------------- |
| afp                         | AFP server information grabbing module                       |                  |
| ajp                         | Check whether the Tomcat server running AJP protocol         |                  |
| amqp                        | Grab information from an AMQP service                        |                  |
| andromouse                  | Checks whether the device is running the remote mouse AndroMouse service. |                  |
| apple-airport-admin         | Check whether the device is an Apple AirPort administrative interface. |                  |
| ard                         | Query the Apple Remote Desktop service for information about the device |                  |
| automated-tank-gauge        | Get the tank inventory for a gasoline station.               | 是               |
| bacnet                      | Gets various information from a BACnet device.               | 是               |
| beanstalk                   | Get general information about the Beanstalk daemon           |                  |
| bgp                         | Checks whether the device is running BGP.                    |                  |
| bitcoin                     | Grabs information about a Bitcoin daemon, including any devices connected to it. |                  |
| bittorrent-tracker          | Check whether there is a BitTorrent tracker running.         |                  |
| blackshades                 | Determine whether a server is running a Blackshades C&C      |                  |
| cassandra                   | Get cluster information for the Cassandra database software. |                  |
| checkpoint-hostname         | Get hostnames for the CheckPoint firewall and management station. |                  |
| cisco-smi                   | Check whether the device supports the Cisco Smart Install feature. |                  |
| citrix-apps                 | This module attempts to query Citrix Metaframe ICA server to obtain a published list of applications. |                  |
| clamav                      | Determine whether a server is running ClamAV                 |                  |
| coap                        | Check whether the server supports the CoAP protocol          |                  |
| coap-dtls                   | Check whether the server supports the CoAP protocol with DTLS |                  |
| codesys                     | Grab a banner for Codesys daemons                            | 是               |
| consul                      | Determine wether consul is running & collect relevant info   |                  |
| couchdb                     | HTTP banner grabbing module                                  |                  |
| crestron                    | Checks for other servers with the same serial number on the local network. AAAAAA is a dummy value. |                  |
| dahua-dvr                   | Grab the serial number from a Dahua DVR device.              |                  |
| darktrack-rat               | Checks whether the device is a C2 for DarkTrack RAT.         |                  |
| dhcp                        | Send a DHCP INFORM request to learn about the lease information from the DHCP server. |                  |
| dht                         | Gets a list of peers from a DHT node.                        |                  |
| dicom                       | Checks whether the DICOM service is running.                 |                  |
| dictionary                  | Connects to a dictionary server using the DICT protocol.     |                  |
| dnp3                        | A dump of data from a DNP3 outstation                        | 是               |
| dns-tcp                     | Try to determine the version of a DNS server by grabbing version.bind |                  |
| dns-udp                     | Try to determine the version of a DNS server by grabbing version.bind |                  |
| echo-udp                    | Checks whether the device is running echo.                   |                  |
| epmd                        | Get a list of Erlang services and the ports they are listening on |                  |
| etcd                        | Etcd cluster information                                     |                  |
| ethereum-rpc                | Grabs version information about the Ethereum node.           |                  |
| ethernetip                  | Grab information from a device supporting EtherNet/IP over TCP | 是               |
| ethernetip-udp              | Grab information from a device supporting EtherNet/IP over UDP | 是               |
| flux-led                    | Grab the current state from a Flux LED light bulb.           |                  |
| fox                         | Grabs a banner for proprietary FOX protocol by Tridium       | 是               |
| ftp                         | Grab the FTP banner                                          |                  |
| gardasoft-vision            | Grabs the version for the Gardasoft controller.              | 是               |
| gearman                     | Gather usage information from a Gearman queue                |                  |
| general-electric-srtp       | Check whether the GE SRTP service is active on the device.   | 是               |
| ghost-rat                   | Checks whether the device is a C2 for Gh0st RAT.             |                  |
| git                         | Check whether git is running.                                |                  |
| gtp-v1                      | Checks whether the device is running a GPRS Tunnel.          |                  |
| hart-ip-udp                 | Checks whether the IP is a HART-IP gateway.                  | 是               |
| hbase                       | Grab the status page for HBase database software.            |                  |
| hbase-old                   | Grab the status page for old, deprecated HBase database software. |                  |
| hddtemp                     | View hard disk information from hddtemp service.             |                  |
| hifly                       | Checks whether the HiFly lighting control is running.        |                  |
| http                        | HTTP banner grabbing module                                  |                  |
| http-simple-new             | HTTP banner grabber only (no robots, sitemap etc.)           |                  |
| http-supermicro             | HTTP banner grabbing module for Supermicro servers           |                  |
| https                       | HTTPS banner grabbing module                                 |                  |
| https-simple-new            | HTTPS banner grabber only (no robots, sitemap etc.)          |                  |
| ibm-db2-das                 | Grab basic information about the IBM DB2 Database Server.    |                  |
| ibm-db2-drda                | Checks for support of the IBM DB2 DRDA protocol.             |                  |
| ibm-nje                     | Check whether the z/OS Network Job Entry service is running. |                  |
| identd                      | Check whether the service is running identd                  |                  |
| idera                       | Grab target system info through Idera uptime agent system    |                  |
| idevice                     | Connects to an iDevice and grabs the property list.          |                  |
| iec-104                     | Banner grabber for the IEC-104 protocol.                     | 是               |
| iec-61850                   | MMS protocol                                                 | 是               |
| ike                         | Checks wheter a device is running a VPN using IKE.           |                  |
| ike-nat-t                   | Checks wheter a device is running a VPN using IKE and NAT traversal. |                  |
| ikettle                     | Check whether the device is a coffee machine/ kettle.        |                  |
| imap                        | Get the welcome message of the IMAP server                   |                  |
| imap-ssl                    | Get the welcome message of the secure IMAP server            |                  |
| insteon-plm                 | Checks whether the device is Insteon PLM type                |                  |
| iota-rpc                    | Grabs version information about the IOTA node.               |                  |
| ipmi                        | Checks whether a device is running IPMI remote management software. |                  |
| iscsi                       | Determine whether a server is an iSCSI target                |                  |
| java-rmi                    | Check whether the device is running Java RMI.                |                  |
| kafka                       | Get information about a Kafka cluster.                       |                  |
| kamstrup                    | Kamstrup Smart Meters                                        | 是               |
| kerberos                    | Checks whether a device is running the Kerberos authentication daemon. |                  |
| kilerrat                    | Determine whether a server is running a KilerRAT C&C         |                  |
| knx                         | Grabs the description from a KNX service.                    | 是               |
| language-server-protocol    | Checks whether the port is running a language server.        |                  |
| lantronix-udp               | Attempts to grab the setup object from a Lantronix device.   | 是               |
| ldap-tcp                    | LDAP banner grabbing module                                  |                  |
| ldap-udp                    | CLDAP banner grabbing module                                 |                  |
| ldaps                       | LDAPS banner grabbing module                                 |                  |
| libreoffice-impress         | Check whether the LibreOffice Impress Remote Server is enabled |                  |
| lifx                        | Check whether there is a BitTorrnt tracker running.          |                  |
| line-printer-daemon         | Get a list of jobs in the print queue to verify the device is a printer. |                  |
| matrikon-opc                | Checks whether the device is running Matrikon OPC.           | 是               |
| mdns                        | Perform a DNS-based service discovery over multicast DNS     |                  |
| melsec-q-tcp                | Get the CPU information from a Mitsubishi Electric Q Series PLC. | 是               |
| melsec-q-udp                | Get the CPU information from a Mitsubishi Electric Q Series PLC. | 是               |
| memcache                    | Get general information about the Memcache daemon            |                  |
| memcache-udp                | Get general information about the Memcache daemon responding on UDP |                  |
| microhard                   | Checks whether the device is running Microhard.              |                  |
| mikrotik-routeros           | Check whether the device operates the Oracle Weblogic T3 protocol |                  |
| minecraft                   | Gets the server status information from a Minecraft server   |                  |
| modbus                      | Grab the Modbus device information via functions 17 and 43.  | 是               |
| monero-rpc                  | Collect information about the Monero daemon.                 |                  |
| mongodb                     | Collects system information from the MongoDB daemon.         |                  |
| moxa-nport                  | Attempts to grab information from Moxna Nport devices.       | 是               |
| mqtt                        | Grab a list of recent messages from an MQTT broker.          |                  |
| ms-sql                      | Check whether the MS-SQL database server is running          |                  |
| ms-sql-monitor              | Pings an MS-SQL Monitor server                               |                  |
| mumble-server               | Grabs the version information for the Murmur service (Mumble server) |                  |
| munin                       | Check whether a Munin node is active and list its plugins    |                  |
| mysql                       | Grabs the version of the running MySQL server                |                  |
| nanocore-122-rat            | Checks whether the device is a C2 for NanoCore Version 1.2.2.0 Cracked |                  |
| nanocore-rat                | Checks whether the device is a C2 for NanoCore RAT.          |                  |
| natpmp                      | Checks whether NAT-PMP is exposed on the device.             |                  |
| netbios                     | Grab NetBIOS information including the MAC address.          |                  |
| netmobility                 | Checks whether the device is a NetMobility.                  |                  |
| newline-tcp                 | Connect to a server with TCP and send a newline.             |                  |
| newline-udp                 | Connect to a server with UDP and send a newline.             |                  |
| njrat                       | Determine whether a server is running a njRAT C&C            |                  |
| nntp                        | Get the welcome message of a Network News server             |                  |
| nodata-dtls                 | Check whether the service supports DTLS and store whatever is returned |                  |
| nodata-tcp                  | Connect to a server without sending any data and store whatever it returns. |                  |
| nodata-tcp-small            | Connect to a server without sending any data and store whatever it returns. |                  |
| nodata-tcp-ssl              | Connect to a server using SSL and without sending any data.  |                  |
| ntp                         | Get a list of IPs that NTP server recently saw and try to get version info. |                  |
| nuclear-rat                 | Checks whether the device is a C2 for Nuclear RAT.           |                  |
| omron-tcp                   | Gets information about the Omron PLC.                        | 是               |
| onvif                       | Check whether the Onvif camera is operating.                 |                  |
| opc-ua                      | Grab a list of nodes from an OPC UA service                  | 是               |
| open-tcp                    | Checks whether a port is open and nothing else.              |                  |
| openvpn                     | Checks whether the other server runs an OpenVPN that doesnt require TLS auth |                  |
| oracle-tns                  | Check whether the Oracle TNS Listener is running.            |                  |
| orcus-rat                   | Checks whether the device is a C2 for Gh0st RAT.             |                  |
| pcanywhere-status           | Asks the PC Anywhere status daemon for basic information.    |                  |
| pcworx                      | Gets information about PC Worx device.                       | 是               |
| plc5                        | Checks whether the device is running Poison Ivy.             | 是               |
| poison-ivy-rat              | Checks whether the device is running Poison Ivy.             |                  |
| pop3                        | Grab the POP3 welcome message                                |                  |
| pop3-ssl                    | Grab the secure POP3 welcome message                         |                  |
| portmap-tcp                 | Get a list of processes that are running and their ports.    |                  |
| portmap-udp                 | Get a list of processes that are running and their ports.    |                  |
| postgresql                  | Collects system information from the PostgreSQL daemon       |                  |
| pptp                        | Connect via PPTP                                             |                  |
| printer-job-language        | Get the current output from the status display on a printer  |                  |
| proconos                    | Gets information about the PLC via the ProConOs protocol.    | 是               |
| qrat                        | Determine whether a server is running a QRAT C&C             |                  |
| quic                        | Checks whether a service supports the QUIC HTTP protocol     |                  |
| rdate                       | Get the time from a remote rdate server                      |                  |
| rdp                         | RDP banner grabbing module                                   |                  |
| realport                    | Get the banner for the Digi Realport device                  | 是               |
| redis                       | Redis banner grabbing module                                 |                  |
| redlion-crimson3            | A fingerprint for the Red Lion HMI devices running CrimsonV3 | 是               |
| remcos-pro-rat              | Checks whether the device is a C2 for RemCos Pro 2.05        |                  |
| riak                        | Sends a ServerInfo request to Riak                           |                  |
| rip                         | Checks whether the device is running the Routing Information Protocol. |                  |
| ripple-rtxp                 | Grabs the list of peers from an RTXP Ripple daemon.          |                  |
| rsync                       | Get a list of shares from the rsync daemon.                  |                  |
| rtsp-tcp                    | Determine which options the RTSP server allows.              |                  |
| s7                          | Communicate using the S7 protocol and grab the device identifications. | 是               |
| sap-router                  | Check whether the SAP Router is active                       |                  |
| scpi                        | Check for the SCPI protocol used by lab equipment            |                  |
| secure-fox                  | Grabs a banner for proprietary FOX protocol by Tridium       | 是               |
| serialnumbered              | Checks for other servers with the same serial number on the local network. AAAAAA is a dummy value. |                  |
| sip                         | Gets the options that the SIP device supports.               |                  |
| smarter-coffee              | Checks the device status of smart coffee machines.           |                  |
| smb                         | Grab a list of shares exposed through the Server Message Block service |                  |
| smtp                        | Get basic SMTP server response                               |                  |
| smtps                       | Grab a banner and certificate for SMTPS servers              |                  |
| snmp                        | Performs an SNMP walk of the system OID                      |                  |
| ssh                         | Get the SSH banner, its host key and fingerprint             |                  |
| statsd-admin                | Gathers statistics from the StatsD service.                  |                  |
| steam-a2s                   | Get a list of IPs that NTP server recently saw and try to get version info. |                  |
| steam-dedicated-server-rcon | Checks whether an IP is running as a Steam dedicated game server with remote authentication enabled. |                  |
| steam-ihs                   | Steam In-Home Streaming protocol                             |                  |
| tacacs                      | Check whether the device supports TACACS+ AAA.               |                  |
| tc-b                        | Cursory check whether a device is running the TC-B protocol  |                  |
| teamviewer                  | Determine whether a server is running TeamViewer             |                  |
| telnet                      | Telnet banner grabbing module                                |                  |
| telnets                     | Telnet wrapped in SSL banner grabbing module                 |                  |
| teradici-pcoip              | Check whether the device is running Teradici PCoIP Management Console. |                  |
| teradici-pcoip-old          | Check whether the device is running Teradici PCoIP Management Console. |                  |
| tibia                       | Grab general information from Open Tibia servers             |                  |
| tor-control                 | Checks whether a device is running the Tor control service.  |                  |
| tor-versions                | Checks whether the device is running the Tor OR protocol.    |                  |
| toshiba-pos                 | Grabs device information for the IBM/ Toshiba 4690.          |                  |
| tuya                        | Check whether a device supports the Tuya API                 |                  |
| ubiquiti-discover           | Grabs information about the Ubiquiti-powered device          |                  |
| udpxy                       | Udpxy banner grabbing module                                 |                  |
| unitronics-pcom             | Collects device information for Unitronics PLCs via PCOM protocol. |                  |
| upnp                        | Collects device information via UPnP.                        |                  |
| vault                       | Determine wether vault is running & collect relevant info    |                  |
| ventrilo                    | Gets the detailed status information from a Ventrilo server. |                  |
| vertx-edge                  | Checks whether the device is running the VertX/ Edge door controller. |                  |
| voldemort                   | Pings the Voldemort database.                                |                  |
| wdbrpc                      | Checks whehter the WDB agent (used for debugging) is enabled on a VxWorks device. |                  |
| weblogic-t3                 | Check whether the device operates the Oracle Weblogic T3 protocol |                  |
| wemo-http                   | Connect to a Wemo Link and grab the setup.xml file           |                  |
| whois                       | Check whether the port is running WHOIS                      |                  |
| x11                         | Connect to X11 w/ no auth and grab the resulting banner.     |                  |
| xiaongmai-backdoor          | Detect backdoor in xiaongmai devices.                        |                  |
| xmpp                        | Sends a hello request to the XMPP daemon                     |                  |
| yahoo-smarttv               | Checks whether the device is running the Yahoo Smart TV device communication service. |                  |
| zookeeper                   | Grab statistical information from a Zookeeper node”          |                  |

## 附录二、Shodan覆盖端口情况

| 类型                      | TCP/UDP覆盖情况（1236个）                                    |
| ------------------------- | ------------------------------------------------------------ |
| Shodan扫描TCP/UDP端口情况 | 7，11，13，15，17，19，20，21，22，23，24，25，26，37，38，43，49，51，53，69，70，79，80，81，82，83，84，85，86，87，88，89，90，91，92，95，96，97，98，99，100，102，104，106，110，111，113，119，121，123，129，131，137，139，143，154，161，175，179，180，195，199，211，221，222，225，263，264，311，340，389，443，444，445，447，448，449，450，465，491，500，502，503，515，520，522，523，541，548，554，555，587，593，623，626，631，636，646，666，675，685，771，772，777，789，800，801，805，806，808，830，843，873，880，888，902，943，990，992，993，994，995，999，1000，1010，1012，1022，1023，1024，1025，1026，1027，1028，1029，1050，1063，1080，1099，1110，1111，1119，1167，1177，1194，1200，1234，1250，1290，1311，1344，1355，1366，1388，1400，1433，1434，1471，1494，1500，1515，1521，1554，1588，1599，1604，1650，1660，1723，1741，1777，1800，1820，1830，1833，1883，1900，1901，1911，1935，1947，1950，1951，1962，1981，1990，1991，2000，2001，2002，2003，2006，2008，2010，2012，2018，2020，2021，2022，2030，2048，2049，2050，2051，2052，2053，2054，2055，2056，2057，2058，2059，2060，2061，2062，2063，2064，2065，2066，2067，2068，2069，2070，2077，2079，2080，2081，2082，2083，2086，2087，2095，2096，2100，2111，2121，2122，2123，2126，2150，2152，2181，2200，2201，2202，2211，2220，2221，2222，2223，2225，2232，2233，2250，2259，2266，2320，2323，2332，2345，2351，2352，2375，2376，2379，2382，2404，2443，2455，2480，2506，2525，2548，2549，2550，2551，2552，2553，2554，2555，2556，2557，2558，2559，2560，2561，2562，2563，2566，2567，2568，2569，2570，2572，2598，2601，2602，2626，2628，2650，2701，2709，2761，2762，2806，2985，3000，3001，3002，3005，3048，3049，3050，3051，3052，3053，3054，3055，3056，3057，3058，3059，3060，3061，3062，3063，3066，3067，3068，3069，3070，3071，3072，3073，3074，3075，3076，3077，3078，3079，3080，3081，3082，3083，3084，3085，3086，3087，3088，3089，3090，3091，3092，3093，3094，3095，3096，3097，3098，3099，3100，3101，3102，3103，3104，3105，3106，3107，3108，3109，3110，3111，3112，3113，3114，3115，3116，3117，3118，3119，3120，3121，3128，3129，3200，3211，3221，3260，3270，3283，3299，3306，3307，3310，3311，3333，3337，3352，3386，3388，3389，3391，3400，3401，3402，3403，3404，3405，3406，3407，3408，3409，3410，3412，3443，3460，3479，3498，3503，3521，3522，3523，3524，3541，3542，3548，3549，3550，3551，3552，3554，3555，3556，3557，3558，3559，3560，3561，3562，3563，3566，3567，3568，3569，3570，3671，3689，3690，3702，3749，3780，3784，3790，3791，3792，3793，3794，3838，3910，3922，3950，3951，3952，3953，3954，4000，4001，4002，4010，4022，4040，4042，4043，4063，4064，4070，4100，4117，4118，4157，4190，4200，4242，4243，4282，4321，4369，4430，4433，4443，4444，4445，4482，4500，4505，4506，4523，4524，4545，4550，4567，4643，4646，4664，4700，4730，4734，4747，4782，4786，4800，4808，4840，4848，4911，4949，4999，5000，5001，5002，5003，5004，5005，5006，5007，5008，5009，5010，5025，5050，5060，5070，5080，5090，5094，5122，5150，5172，5190，5201，5209，5222，5269，5280，5321，5353，5357，5400，5431，5432，5443，5446，5454，5494，5500，5542，5552，5555，5560，5567，5568，5569，5577，5590，5591，5592，5593，5594，5595，5596，5597，5598，5599，5600，5601，5602，5603，5604，5605，5606，5607，5608，5609，5632，5672，5673，5683，5684，5800，5801，5822，5853，5858，5900，5901，5906，5907，5908，5909，5910，5938，5984，5985，5986，6000，6001，6002，6003，6004，6005，6006，6007，6008，6009，6010，6036，6080，6102，6161，6262，6264，6308，6352，6363，6379，6443，6464，6503，6510，6511，6512，6543，6550，6560，6561，6565，6580，6581，6588，6590，6600，6601，6602，6603，6605，6622，6650，6662，6664，6666，6667，6668，6697，6748，6789，6881，6887，6955，6969，6998，7000，7001，7002，7003，7004，7005，7010，7014，7070，7071，7080，7081，7090，7170，7171，7218，7401，7415，7433，7443，7444，7445，7465，7474，7493，7500，7510，7535，7537，7547，7548，7634，7654，7657，7676，7700，7776，7777，7778，7779，7788，7887，7979，7998，7999，8000，8001，8002，8003，8004，8005，8006，8007，8008，8009，8010，8011，8012，8013，8014，8015，8016，8017，8018，8019，8020，8021，8022，8023，8024，8025，8026，8027，8028，8029，8030，8031，8032，8033，8034，8035，8036，8037，8038，8039，8040，8041，8042，8043，8044，8045，8046，8047，8048，8049，8050，8051，8052，8053，8054，8055，8056，8057，8058，8060，8064，8066，8069，8071，8072，8080，8081，8082，8083，8084，8085，8086，8087，8088，8089，8090，8091，8092，8093，8094，8095，8096，8097，8098，8099，8100，8101，8102，8103，8104，8105，8106，8107，8108，8109，8110，8111，8112，8118，8123，8126，8139，8140，8143，8159，8180，8181，8182，8184，8190，8200，8222，8236，8237，8238，8239，8241，8243，8248，8249，8251，8252，8282，8291，8333，8334，8383，8401，8402，8403，8404，8405，8406，8407，8408，8409，8410，8411，8412，8413，8414，8415，8416，8417，8418，8419，8420，8421，8422，8423，8424，8425，8426，8427，8428，8429，8430，8431，8432，8433，8442，8443，8444，8445，8446，8447，8448，8500，8513，8545，8553，8554，8585，8586，8590，8602，8621，8622，8623，8637，8649，8663，8666，8686，8688，8700，8733，8765，8766，8767，8779，8782，8784，8787，8788，8789，8790，8791，8800，8801，8802，8803，8804，8805，8806，8807，8808，8809，8810，8811，8812，8813，8814，8815，8816，8817，8818，8819，8820，8821，8822，8823，8824，8825，8826，8827，8828，8829，8830，8831，8832，8833，8834，8835，8836，8837，8838，8839，8840，8841，8842，8843，8844，8845，8846，8847，8848，8849，8850，8851，8852，8853，8854，8855，8856，8857，8858，8859，8860，8861，8862，8863，8864，8865，8866，8867，8868，8869，8870，8871，8872，8873，8874，8875，8876，8877，8878，8879，8880，8881，8885，8887，8888，8889，8890，8891，8899，8935，8969，8988，8989，8990，8991，8993，8999，9000，9001，9002，9003，9004，9005，9006，9007，9008，9009，9010，9011，9012，9013，9014，9015，9016，9017，9018，9019，9020，9021，9022，9023，9024，9025，9026，9027，9028，9029，9030，9031，9032，9033，9034，9035，9036，9037，9038，9039，9040，9041，9042，9043，9044，9045，9046，9047，9048，9049，9050，9051，9070，9080，9082，9084，9088，9089，9090，9091，9092，9093，9094，9095，9096，9097，9098，9099，9100，9101，9102，9103，9104，9105，9106，9107，9108，9109，9110，9111，9119，9136，9151，9160，9189，9191，9199，9200，9201，9202，9203，9204，9205，9206，9207，9208，9209，9210，9211，9212，9213，9214，9215，9216，9217，9218，9219，9220，9221，9222，9251，9295，9299，9300，9301，9302，9303，9304，9305，9306，9307，9308，9309，9310，9311，9389，9418，9433，9443，9444，9445，9500，9527，9530，9550，9595，9600，9606，9633，9663，9682，9690，9704，9743，9761，9765，9861，9869，9876，9898，9899，9943，9944，9950，9955，9966，9981，9988，9990，9991，9992，9993，9994，9997，9998，9999，10000，10001，10134，10243，10250，10443，10554，11112，11211，11300，12000，12345，13579，14147，14265，14344，16010，16464，16992，16993，17000，18081，18245，20000，20087，20256，20547，21025，21379，22222，23023，23424，25105，25565，27015，27016，27017，27036，28015，28017，30718，32400，32764，33060，33338，37215，37777，41794，44818，47808，48899，49152，49153，50000，50050，50070，50100，51106，51235，52869，53413，54138，54984，55442，55443，55553，55554，60001，60129，62078，64738 |

