## Network > DNS Plus > Overview

DNS Plus is a type of DNS that allows users all around the world to access their networks in a fast and reliable way. It supports Anycast network for fast and reliable access to DNS from anywhere. In addition, DNS services are directly available in the web console without separate DNS solutions or servers.

## Key features

- DNS
    - DNS services are directly available in the web console without separate DNS solutions or servers.
    - It supports Anycast network so that a high speed services can be provided in and out of the country.
    - As designed to accept a massive volume of DNS queries, it can process a huge volume of DNS queries and mitigate DDoS attacks against DNS.

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

## Service targets

- DNS
    - For developers whose applications and services will be provided to global customers
    - For administrators and maintenance engineers who want to ease the burden of management and maintenance of DNS servers

- GSLB
    - 안정적인 서비스 트래픽 로드밸런싱을 원하는 경우
    - 전 세계적인 서비스 트래픽 로드밸런싱을 원하는 경우
    - 로드밸런싱을 통한 DR(Disaster Recovery) 구성을 원하는 경우

## Service terms

| Terms | Description |
|---|---|
| Domain | An address used to identify a computer on the network, which is presented in a human-recognizable format. |
| DNS Zone | A domain area of the host that DNS provides service. As a container of record sets, it specified how to route traffic from domain and subdomain. |
| Record set | Host information that DNS provides service. This is used to route traffic from domain. |
| Record set type | A resource type which shows how to route traffic from host. |
| Record value | A description of how to route the traffic from the host. The entry is determined based on the record set type (resource type) and one or more record values can be registered. |
| GSLB 도메인 | Global Server Load Balancing 도메인의 약어이며, DNS Plus에서 생성한 서비스 도메인(레코드 세트)에 CNAME 매핑하여 사용하실 수 있는 도메인입니다. |
| Pool | 엔드포인트를 그룹핑하는 요소이며, 라우팅 규칙이 적용되는 최소 단위입니다. Pool 내에 포함된 엔드포인트는 가중치에 따라 로드밸런싱 됩니다. |
| 엔드포인트 | 실제 트래픽을 처리하는 서버를 말합니다. 엔드포인트는 IP 및 도메인 형태로 사용 가능합니다. |
| 헬스체크 | 설정한 값에 따라 Pool내에 속한 엔드포인트에 대해 접근성을 확인하는 요소입니다. |
