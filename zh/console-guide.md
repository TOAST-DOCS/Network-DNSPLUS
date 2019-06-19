## Network > DNS Plus > 콘솔 사용 가이드

여기에서는 콘솔에서 DNS Zone과 레코드 세트를 관리하는 방법을 설명합니다.

## DNS Zone 관리

메뉴의 **DNS Plus** 화면에서 DNS Zone을 관리할 수 있습니다.

![image_01_20190604](https://static.toastoven.net/prod_dnsplus/image_01_20190604.png)

### DNS Zone 생성

![image_02_20190604](https://static.toastoven.net/prod_dnsplus/image_02_20190604.png)

1. DNS Zone은 레코드 세트의 컨테이너로 DNS가 서비스하는 호스트에 대한 도메인 영역이며, **DNS Zone 생성** 버튼을 클릭하여 생성합니다.

2. **DNS Zone 이름**과 **설명**을 입력하고 **확인** 버튼을 클릭합니다.  

- **DNS Zone 이름**에 사용자가 소유한 도메인 또는 하위 도메인을 [FQDN(fully qualified domain name)](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력합니다.
- **DNS Zone 이름**은 DNS 서버에서 유일해야 합니다.
- 동일한 **DNS Zone 이름**은 DNS 서버 수만큼 생성 가능합니다. DNS 서버는 3대입니다.
- 생성이 완료되면 기본으로 생성되는 NS 레코드 세트의 네임 서버 정보를 도메인에 설정해야 합니다. 기본으로 생성되는 레코드 세트는 [레코드 세트 관리](./console-guide/#_1)에서 확인할 수 있습니다.
	- 신규로 등록한 도메인으로 생성한 경우 도메인 등록 기관에 네임 서버 정보를 해당 네임 서버로 설정해야 합니다.
	- 운영 중인 도메인의 하위 도메인으로 생성한 경우 운영 중인 도메인에 NS 레코드 세트를 하위 도메인 이름과 해당 네임 서버로 생성해야 합니다.

### DNS Zone 수정

![image_03_20190604](https://static.toastoven.net/prod_dnsplus/image_03_20190604.png)

1. 수정할 DNS Zone을 선택한 후, **DNS Zone 수정** 버튼을 클릭합니다.

2. **설명**을 수정하고, **확인** 버튼을 클릭합니다.

### DNS Zone 삭제

![image_04_20190604](https://static.toastoven.net/prod_dnsplus/image_04_20190604.png)

1. 삭제할 DNS Zone을 모두 선택한 후, **DNS Zone 삭제** 버튼을 클릭합니다.

2. **확인** 버튼을 클릭합니다. DNS Zone 내 모든 레코드 세트도 삭제되며 일정 시간이 소요됩니다.

## 레코드 세트 관리

![image_05_20190604](https://static.toastoven.net/prod_dnsplus/image_05_20190604.png)

메뉴의 **DNS Plus** 화면에서 선택한 DNS Zone의 레코드 세트를 관리할 수 있습니다.

- **DNS Zone 이름** 또는 하위 이름에 대한 레코드 세트를 레코드 세트 타입별로 만들 수 있습니다.
- **DNS Zone 이름**의 SOA와 NS 레코드 세트는 기본으로 생성되며 수정 및 삭제할 수 없습니다.
- SOA 레코드 세트는 생성/수정/삭제할 수 없으며, NS 레코드 세트는 **DNS Zone 이름**으로 생성/수정/삭제할 수 없습니다.


### 레코드 세트 생성

![image_06_20190604](https://static.toastoven.net/prod_dnsplus/image_06_20190604.png)

1. 레코드 세트는 서비스되는 호스트 정보이며, **레코드 세트 생성** 버튼을 클릭하여 생성합니다.

2. 레코드 세트 정보를 입력합니다.

- 레코드 세트 이름: 서비스할 호스트 이름으로 **DNS Zone 이름** 또는 하위 이름이 될 수 있도록 [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력합니다.
- 레코드 세트 타입: 호스트의 유형으로 사용 목적에 따라 유형을 선택합니다.
- TTL(초): Time to live로 데이터의 유효 기간을 나타냅니다. 네임 서버에서 레코드 세트 정보의 갱신 주기를 초 단위로 입력합니다. 우측 버튼을 클릭하여 간단하게 입력할 수 있습니다.

3. 레코드 세트 타입에 따른 레코드 정보를 입력합니다.

- 레코드 세트 타입에 따라 레코드값을 다르게 입력해야 합니다. 생성 화면 하단 설명에 따라 입력합니다.
- 레코드 세트 타입에 따라 여러 개의 레코드를 입력할 수 있습니다. 화면 우측 **+** 버튼을 클릭해 레코드를 추가할 수 있으며, 추가된 레코드의 **-** 버튼을 클릭해 레코드를 제거할 수 있습니다.

4. 설정 완료 후 **확인** 버튼을 클릭합니다.

### 레코드 세트 수정

![image_07_20190604](https://static.toastoven.net/prod_dnsplus/image_07_20190604.png)

1. 수정할 레코드 세트를 선택한 후, **레코드 세트 수정** 버튼을 클릭합니다.

2. **레코드 세트 이름**과 **레코드 세트 타입**은 수정할 수 없으며, **TTL(초)**과 **레코드값**은 수정할 수 있습니다.

3. 설정 완료 후 **확인** 버튼을 클릭합니다.

### 레코드 세트 삭제

![image_08_20190604](https://static.toastoven.net/prod_dnsplus/image_08_20190604.png)

1. 삭제할 레코드 세트를 모두 선택한 후, **레코드 세트 삭제** 버튼을 클릭합니다.

2. **확인** 버튼을 클릭합니다.

### 레코드 세트 통계

![image_09_20190604](https://static.toastoven.net/prod_dnsplus/image_09_20190604.png)

1. 확인할 레코드 세트를 선택한 후, **레코드 세트 통계** 버튼을 클릭합니다.

2. 레코드 세트의 최근 일주일의 쿼리 정보와 평균 응답 시간을 확인할 수 있습니다.


### 조회 및 목록 보기

![image_10_20190604](https://static.toastoven.net/prod_dnsplus/image_10_20190604.png)

1. **타입 필터**를 이용하여 선택한 레코드 세트 타입만 볼 수 있습니다.

2. 검색어를 입력하고 **Enter Key** 또는 **검색**을 클릭하면 레코드 세트 이름으로 검색합니다.  

- 검색어를 포함하는 모든 값을 검색합니다.
