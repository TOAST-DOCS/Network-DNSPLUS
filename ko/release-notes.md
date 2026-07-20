<!-- pre-align:aligned sig=464bf6032da9 -->

<a id="network-dns-plus-release-notes"></a>
## Network > DNS Plus > 릴리스 노트 { #network-dns-plus-release-notes }

<a id="april-14-2026"></a>
### 2026. 04. 14. { #april-14-2026 }

<a id="april-14-2026-added-features"></a>
#### 기능 추가
*  API v2.0 추가
    * User Access Key 토큰을 지원합니다.

<a id="november-25-2025"></a>
### 2025. 11. 25. { #november-25-2025 }

<a id="november-25-2025-feature-updates"></a>
#### 기능 변경
*  TXT 레코드 세트 타입의 레코드 값 최대 길이를 255바이트에서 4096바이트로 변경했습니다.

<a id="april-29-2025"></a>
### 2025. 04. 29. { #april-29-2025 }

<a id="april-29-2025-feature-updates"></a>
#### 기능 변경
*  레코드 세트 TTL의 최솟값을 1에서 10으로 변경했습니다.

<a id="may-28-2024"></a>
### 2024. 05. 28. { #may-28-2024 }

<a id="may-28-2024-added-features"></a>
#### 기능 추가 
* GSLB 헬스 체크에서 헬스 체크 요청의 헤더, 헬스 체크 주기, 최대 응답 대기 시간, 최대 재시도 횟수 설정 기능이 추가되었습니다.

<a id="march-12-2024"></a>
### 2024. 03. 12. { #march-12-2024 }

<a id="march-12-2024-feature-updates"></a>
#### 기능 개선

* SPF 레코드 세트 타입 지원이 중단되었습니다. TXT 레코드 세트 타입으로 대신 사용할 수 있습니다.
    * 상세 내용은 [[RFC 7208#section-14.1]](https://datatracker.ietf.org/doc/html/rfc7208#section-14.1)에서 확인할 수 있습니다.

<a id="august-24-2021"></a>
### 2021. 08. 24. { #august-24-2021 }

<a id="august-24-2021-added-features"></a>
#### 기능 추가

* 레코드 세트 대량 생성 기능이 추가되었습니다.


<a id="september-22-2020"></a>
### 2020. 09. 22. { #september-22-2020 }

<a id="september-22-2020-feature-updates"></a>
#### 기능 개선

* 레코드 세트 수정 시 레코드 세트 타입 수정할 수 있도록 개선되었습니다.


<a id="december-24-2019"></a>
### 2019. 12. 24. { #december-24-2019 }

<a id="december-24-2019-added-features"></a>
#### 기능 추가

* 엔드포인트 서버의 트래픽을 안정적으로 로드밸런싱할 수 있는 GSLB(Global Server Load Balancing) 기능이 추가되었습니다.
* 생성되는 GSLB 도메인은 라우팅 규칙에 따라 DR(Disaster Recovery), 랜덤 로드밸런싱, 전 세계적인 로드밸런싱으로 구성할 수 있습니다.
* Pool은 라우팅 규칙을 적용할 수 있는 최소 단위로 엔드포인트 서버를 그룹핑하는 요소입니다.
* 주기적으로 Pool에 포함된 엔드포인트 서버에 헬스 체크를 수행하여 안정적인 서비스를 지원할 수 있습니다. 헬스 체크는 HTTP/HTTPS/TCP를 지원합니다.

<a id="december-24-2019-feature-updates"></a>
#### 기능 개선

* 레코드 세트 생성/수정 시 CNAME 레코드 세트 타입을 사용자의 GSLB 도메인을 선택하여 입력할 수 있도록 개선되었습니다.


<a id="august-27-2019"></a>
### 2019. 08. 27. { #august-27-2019 }

<a id="august-27-2019-feature-updates"></a>
#### 기능 개선

* 레코드 세트의 최대 생성 가능 개수를 추가했습니다. DNS Zone당 레코드 세트는 최대 5,000개까지 생성할 수 있습니다.
* 레코드 세트 통계 조회 시 CNAME 레코드 세트 타입은 A 레코드 세트 타입과 AAAA 레코드 세트 타입을 같이 조회하도록 수정했습니다.


<a id="june-25-2019"></a>
### 2019. 06. 25. { #june-25-2019 }

<a id="june-25-2019-release-of-a-new-product"></a>
#### 신규 상품 출시

* DNS Plus는 도메인 관리 기능을 제공하는 서비스입니다.
* DNS 서버를 간편하게 설정할 수 있습니다.
