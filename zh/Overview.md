## Network > DNS Plus > 概要

DNS Plus是全球用户可稳定、快速访问的DNS。支持任播网络，无论在何处均可稳定且快速地访问DNS，无需另外的DNS解决方案或服务器，即可直接在网页控制台中提供DNS服务。

## 主要功能

- DNS
    - 无需另行安装DNS服务器，在网页控制台中即可进行DNS操作。
    - 支持任播网络，不仅韩国国内，在海外也可快速提供服务。
    - 设计为可接收大规模DNS查询，因此还可缓解大规模DNS答疑处理及以DNS为对象的DDoS攻击。

- GSLB
    - DNS Plus GSLB(Global Server Load Balancing)는 DNS 서비스 기반으로 엔드포인트에 안정적으로 트래픽을 로드밸런싱 해주는 상품입니다.
    - 다양한 라우팅 규칙을 제공합니다.

| 라우팅 규칙 | 설명 | 상세 내용 |
|---|---|---|
| FAILOVER | GSLB 도메인에 연결된 Pool의 우선순위로 라우팅합니다. |  |
| RANDOM | GSLB 도메인에 연결된 Pool 중 사용 가능한 Pool을 무작위로 라우팅합니다. |  |
| GEOLOCATION | 설정된 지역의 트래픽을 해당 연결된 Pool로 라우팅 합니다.<br>지역에 대한 설정이 없는 경우 우선순위로 라우팅합니다. | <설정 가능한 지역><br>- Western North America<br>- Eastern Nort America<br>- Western Europe<br>- Eastern Europe<br>- Northern South America<br>- Southern South America<br>- Oceania<br>- Middle East<br>- Northern Africa<br>- Southern Africa<br>- India<br>- Southeast Asia<br>- Northeast Asia |

- Pool을 통한 엔드포인트 그룹핑
    - Pool을 이용하여 특정 목적에 따라 엔드포인트를 그룹핑 할 수 있습니다.
    - Pool 내에 포함된 엔드포인트별로 가중치를 설정할 수 있습니다.

- 다양한 엔드포인트 헬스체크 기능 지원
    - HTTP/HTTPS/TCP 를 이용해 Pool에 포함된 엔드포인트의 헬스체크를 수행함으로써 안정적인 서비스를 지원합니다.
    - HTTP 헬스체크의 경우 포트, 경로, 예상 상태 코드, 예상 응답 본문을 설정할 수 있습니다.
    - HTTPS 헬스체크의 경우 인증서 유효성, 포트, 경로, 예상 상태 코드, 예상 응답 본문을 설정할 수 있습니다.

## 服务对象

- DNS
    - 开发以全球为对象提供服务的应用程序时
    - DNS服务器的管理与维护负担较重时

- GSLB
    - 안정적인 서비스 트래픽 로드밸런싱을 원하는 경우
    - 전 세계적인 서비스 트래픽 로드밸런싱을 원하는 경우
    - 로드밸런싱을 통한 DR(Disaster Recovery) 구성을 원하는 경우

## 服务用语

| 用语 | 说明 |
|---|---|
| 域名 | 将网络中识别的计算机的地址以人易于认知的形式进行表现的方法。|
| DNS Zone | DNS服务的主机的域名区域。是记录集合的容器，包括对域名及下级域名的通信量进行路由的方法。|
| 记录集合 | 作为以DNS Zone服务的主机信息，是对域名的通信量进行路由的方法。|
| 记录集合类型 | 显示对域名的通信量进行路由的源类型。|
| 记录值 | 记述对主机的通信量进行路由的方法，根据记录集合类型(源类型)决定输入内容，除特定类型外，可登记一个以上。|
| GSLB 도메인 | Global Server Load Balancing 도메인의 약어이며, DNS Plus에서 생성한 서비스 도메인(레코드 세트)에 CNAME 매핑하여 사용하실 수 있는 도메인입니다. |
| Pool | 엔드포인트를 그룹핑하는 요소이며, 라우팅 규칙이 적용되는 최소 단위입니다. Pool 내에 포함된 엔드포인트는 가중치에 따라 로드밸런싱 됩니다. |
| 엔드포인트 | 실제 트래픽을 처리하는 서버를 말합니다. 엔드포인트는 IP 및 도메인 형태로 사용 가능합니다. |
| 헬스체크 | 설정한 값에 따라 Pool내에 속한 엔드포인트에 대해 접근성을 확인하는 요소입니다. |
