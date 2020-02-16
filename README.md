# v2ray-example
通过这个简单的教程
>你将看到如何配置一个常规的v2ray服务器环境
>在Linux下顺滑的使用v2ray而无需GUI客户端

****本教程的测试服务器采用 Debian 10 ，客户端为 Fedora 31，请根据你的系统环境作对应的矫正***

##### 安装服务端并配置
下载适合你系统的v2ray版本
> https://github.com/v2ray/v2ray-core/releases

解压文件 
> unzip v2ray-linux-64.zip (如果没有unzip,需要安装 apt install unzip)

编辑config.json文件
> 直接将仓库中的 server.config.json配置信息覆盖config.json里面的配置作为基础，文件名请保持

config.json不变
> 对新手来说，只需要更改端口

最后运行 ./v2ray，服务就跑起来了
但！为了服务更加优雅一点，以及断开服务器ssh后服务仍然保持运行
> 在/etc/profile 中加入 /usr/bin/nohup /opt/v2ray/v2ray > /dev/null 2>&1 &
> 执行 source /etc/profile
> 这样服务就优雅的跑起来了，并且服务器开机后会自动运行v2ray服务

如果你的服务器在海外，可以选择开启bbr来优化一下你的服务器网速
> 在 /etc/sysctl.conf 中添加
net.core.default_qdisc=fq
net.ipv4.tcp_congestion_control=bbr
>**我惊奇的发现 Debian 10默认就开启了**

至此，服务端环境已经ok了，如果你手机上有v2ray客户端，可以连接使用了

##### 安装客户端环境
和服务器安装完全一样:tw-1f31f:，唯一的区别是将仓库中个的client.config.json替换条到config.json, 唯一需要改的配置项是ip地址
在客户端运行v2ray，还需要在浏览器配合 SwitchyOmega 插件使用
> 下载地址 https://github.com/FelisCatus/SwitchyOmega/releases

观察config.json你会发现，这里即配置了sock还配置了http,sock对应了10808端口，
而http对应了10809，也就是说你可以同时使用两种代理方式
> 在 SwitchyOmega中我们随便配置一种代理即可,比如 sock
> 协议：sock5 地址：127.0.0.1 端口：10808

光是浏览器使用还不够，我们还想要在命令行终端上使用
> 在 /etc/bashrc中添加如下别名定义
> alias fq="export http_proxy='http://127.0.0.1:10809' && export https_proxy='http://127.0.0.1:10809'"
> source /etc/bashrc
> 打开一个新终端，执行 fq,然后下面你就可以wget google.com 了



