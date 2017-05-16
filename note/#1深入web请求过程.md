# 1 深入web请求过程

- B/S框架带来好处：
  - 客户端使用统一的浏览器（Browser）。不需要特殊配置，交互性强，用户行为可继承性强。
  - 服务端（Server）基于统一的HTTP。简化开发模式，节省开发成本。基于HTTP的服务器有很多，如Apache、IIS、Nginx、Tomcat、JBoss等。

## 1.1 B/S网络架构概述

  ![webtu1-1](/assets/webtu1-1.jpg)

- 一些原则：
  - 互联网上所有资源都要用一个URL（统一资源定位符）来表示。
  - 必须基于HTTP与服务端交互。
  - 数据展示必须在浏览器中进行。

## 1.2 如何发起一个请求

1. 根据地址栏里输入的URL的域名DNS解析出IP地址。
2. 根据这个IP地址和默认的80端口与远程服务器建立Socket连接。
3. 根据这个URL组装一个get类型的HTTP请求头。
4. 发送到目标服务器。
5. 服务器返回数据。
6. 断开这个连接。

## 1.3 HTTP解析

- HTTP Header控制着互联网上成千上万的用户的数据的传输，控制着用户的浏览器的渲染行为和服务器的执行逻辑。

  ![webb1-1](/assets/webb1-1.jpg)

  ![webb1-2](/assets/webb1-2.jpg)

  ![webb1-3](/assets/webb1-3.jpg)

### 1.3.2 浏览器缓存机制

- Crtl+F5组合：浏览器会直接向目标URL发送请求，而不会使用浏览器缓存的数据；在HTTP的请求头中会增加一些请求头，它告诉 服务端我们要获取最新的数据而不是缓存。

  ![webb1-4](/assets/webb1-4.jpg)

## 1.4 DNS域名解析

### 1.4.1 DNS域名解析过程

![webtu1-10](/assets/webtu1-10.jpg)

1. 浏览器会检查缓存中有没有这个域名对应的解析过的IP地址，如果有，这个解析过程就将结束。
2. 如果没有，浏览器会查找操作系统缓存中是否有这个域名对应的DNS解析结果。（c:\Windows\System32\drivers\etc\hosts，浏览器会 **首先** 使用这个IP地址。）
3. 操作系统把域名发送给LDNS，也就是本地区的域名服务器。LDNS主要承担了域名的解析工作。
4. 如果LDNS仍然没有命中，就直接到Root Server域名服务器请求解析。
5. 根域名服务器返回给本地域名服务器一个所查询与的主域名服务器（gTLD Server）地址。gTLD是国际顶级域名服务器，如.com、.cn、.org等。
6. 本地域名服务器（Local DNS Server）再向上一步返回的gTLD服务器发送请求。
7. 接受请求的gTLD服务器查找并返回此域名对应的Name Server域名服务器的地址。（这个域名解析任务由这个域名提供商的服务器来完成）。
8. Name Server域名服务器会查询存储的域名和IP的映射关系表，连同TTL值返回给DNS Server 域名服务器。
9. Local DNS Server会缓存这个域名和IP的对应关系，缓存的时间有TTL值控制。
10. 解析的结果返回给用户，用户根据TTL值缓存在本地系统缓存中。

### 1.4.4 几种域名解析方式

- A记录，A代表的是Address，用来指定域名对应的IP地址，如将item.taobao.com指定到115.238.23.xxx。A记录可以将多个域名解析到一个IP地址，但 **不能** 将一个域名解析到多个IP地址。
- MX记录， 表示的是Mail Exchange。就是将某个域名下的邮件服务器指向自己的Mail Server。
- CNAME记录，全程Canonical Name（别买解析）。可以为一个域名设置一个或多个别名。如将taobao.com解析到xulingbo.net，xulingbo.net是taobao.com的别名。
- NS记录，为某个域名指定DNS解析服务器，也就是这个域名有指定的IP地址的DNS服务器去解析。
- TXT记录，为某个主机名或域名设置说明。如可以为xulingbo.net设置TXT记录为“君山的博客|许令波”。

## 1.5 CDN工作机制

- CDN就是内容分布网络（Content Delivery Network），是构建在现有Internet上的一种先进的流量分配网络。目前的CDN都以缓存网站中的静态数据为主，如CSS、JS、图片和静态页面等数据。
  - 可扩展（Scalability）。
  - 安全性（Security）。
  - 可靠性、响应和执行（Reliability、Responsiveness、Performance）。

### 1.5.1 CDN架构

![webtu1-16](/assets/webtu1-16.jpg)

### 1.5.2 负载均衡

- 负载均衡有三种架构：链路负载均衡、集群负载均衡和操作系统负载均衡。
  - 链路负载均衡就是前面提到的通过DNS解析成不同的IP，然后用户根据这个IP来访问不同的目标服务器。
  - 集群负载均衡分为硬件负载均衡和软件负载均衡。硬件负载均衡用一台专门的硬件设备来转发请求。
  - 操作系统负载均衡利用操作系统级别的软中断或者硬件中断来达到负载均衡，如可以设置多队列网卡等来实现。

![webtu1-17](/assets/webtu1-17.jpg)

![webtu1-18](/assets/webtu1-18.jpg)

![webtu1-19](/assets/webtu1-19.jpg)

### 1.5.3 CDN动态加速

- 技术原理：在CDN的DNS解析中通过动态的链路探测来寻找回源最好的一条路径，然后通过DNS 的调度将所有请求调度到选定的这条路径上回源，从而加速用户访问的效率。


  ![webtu1-20](/assets/webtu1-20.jpg)
