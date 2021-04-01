# ZMap之坑

[2016/11/6](https://evilcos.me/?p=557) BY [余弦](https://evilcos.me/?author=1)·30,636 VIEWS | [2 COMMENTS](https://evilcos.me/?p=557#comments)

ZMap确实是个伟大的工具，但离伟大的社区、伟大的产品还有挺长的路要走。ZMap现在稳定版本是：zmap-2.1.0，下一个大版本是3，作者的野心应该是很大的，但是3能否顺利出来，出来能否得到更好口碑，就真不好说了。

# 天坑

对于ZMap来说，最大的使用成本在于：不是谁都能像ZoomEye/Shodan那样的大工程拥有足够的硬件环境（包括机房环境），一个不小心就是被封。这是ZMap最大的坑，我们称之为天坑，这种天坑无形中也形成了足够的竞争门槛。但是这个天坑除了土豪一把也不是没办法填上，比如有的人就动了那么点洪荒之力，如“Internet Census 2012 ”工程，这个洪荒之力就是“Carna Botnet“。细节（不知道有多少人真的研读了）：

>   http://internetcensus2012.bitbucket.org/paper.html



*跑个小题：在职场上，我经常建议一些新人好好珍惜当前职场给你的发展空间，很多人不明白什么是发展空间，甚至很难意识到自己正处在一个得天独厚的发展空间里，比如进入ZoomEye工程，那些硬件环境瞬间可以把你的格局拉高一个档次；再比如进入运营商核心网络，无论是作为核心员工进入还是作为乙方安全服务进入，这种天赐良机不把握好，简直是“暴殄天物”；不用过多举例，你可以举一反三。这些都是你未来拥有的竞争门槛。*

# 几个小坑

ZMap的天坑是不能怪ZMap的，这是没办法的事。然而有一些坑就是ZMap的不对了。

举几个例子：

## 1.

如果你要编译ZMap，还是老实在https://zmap.io/download.html这下载最新的稳定版源码进行编译，如果你跟风去编译最新的源码，除非你很明白作者的3版本意图。

## 2.

如果要使用forge_scoket，官方文档会把你坑成狗，建议如下：

>   git clone https://github.com/ewust/forge_socket.git

里面的README都是5年前的，而forge_socket.c才3个月前做了次更新：support up to 4.4 kernel。clone到zmap-2.1.0根目录下，按说明make，然后insmod forge_socket.ko，完成内核模块加载。

cp forge_socket.h到zmap-2.1.0根目录下的examples/forge-socket/下，make报错（unlock_file未定义引用错误），通过修改Makefile的：

>   forge-socket: forge-socket.o xalloc.o logger.o

为

>   forge-socket: forge-socket.o xalloc.o logger.o lockfd.o

继续make，可以使用了：

>   ./forge-socket

特别提醒下：https://github.com/ewust/forge_socket.git与zmap-2.1.0根目录下的examples/forge-socket/是互补的，这两个forge_socket不是一个玩意，前者是个内核模块，后者仅仅是调用了这个内核模块的小工具而已。

## 3.

上面那点其实已经深深感知到官方文档的“恶意”，再补一刀，官方文档里的：

>   iptables -A OUTPUT -p tcp -m tcp –tcp-flgas RST,RST RST,RST -j DROP

flgas是什么鬼？应该是flags…

## 4.

还有一些小坑本质是上面说的天坑衍生出来的，就不提了，实战出真知。

# 随便说说

如果仔细过一遍ZMap的GitHub众多小项目，是可以看出ZMap的野心的，包括关联大工程scans.io，以及附属品censys.io，不得不说句良心话：censys.io之于zoomeye.org和shodan.io，没有可比性，网络空间搜索引擎，当前全球最好用的也只有zoomeye.org及shodan.io，对我来说这两大搜索引擎是互补着用的。

ZMap这个扫描工程之于伟大的Nmap完成是另一个分支，开辟扫描领域的新天地，类似的项目至少还有masscan。如果把Nmap比作牛顿，那ZMap/masscan这类工具就是爱因斯坦，它做的事并没推翻Nmap的世界，而是构建一套高大上适合高速网络环境下的事:-)

扫描领域，坑是无数的，填坑最好的方式就是实战，没有其他捷径。