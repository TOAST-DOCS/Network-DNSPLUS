## Network > DNS Plus > 概要

DNS Plus是全球用户可稳定、快速访问的DNS。支持任播网络，无论在何处均可稳定且快速地访问DNS，无需另外的DNS解决方案或服务器，即可直接在网页控制台中提供DNS服务。

## 主要功能

- DNS
    - 无需另行安装DNS服务器，在网页控制台中即可进行DNS操作。
    - 支持任播网络，不仅韩国国内，在海外也可快速提供服务。
    - 设计为可接收大规模DNS查询，因此还可缓解大规模DNS答疑处理及以DNS为对象的DDoS攻击。

- GSLB
    - DNS Plus GSLB(Global Server Load Balancing)是以DNS服务为基础，稳定地向端点负载均衡流量的商品。
    - 提供各种各样的路由规则。

    | 路由规则 | 说明 | 详细内容 |
    |---|---|---|
    | FAILOVER | 按照连接至GSLB域的Pool的优先顺序路由。 |  |
    | RANDOM | 把连接至GSLB域的Pool中可以使用的Pool进行随机路由。 |  |
    | GEOLOCATION | 把设置的地区流量向相应连接的Pool路由。<br>无地区相关设置时，按照优先顺序路由。  | <通过Pool的端点分组><br>- Western North America<br>- Eastern Nort America<br>- Western Europe<br>- Eastern Europe<br>- Northern South America<br>- Southern South America<br>- Oceania<br>- Middle East<br>- Northern Africa<br>- Southern Africa<br>- India<br>- Southeast Asia<br>- Northeast Asia |

    - 通过Pool的端点分组
        - 可以利用Pool，按照特定的目的对端点进行分组。
        - 可以按包含在Pool内的端点类别来设置加权值。

    - 支持各种端点健康检车功能
        - 通过利用HTTP/HTTPS/TCP对包含在Pool的端点进行健康检查，支援稳定的服务。
        - HTTP健康检查时，可以设置Port、路径、预测状态代码、预测响应正文。
        - HTTPS健康检查时，可以设置证书有效性、Port、路径、预测状态代码、预测响应正文。

## 服务对象

- DNS
    - 开发以全球为对象提供服务的应用程序时
    - DNS服务器的管理与维护负担较重时

- GSLB
    - 需要稳定的服务流量负载均衡时
    - 需要全世界的服务流量负载均衡时
    - 需要通过负载均衡的DR(Disaster Recovery)结构时

## 服务用语

| 用语 | 说明 |
|---|---|
| 域名 | 将网络中识别的计算机的地址以人易于认知的形式进行表现的方法。|
| DNS Zone | DNS服务的主机的域名区域。是记录集合的容器，包括对域名及下级域名的通信量进行路由的方法。|
| 记录集合 | 作为以DNS Zone服务的主机信息，是对域名的通信量进行路由的方法。|
| 记录集合类型 | 显示对域名的通信量进行路由的源类型。|
| 记录值 | 记述对主机的通信量进行路由的方法，根据记录集合类型(源类型)决定输入内容，除特定类型外，可登记一个以上。|
