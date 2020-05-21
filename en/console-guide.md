## Network > DNS Plus > Console User Guide

This Console User Guide describes how to manage DNS Zones and record sets in the console.

## Manage DNS Zone

You can manage the DNS Zone on the **DNS** screen of the menu.

![image_01_20191218](https://static.toastoven.net/prod_dnsplus/image_01_20191218.png)

### Create DNS Zone

1. The DNS Zone is a container of the record sets, which means a domain area for the host where the DNS provides service. Click the **Create DNS zone** button to create one.

2. Enter the **DNS Zone name** and the **Description** and then click the **OK** button.  

	- In the **DNS Zone name**, type the user-owned domain or the sub domain as follows: [FQDN(fully qualified domain name)](https://en.wikipedia.org/wiki/Fully_qualified_domain_name).
	- Its **DNS Zone name** must be unique within the DNS server.
	- The same **DNS Zone name** can be created as many as the number of DNS servers. There are three DNS servers.
	- After it has been created, set the name server information of the default NS record set in the domain. For the default record set, see [Manage Record Set](./console-guide/#manage-record-set).
		- If it is created as a newly-registered domain, set the name server information to the corresponding name server in the domain registrar.
		- If it is created as a sub domain of the domain currently in operation, set the NS record set using the sub domain name and the corresponding name server in the domain being operated.

![image_02_20191218](https://static.toastoven.net/prod_dnsplus/image_02_20191218.png)

### Updating DNS Zone

1. Select the DNS Zone to modify and then click the **Update DNS Zone** button.

2. Modify the **Description** and then click the **OK** button.

![image_03_20191218](https://static.toastoven.net/prod_dnsplus/image_03_20191218.png)

### Delete DNS Zone

1. Select all DNS Zones to delete and then click the **Delete DNS Zone** button.

2. Click the **OK** button. All record sets within the DNS Zone will be deleted and it takes some time to finish the job.

![image_04_20191218](https://static.toastoven.net/prod_dnsplus/image_04_20191218.png)


## Manage Record set

Manage record sets of the DNS Zone selected on the **DNS** screen of the menu.

- Create the record sets for the **DNS Zone name** or the sub name by record set type.
- SOA and NS record sets of the **DNS Zone name** are created by default and cannot be modified or deleted.
- SOA record set cannot be created, modified, or deleted. NS record set cannot be created, modified, or deleted using the **DNS Zone name**.

![image_05_20191218](https://static.toastoven.net/prod_dnsplus/image_05_20191218.png)

### Create Record set

1. The record set is the information of the host served. Click the **Create record set** button to create it.

2. Enter the record set information.

	- Record set name: Enter the **DNS Zone name** or the sub name as the host name to serve in the [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) type.
	- Record set type: This is a type of host which can be selected according to the purpose of use.
	- TTL(sec) Time to live (TTL) refers to the valid period of data. Enter the renew cycle of the record set information in seconds in the name server. Right-click for easy entry.

3. Enter record information appropriate for the record set type.

	- A different record value must be entered depending on the record set type. Enter the record value according to the description displayed at the bottom of the screen.
	- The record value must be different by record set type. Click the **+** button on the right side of the screen to add a record. Click the **-** button next to the added record to remove it.

4. After completing the setup, click the **OK** button.

5. 레코드 세트 생성 개수는 제한되어 있으며 연장이 필요하면 별도로 문의해 주시기 바랍니다. 

	- 문의처: [1:1 문의](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

![image_06_20200511](https://static.toastoven.net/prod_dnsplus/image_06_20200511.png)

### Modify Record set

1. Select a record set to modify and then click the **Modify Record Set** button.

2. **Record set name** and **record set type** cannot be modified. However, **TTL(sec)** and **record value** can be modified.

3. After completing the setup, click the **OK** button.

![image_07_20191218](https://static.toastoven.net/prod_dnsplus/image_07_20191218.png)

### Delete Record set

1. Select the record set to delete and then click the **Delete Record Set** button.

2. Click the **OK** button.

![image_08_20191218](https://static.toastoven.net/prod_dnsplus/image_08_20191218.png)

### Record set statistics

1. Select the record set to view and then click the **Record Set Statistics** button.

2. You can view the query information and average response time of the record set for the last seven days.

![image_09_20191218](https://static.toastoven.net/prod_dnsplus/image_09_20191218.png)

### Look up and view list

1. View only the record set types selected with **Type Filter**.

2. Type the keyword and click the **Enter Key** or the **Search** to search for the record set name.  

	- All values which include the keyword are searched.

![image_10_20191218](https://static.toastoven.net/prod_dnsplus/image_10_20191218.png)


## GSLB 및 연결된 Pool 관리

메뉴의 **GSLB** 화면에서 GSLB(global server load balancing)를 관리할 수 있으며, 선택한 GSLB의 Pool 연결을 관리할 수 있습니다.

![image_11_20191218](https://static.toastoven.net/prod_dnsplus/image_11_20191218.png)

### GSLB 생성

**GSLB 도메인**은 **라우팅 규칙**에 따라 안정적으로 트래픽이 로드밸런싱됩니다. GSLB를 생성하는 방법은 다음과 같습니다.

1. **GSLB 생성** 버튼을 클릭합니다.

2. **GSLB 생성** 대화 상자에 정보를 입력합니다.

	- GSLB 이름: 영대소문자와 숫자, '-', '_'로 지정할 수 있습니다.
	- 라우팅 규칙: GSLB 도메인에 대한 로드밸런싱 방법으로 FAILOVER, RANDOM, GEOLOCATION 중에서 선택할 수 있습니다.
		- FAILOVER: 연결된 Pool의 우선순위로 라우팅합니다.
		- RANDOM: 연결된 Pool 중 사용 가능한 Pool을 무작위로 선택하여 라우팅합니다.
		- GEOLOCATION: 설정된 지역의 트래픽을 해당 연결된 Pool로 라우팅합니다. 지역 설정이 없는 경우 우선순위로 라우팅합니다.
	- TTL(초): Time to live로 데이터의 유효 기간을 나타냅니다. GSLB 도메인의 갱신 주기를 초 단위로 입력합니다. 오른쪽 버튼을 클릭하여 간단하게 입력할 수 있습니다.
	- 상태: GSLB 도메인의 활성화 여부를 선택합니다.
	
3. 설정 완료 후 **확인** 버튼을 클릭합니다.

4. GSLB 생성 개수는 제한되어 있으며 연장이 필요하면 별도로 문의해 주시기 바랍니다. 

	- 문의처: [1:1 문의](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

![image_12_20200511](https://static.toastoven.net/prod_dnsplus/image_12_20200511.png)

### GSLB 수정

1. 수정할 GSLB를 선택한 후, **GSLB 수정** 버튼을 클릭합니다.

2. **GSLB 이름**과 **라우팅 규칙**, **TTL(초)**, **상태**를 수정하고, **확인** 버튼을 클릭합니다.

![image_13_20191218](https://static.toastoven.net/prod_dnsplus/image_13_20191218.png)

### GSLB 삭제

삭제할 GSLB를 모두 선택한 후, **GSLB 삭제** 버튼을 클릭하고 **GSLB 삭제** 대화 상자에서 **확인**을 클릭합니다.

![image_14_20191218](https://static.toastoven.net/prod_dnsplus/image_14_20191218.png)

### Pool 연결

GSLB에 연결할 Pool을 설정합니다.

1. **Pool 연결** 버튼을 클릭합니다.

2. **Pool 연결** 대화 상자에서 연결할 Pool을 선택하고 라우팅 규칙에 따라 설정 정보를 입력합니다.

	- 우선순위: 숫자가 작을수록 라우팅 순서가 높으며, 기존에 연결된 Pool과 동일한 우선순위를 입력한 경우 기존 Pool의 라우팅 순서가 낮아집니다. 라우팅 규칙이 FAILOVER 또는 GEOLOCATION일 때 설정합니다.
	- 지역: 설정한 지역의 트래픽을 연결한 Pool로 라우팅하며, 라우팅 규칙이 GEOLOCATION일 때 설정합니다.
	예) GSLB에 아래 표와 같이 Pool이 연결되어 있을 경우
		- FAILOVER일 때 Pool-A로 라우팅됩니다.
		- GEOLOCATION일 때 클라이언트가 Northeast Asia 지역에 있는 경우 Pool-B로 라우팅됩니다.
		- GEOLOCATION일 때 클라이언트가 Western North America 지역에 있는 경우 Pool-C로 라우팅됩니다.
		- GEOLOCATION일 때 클라이언트가 Western Europe 지역에 있는 경우 설정된 지역이 없기 때문에 우선순위에 따라 Pool-A로 라우팅됩니다.

	| Pool 이름 | 우선순위 | 지역 |
	| --- | --- | --- |
	| Pool-A | 1 |  |
	| Pool-B | 2 | Northeast Asia, Southeast Asia |
	| Pool-C | 3 | Western North America |

3. **확인** 버튼을 클릭합니다.

4. Pool 연결 개수는 제한되어 있습니다. 연장이 필요하면 별도로 문의해 주시기 바랍니다.

	- 문의처: [1:1 문의](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

![image_15_20191218](https://static.toastoven.net/prod_dnsplus/image_15_20191218.png)

### Pool 연결 수정

1. 수정할 Pool을 선택한 후, **Pool 연결 수정** 버튼을 클릭합니다.

2. **Pool**은 수정할 수 없으며, 라우팅 규칙에 따라 설정 정보를 수정할 수 있습니다.

3. **확인** 버튼을 클릭합니다.

![image_16_20191218](https://static.toastoven.net/prod_dnsplus/image_16_20191218.png)

### Pool 연결 해제

1. 연결을 해제할 Pool을 모두 선택한 후, **Pool 연결 해제** 버튼을 클릭합니다.

2. **확인** 버튼을 클릭합니다.

![image_17_20191218](https://static.toastoven.net/prod_dnsplus/image_17_20191218.png)

### GSLB 검색 및 목록 보기

이름으로 원하는 GSLB를 검색할 수 있습니다.

1. 오른쪽 위 검색 창에 검색어를 입력하고 **검색**을 클릭하거나 **Enter** 키를 누릅니다. 검색어를 포함한 모든 값이 결과로 표시됩니다.

2. 연결된 Pool 목록에 검색어를 입력하면 현재 목록 내에서 검색됩니다.

	- **Pool 이름**, **우선순위**, **지역** 항목에 검색어를 포함하는 모든 Pool을 검색해 표시합니다.

3. GSLB 상태는 아래 규칙에 따라 표시되며 연결된 Pool은 [Pool](./console-guide/#_8)의 상태를 표시합니다.

| GSLB 상태 | GSLB 활성화/비활성화 | Pool 상태 |
| --- | --- | --- |
| 정상(초록색) | 활성화 | 1개 이상 정상 상태 |
| 오류(붉은색) | 활성화 | 정상 상태가 없고 1개 이상 오류 상태 |
| 알 수 없음(회색) | 활성화 | 모두 비활성화 또는 알 수 없음 상태,<br>연결된 Pool이 없는 상태 |
| 비활성화(회색) | 비활성화 | 무관 |

![image_18_20191218](https://static.toastoven.net/prod_dnsplus/image_18_20191218.png)

## Pool 및 엔드포인트 관리

메뉴의 **GSLB** 화면에서 Pool을 관리할 수 있으며, 선택한 Pool의 엔드포인트를 관리할 수 있습니다.

![image_19_20191218](https://static.toastoven.net/prod_dnsplus/image_19_20191218.png)

### Pool 생성

1. 엔드포인트를 그룹핑하는 요소이며, 라우팅 규칙이 적용되는 최소 단위입니다. **Pool 생성** 버튼을 클릭하여 생성합니다.

2. **Pool** 정보를 입력합니다.

	- Pool 이름: 생성되는 Pool을 지칭하는 이름으로 영대소문자와 숫자, '-', '_'로 입력 가능합니다.
	- 상태: Pool의 활성화 여부를 선택합니다.
	- 헬스 체크: Pool 내의 엔드포인트의 접근성을 확인할 헬스 체크를 선택할 수 있습니다.

3. **엔드포인트** 정보를 입력합니다.

	- 여러 개의 엔드포인트를 입력할 수 있습니다. 화면 우측 **+** 버튼을 클릭해 엔드포인트를 추가할 수 있으며, 추가된 엔드포인트의 **-** 버튼을 클릭해 엔드포인트를 제거할 수 있습니다.
	- 엔드포인트 주소: 도메인 주소 또는 IPv4 주소로 입력할 수 있으며 [예약된 IP 주소](https://en.wikipedia.org/wiki/Reserved_IP_addresses)는 입력할 수 없습니다. Pool 내에서 엔드포인트 주소는 중복될 수 없습니다.
	- 가중치: 0~1.00으로 입력할 수 있으며 Pool 내의 다른 엔드포인트 가중치와 상대적으로 동작합니다. 동일한 가중치는 Pool 내에서 동일한 비중을 가집니다.
	- 상태: 엔드포인트의 활성화 여부를 선택합니다.
	
4. 설정 완료 후 **확인** 버튼을 클릭합니다.

5. Pool의 생성 개수, Pool 내의 엔드포인트 개수, 전체 엔드포인트의 개수는 제한되어 있으며 연장이 필요한 경우 별도로 문의해 주시기 바랍니다. 

   - 문의처: [1:1 문의](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

![image_20_20191218](https://static.toastoven.net/prod_dnsplus/image_20_20191218.png)

### Pool 수정

1. 수정할 Pool을 선택한 후, **Pool 수정** 버튼을 클릭합니다.

2. **Pool 이름**과 **상태**, **헬스 체크**, **엔드포인트**를 수정합니다.

3. 설정 완료 후 **확인** 버튼을 클릭합니다.

![image_21_20191218](https://static.toastoven.net/prod_dnsplus/image_21_20191218.png)

### Pool 삭제

1. 삭제할 Pool을 모두 선택한 후, **Pool 삭제** 버튼을 클릭합니다.

2. **확인** 버튼을 클릭합니다.

![image_22_20191218](https://static.toastoven.net/prod_dnsplus/image_22_20191218.png)

### Pool 검색 및 목록 보기

1. Pool 목록에 검색어를 입력하고 **검색**을 클릭하거나 **Enter** 키를 누릅니다. 검색어를 포함한 모든 값이 결과로 표시됩니다.

2. 엔드포인트 목록에 검색어를 입력하면 현재 목록 내에서 검색합니다.

	- **엔드포인트 주소**, **가중치** 항목에 검색어를 포함하는 모든 엔드포인트를 검색해 보여줍니다.

3. Pool의 상태는 아래 규칙에 따라 표시됩니다.

| Pool 상태 | Pool 활성화/비활성화 | 헬스 체크 설정 | 엔드포인트 상태 |
| --- | --- | --- | --- |
| 정상(초록색) | 활성화 | 연결 | 1개 이상 정상 상태 |
| 오류(붉은색) | 활성화 | 연결 | 정상 상태가 없고 1개 이상 오류 상태 |
| 알 수 없음(회색) | 활성화 | 연결 안함,<br>연결인 경우 엔드포인트 상태에 따름 | 모두 비활성화 또는 알 수 없음 상태 |
| 비활성화(회색) | 비활성화 | 무관 | 무관 |

![image_23_20191218](https://static.toastoven.net/prod_dnsplus/image_23_20191218.png)


## 헬스 체크 관리

메뉴의 **GSLB** 화면에서 헬스 체크를 관리할 수 있습니다.

![image_24_20191218](https://static.toastoven.net/prod_dnsplus/image_24_20191218.png)

### 헬스 체크 생성

1. 헬스 체크를 생성하려면 **헬스 체크 생성** 버튼을 클릭합니다. 설정한 값에 따라 Pool 내 엔드포인트의 접근성을 확인할 수 있습니다.

2. **헬스 체크** 정보를 입력합니다.

	- **헬스 체크 이름**: 생성되는 헬스 체크의 이름으로 영어 대소문자와 숫자, '-', '_'로 입력 가능합니다.
	- **프로토콜**: 헬스 체크를 수행할 때 사용할 프로토콜로 HTTPS/HTTP/TCP를 선택할 수 있습니다. 선택한 프로토콜에 따라 입력할 수 있는 정보가 다릅니다.
		- **HTTPS 입력 가능 항목**: 인증서 검증 안 함, 포트, 경로, 예상 상태 코드, 예상 응답 본문
		- **HTTP 입력 가능 항목**: 포트, 경로, 예상 상태 코드, 예상 응답 본문
		- **TCP 입력 가능 항목**: 포트
	- **인증서 검증 안 함**: **예**를 선택하면 헬스 체크가 수행될 때 엔드포인트의 TLS/SSL 인증서가 유효하지 않아도 무시합니다.
	- **포트**: 헬스 체크를 수행할 때 사용할 포트를 입력합니다.
	- **경로**: 헬스 체크를 수행할 때 사용할 경로를 입력합니다. 시작 문자는 슬래시(/)여야 합니다.
	- **예상 상태 코드**: 헬스 체크의 예상 [HTTP Status Code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) 응답을 입력합니다. 와일드카드로 'x' 문자를 입력할 수 있으며 일치할 경우 엔드포인트를 정상으로 판단합니다. 예) 2xx, 20x, 200
	- **예상 응답 본문**: 헬스 체크의 예상 응답 본문을 입력할 수 있습니다.
	- **예상 상태 코드**와 **예상 응답 본문**을 판단할 때 엔드포인트에서 리디렉션된 페이지는 지원하지 않습니다.
	
3. 설정 완료 후 **확인** 버튼을 클릭합니다.

4. 헬스 체크 생성 개수는 제한되어 있으며 연장이 필요한 경우 별도로 문의해 주시기 바랍니다. 

   	- 문의처: [1:1 문의](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

![image_25_20191218](https://static.toastoven.net/prod_dnsplus/image_25_20191218.png)

### 헬스 체크 수정

1. 수정할 헬스 체크를 선택한 후, **헬스 체크 수정** 버튼을 클릭합니다.

2. **헬스 체크 이름**과 **프로토콜**, **인증서 검증 안함**, **포토**, **경로**, **예상 상태 코드**, **예상 응답 본문**을 수정합니다.

3. 설정 완료 후 **확인** 버튼을 클릭합니다.

![image_26_20191218](https://static.toastoven.net/prod_dnsplus/image_26_20191218.png)

### 헬스 체크 삭제

1. 삭제할 헬스 체크를 모두 선택한 후, **헬스 체크 삭제** 버튼을 클릭합니다.

2. **확인** 버튼을 클릭합니다.

![image_27_20191218](https://static.toastoven.net/prod_dnsplus/image_27_20191218.png)

### 헬스 체크 검색 및 기본 정보 확인

1. 헬스 체크 목록 오른쪽 위의 입력란에 헬스 체크 이름을 입력하고 **검색**을 클릭하거나 **Enter** 키를 누릅니다. 검색어를 포함한 모든 값이 결과로 표시됩니다.

2. 검색된 헬스 체크 목록에서 원하는 항목을 선택하면 헬스 체크에 설정된 **기본 정보**를 확인할 수 있습니다.

![image_28_20191218](https://static.toastoven.net/prod_dnsplus/image_28_20191218.png)
