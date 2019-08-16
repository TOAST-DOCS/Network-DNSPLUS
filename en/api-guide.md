## Network > DNS Plus > API 가이드

DNS Plus 서비스의 API를 설명합니다.


## API 공통 정보

### 사전 준비

- API를 사용하려면 앱키가 필요합니다.
- 앱키는 콘솔 상단 **URL & Appkey** 메뉴에서 확인이 가능합니다.

### 응답 공통 정보

- 모든 API 요청에 '200 OK'로 응답합니다. 자세한 응답 결과는 응답 본문의 헤더를 참고하세요.

[성공 응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "Success"
    }
}
```

[실패 응답 본문]

```
{
    "header": {
        "isSuccessful": false,
        "resultCode": 4010001,
        "resultMessage": "Invalid appKey. "
    }
}
```


## DNS Zone API

### DNS Zone 조회

- DNS Zone 목록을 조회합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| GET | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X GET 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones'
```

[옵션]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| zoneIdList | List | 최대 3,000개 | 선택 |  | DNS Zone ID 목록 |
| zoneStatusList | List | CREATING, <br>DELETING, <br>DELETING_FAIL, <br> USE | 선택 | | DNS Zone 상태 목록 <br>(CREATING: 생성 중, <br>DELETING: 삭제 중, <br>DELETING_FAIL: 삭제 실패, <br>USE: 사용) |
| searchZoneName | String |  | 선택 |  | 검색할 DNS Zone 이름 |
| engineId | String | | 선택 |  | DNS 서버 ID |
| page | int | 최소 1 | 선택 | 1 | 페이지 번호 |
| limit | int | 최소 1, 최대 3,000 | 선택 | 50 | 조회 개수 |
| sortDirection | String | DESC, ASC | 선택 | DESC | 정렬 방향(DESC: 내림차순, ASC: 오름차순) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>ZONE_NAME, <br>ZONE_STATUS, <br>RECORDSET_COUNT | 선택 | CREATED_AT | 정렬 대상 <br>(CREATED_AT: 생성일, <br>UPDATED_AT: 수정일, <br>ZONE_NAME: DNS Zone 이름, <br>ZONE_STATUS: DNS Zone 상태, <br>RECORDSET_COUNT: 레코드 세트 개수) |

#### 응답

[응답 본문]

```
{
    "header": {
        // 생략
    },
    "totalCount": 1,
    "zoneList": [
        {
            "engineId": "e13a1bcf0aa8e07f6a4fae94ed869c39",
            "zoneId": "bff20a9a-24cf-4670-8b34-007622ec010e",
            "zoneName": "test.dnsplus.com.",
            "zoneStatus": "USE",
            "description": "테스트",
            "createdAt": "2019-06-04T12:32:50.000+09:00",
            "updatedAt": "2019-06-04T12:32:50.000+09:00",
            "recordsetCount": 2
        }
    ]
}
```

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| totalCount | long | 전체 DNS Zone 개수 |
| zoneList | List | DNS Zone 목록 |
| zoneList[0].engineId | boolean | DNS 서버 ID |
| zoneList[0].zoneId | String | DNS Zone ID |
| zoneList[0].zoneName | String | DNS Zone 이름 |
| zoneList[0].zoneStatus | String | DNS Zone 상태 |
| zoneList[0].description | String | 설명 |
| zoneList[0].createdAt | DateTime | 생성일 |
| zoneList[0].updatedAt | DateTime | 수정일 |
| zoneList[0].recordsetCount | long | 레코드 세트 개수 |


### DNS Zone 생성

- DNS Zone을 생성합니다.
- **DNS Zone 이름**은 DNS 서버에서 유일해야 합니다.
- 동일한 **DNS Zone 이름**은 DNS 서버 수만큼 생성 가능합니다. DNS 서버는 3대입니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| POST | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X POST 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones' \
-H 'Content-Type: application/json' \
--data '{ "zone": { "zoneName": "test.dnsplus.com.", "description": "test" }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| zone | Object |  | 필수 |  | DNS Zone |
| zone.zoneName | String | 최대 254자 | 필수 |  | 생성할 DNS Zone 이름, <br>도메인을 [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력 |
| zone.description | String | 최대 255자 | 선택 |  | DNS Zone 설명 |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


### DNS Zone 수정

- DNS Zone을 수정합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| PUT | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId} |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {zoneId}는 DNS Zone ID이며 [DNS Zone 조회](./api-guide/#dns-zone)를 통해서 알 수 있습니다.

```
curl -X PUT 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}' \
-H 'Content-Type: application/json' \
--data '{ "zone": { "description": "test" }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| zone | Object |  | 필수 |  | DNS Zone |
| zone.description | String | 최대 255자 | 선택 |  | DNS Zone 설명 |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


### DNS Zone 삭제 (비동기)

- 여러 개의 DNS Zone을 삭제하며, DNS Zone의 레코드 세트도 함께 삭제합니다.
- 실제 데이터 삭제는 비동기로 처리됩니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| DELETE | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/async |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- DNS Zone ID는 [DNS Zone 조회](./api-guide/#dns-zone)를 통해서 알 수 있습니다.

```
curl -X DELETE 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/async?
zoneIdList=bff20a9a-24cf-4670-8b34-007622ec010e,52bc0031-37eb-4b82-b4d7-eaab24188dc4'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| zoneIdList | List | 최소 1개, 최대 3,000개 | 필수 |  | DNS Zone ID 목록 |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


## 레코드 세트 API

### 레코드 세트 조회

- 레코드 세트 목록을 조회합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| GET | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {zoneId}는 DNS Zone ID이며 [DNS Zone 조회](./api-guide/#dns-zone)를 통해서 알 수 있습니다.

```
curl -X GET 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets'
```

[옵션]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordsetIdList | List | 최대 3,000개 | 선택 |  | 레코드 세트 목록 |
| recordsetTypeList | List | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, SPF, NS, SOA | 선택 | | 레코드 세트 타입 목록 |
| searchRecordsetName | String |  | 선택 |  | 검색할 레코드 세트 이름 |
| page | int | 최소 1 | 선택 | 1 | 페이지 번호 |
| limit | int | 최소 1, 최대 3,000 | 선택 | 50 | 조회 개수 |
| sortDirection | String | DESC, ASC | 선택 | DESC | 정렬 방향(DESC: 내림차순, ASC: 오름차순) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>RECORDSET_NAME, <br>RECORDSET_TYPE, <br>RECORDSET_TTL | 선택 | CREATED_AT | 정렬 대상 <br>(CREATED_AT: 생성일, <br>UPDATED_AT: 수정일, <br>RECORDSET_NAME: 레코드 세트 이름, <br>RECORDSET_TYPE: 레코드 세트 타입, <br>RECORDSET_TTL: TTL(초)) |

#### 응답

[응답 본문]

```
{
    "header": {
        // 생략
    },
    "totalCount": 2,
    "recordsetList": [
        {
            "recordsetId": "9e92b547-e2c1-4398-8904-552e0ca465e2",
            "recordsetName": "test.dnsplus.com.",
            "recordsetType": "SOA",
            "recordsetTtl": 1500,
            "recordsetStatus": "USE",
            "createdAt": "2019-06-04T12:32:50.000+09:00",
            "updatedAt": "2019-06-04T12:32:50.000+09:00",
            "recordList": [
                {
                    "recordDisabled": false,
                    "recordContent": "ns1.dnsplus.com. hostmaster.dnsplus.com. 2019060401 10800 3600 604800 1200",
                    // 생략: 레코드 세트 타입에 따라 다름
                }
            ]
        },
        {
            "recordsetId": "edb9512b-6e62-409c-99ee-092d340e0adf",
            "recordsetName": "test.dnsplus.com.",
            "recordsetType": "NS",
            "recordsetTtl": 1500,
            "recordsetStatus": "USE",
            "createdAt": "2019-06-04T12:32:50.000+09:00",
            "updatedAt": "2019-06-04T12:32:50.000+09:00",
            "recordList": [
                {
                    "recordDisabled": false,
                    "recordContent": "ns.toastdns-jin.com.",
                    // 생략: 레코드 세트 타입에 따라 다름
                },
                {
                    "recordDisabled": false,
                    "recordContent": "ns.toastdns-jin.net.",
                    // 생략: 레코드 세트 타입에 따라 다름
                }
            ]
        }
    ]
}
```

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| totalCount | long | 전체 레코드 세트 개수 |
| recordsetList | List | 레코드 세트 목록 |
| recordsetList[0].recordsetId | String | 레코드 세트 ID |
| recordsetList[0].recordsetName | String | 레코드 세트 이름 |
| recordsetList[0].recordsetType | String | 레코드 세트 타입 |
| recordsetList[0].recordsetTtl | int | 네임 서버에서 레코드 세트 정보의 갱신 주기 |
| recordsetList[0].recordsetStatus | String | 레코드 세트 상태 |
| recordsetList[0].createdAt | DateTime | 생성일 |
| recordsetList[0].updatedAt | DateTime | 수정일 |
| recordsetList[0].recordList | List | 레코드 목록 |
| recordsetList[0].recordList[0].recordDisabled | boolean | 레코드 비활성화 여부 |
| recordsetList[0].recordList[0].recordContent | String | 레코드값이며 레코드 세트 타입에 따른 상세 필드를 한 줄로 표시한 내용 |


### 레코드 세트 생성

- 레코드 세트를 생성합니다.
- **레코드 세트 타입**으로 A, AAAA, CAA, CNAME, MX, NAPTR, PTR, TXT, SRV, SPF, NS, SOA를 지원합니다.
- SOA 레코드 세트는 생성/수정/삭제할 수 없으며, NS 레코드 세트는 **DNS Zone 이름**으로 생성/수정/삭제할 수 없습니다.
- 레코드 세트 내의 레코드 목록의 길이는 최대 512바이트입니다.
- DNS Zone당 레코드 세트는 최대 5,000개까지 생성할 수 있습니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| POST | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {zoneId}는 DNS Zone ID이며 [DNS Zone 조회](./api-guide/#dns-zone)를 통해서 알 수 있습니다.
- 레코드값은 필수이며 입력 방법으로 recordset.recordList[0].recordContent 필드 또는 상세 필드를 선택할 수 있습니다.
- recordContent 필드는 공백을 구분 문자로 하여 상세 필드를 한 줄로 표시한 내용입니다. 상세 필드는 [레코드 세트 타입에 따른 상세 필드]에서 확인할 수 있습니다.
- 상세 필드와 recordContent 필드를 동시에 입력하면 recordContent 필드를 기준으로 생성됩니다.

```
curl -X POST 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets' \
-H 'Content-Type: application/json' \
--data '{ "recordset": { "recordsetName": "sub.test.dnsplus.com.", "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset | Object |  | 필수 |  | 레코드 세트 |
| recordset.recordsetName | String | 최대 254자<br>(DNS Zone 이름 포함) | 필수 |  | 생성할 레코드 세트 이름, <br>도메인을 [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력 |
| recordset.recordsetType | String | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, SPF, NS | 필수 |  | 레코드 세트 타입 |
| recordset.recordsetTtl | int | 최소 1, 최대 2147483647 | 필수 |  | 네임 서버에서 레코드 세트 정보의 갱신 주기 |
| recordset.recordList | List |  | 필수 |  | 레코드 목록 |
| recordset.recordList[0].recordDisabled | boolean |  | 선택 | false | 레코드 비활성화 여부 |
| recordset.recordList[0].recordContent | String |  | 필수 |  | 레코드 세트 타입에 따른 상세 필드를 한 줄로 표시한 내용 |

[레코드 세트 타입에 따른 상세 필드]

- A 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - 하나의 도메인명에 여러 개의 IPv4 주소를 등록할 수 있습니다.

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].ipV4 | String |  | 필수 |  | IPv4 형식의 주소 |


- AAAA 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - 하나의 도메인명에 여러 개의 IPv6 주소를 등록할 수 있습니다

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].ipV6 | String |  | 필수 |  | IPv6 형식의 주소 |


- CAA 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - 도메인에 발급이 허용된 인증 기관(CA)을 지정하면 허용되지 않은 인증 기관(CA)이 인증서를 발급하는 것을 방지할 수 있습니다.
    - issue 태그는 도메인 또는 하위 도메인에 대한 인증서 발행 권한입니다.
    - issuewild 태그는 도메인 또는 하위 도메인에 대한 와일드카드 인증서 발행 권한입니다.
        - issue 태그와 issuewild 태그의 설정 방법은 동일합니다.
        - 인증서 발행 허용: 인증 기관 주소 입력, 부가 설정이 필요한 경우 세미콜론(;)으로 분리하고 '이름=값' 쌍으로 지정
        - 인증서 발행 금지: 세미콜론(;) 입력
    - iodef 태그는 인증 기관(CA)이 잘 못 된 요청을 받을 경우 설정된 이메일 또는 URL 주소로 알립니다.
        - 메일 입력 형식: "mailto:*email-address*"
        - URL 주소 입력 형식: "http://*URL*" 또는 "https://*URL*"
    - 사용자 지정 태그는 인증 기관(CA)에서 RFC 표준 외의 부가 기능을 지원하는 경우의 설정입니다.

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].flags | int | 0 또는 128 | 필수 |  | 정의된 태그인 경우 0, <br>사용자 지정 태그인 경우 128 |
| recordset.recordList[0].tag | String | TAG_ISSUE, <br>TAG_ISSUEWILD, <br>TAG_IODEF, <br>사용자 지정 태그 최대 15 | 필수 |  | TAG_ISSUE: issue 태그, <br>TAG_ISSUEWILD: issuewild 태그, <br>TAG_IODEF: iodef 태그, <br>사용자 지정 태그 |
| recordset.recordList[0].stringValue | String | 최대 512자(인용부호 포함) | 필수 |  | 태그에 따른 내용 |


- CNAME 레코드 세트
    - 하나의 레코드를 입력할 수 있습니다.
    - 레코드 세트 이름을 정규 이름의 별칭(canonical)으로 정의합니다.
    - A 레코드를 먼저 설정하고 CNAME을 설정해야 합니다.

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | 최대 255자 | 필수 |  | 도메인을 [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력 |


- MX 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - 도메인에 대한 메일 서버를 지정합니다.

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].priority | int | 최소 0, 최대 65535 | 필수 |  | 우선순위 |
| recordset.recordList[0].domainName | String | 최대 255자 | 필수 |  | 도메인을 [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력 |


- NAPTR 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - DDDS(Dynamic Delegation Discovery System) 애플리케이션에서 하나의 값을 다른 값으로 변환하거나 대체하기 위해 사용합니다.
    - 순서 항목은 DDDS 애플리케이션이 레코드를 평가하는 순서입니다.
    - 선호 순서 항목은 두 개 이상의 레코드가 순서 항목이 동일한 경우 우선하여 평가하는 순서입니다.
    - 구분 항목은 DDDS 애플리케이션 설정으로 공백, 'S', 'A', 'U', 'P'를 사용할 수 있으며 그 외의 문자는 예약되어 있습니다.
    - 서비스 항목은 DDDS 애플리케이션 설정으로 상세 정의는 RFC 문서에서 확인할 수 있습니다.
        - URI DDDS 애플리케이션 [RFC 3404#section-4.4](https://tools.ietf.org/html/rfc3404#section-4.4)
        - S-NAPTR DDDS 애플리케이션 [RFC 3958#section-6.5](https://tools.ietf.org/html/rfc3958#section-6.5)
        - U-NAPTR DDDS 애플리케이션 [RFC 4848#section-4.5](https://tools.ietf.org/html/rfc4848#section-4.5)
    - 정규식 항목은 DDDS 애플리케이션에서 입력값을 출력값으로 변환하는 데 사용합니다. 상세 정의는 [RFC 3402#section-3.2](https://tools.ietf.org/html/rfc3402#section-3.2)에서 확인할 수 있습니다.
    - 대체값 항목은 DDDS 애플리케이션이 DNS 쿼리를 제출할 도메인 이름으로 입력값을 대체합니다. 정규식 항목을 설정하는 경우 '.'로 설정합니다.

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].order | int | 최소 0, 최대 65535 | 필수 |  | 순서 |
| recordset.recordList[0].preference | int | 최소 0, 최대 65535 | 필수 |  | 선호 순서 |
| recordset.recordList[0].flags | String | 최대 3자(인용부호 포함) | 필수 |  | 구분 |
| recordset.recordList[0].service | String | 최대 257자(인용부호 포함) | 필수 |  | 서비스 |
| recordset.recordList[0].regexp | String | 최대 257자(인용부호 포함) | 필수 |  | 정규식 |
| recordset.recordList[0].replacement | String | 최대 255자 | 필수 |  | 대체값으로 '.' 또는 도메인을 [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력 |


- PTR 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - IP 주소를 이용해서 도메인 정보를 조회하는 역방향 질의 기능입니다. ISP 업체에 요청하여 설정해야 합니다.
    - IP 주소는 역순으로 레코드 세트 이름에 입력해야 합니다. (예제) 127.0.0.1, 1.0.0.127.in-addr.arpa

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | 최대 255자 | 필수 |  | 도메인을 [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력 |


- TXT 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - 레코드 세트 이름에 대한 텍스트 내용을 입력합니다.

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].stringValue | String | 최대 255바이트(인용부포 포함) | 필수 |  | 텍스트 내용 |


- SRV 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - 유사한 TCP/IP 기반 서비스를 제공하는 여러 서버를 단일 DNS 쿼리 동작을 사용하여 찾을 수 있습니다.

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].priority | int | 최소 0, 최대 65535 | 필수 |  | 우선순위 |
| recordset.recordList[0].weight | int | 최소 0, 최대 65535 | 필수 |  | 가중치 |
| recordset.recordList[0].port | int | 최소 0, 최대 65535 | 필수 |  | 포트 |
| recordset.recordList[0].domainName | String | 최대 255자 | 필수 |  | 도메인을 [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력 |


- SPF 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - 이메일 발신자 도메인 인증 방식으로 수신 메일 서버가 발송 메일 서버와 메일 주소가 일치하는지 확인하는 기능입니다.
    - 아래와 같은 형태로 입력 가능하며 상세 정의는 [RFC4408](https://tools.ietf.org/html/rfc4408)에서 확인할 수 있습니다.
    - 수식자의 기본값은 '+'이며 메커니즘에 따라 IP, 도메인 이름 등을 추가로 입력합니다.
        - 형태: "v=spf1 {수식자}{메커니즘}{내용} {변경자}={내용}"
        - 수식자: '+'(Pass), '-'(Fail), '~'(Soft Fail), '?'(Neutral)
        - 메커니즘: all, include, a, mx, prt, ip4, ip6, exists
        - 변경자: redirect, exp, 사용자 지정
        - (예제)
            - "v=spf1 mx -all"
            - "v=spf1 ip4:192.168.0.1/16 -all"
            - "v=spf1 a:toast.com -all"
            - "v=spf1 redirect=toast.com"

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].stringValue | String | 최대 255바이트(인용부포 포함) | 필수 |  | SPF 형식에 따른 내용 |


- NS 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - 레코드 세트 이름에 대한 네임 서버를 지정합니다.
    - 레코드 세트 이름은 DNS Zone 이름의 하위 도메인으로만 생성이나 수정할 수 있습니다.

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | 최대 255자 | 필수 |  | 도메인을 [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력 |


#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


### 레코드 세트 수정

- 레코드 세트를 수정합니다.
- **레코드 세트 이름**과 **레코드 세트 타입**은 수정할 수 없으며, **TTL(초)**과 **레코드값**은 수정할 수 있습니다.
- SOA 레코드 세트는 생성/수정/삭제할 수 없으며, NS 레코드 세트는 **DNS Zone 이름**으로 생성/수정/삭제할 수 없습니다.
- 레코드 세트 내의 레코드 목록의 길이는 최대 512바이트입니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| PUT | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/{recordsetId} |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {zoneId}는 DNS Zone ID이며 [DNS Zone 조회](./api-guide/#dns-zone)를 통해서 알 수 있습니다.
- {recordsetId}는 레코드 세트 ID이며 [레코드 세트 조회](./api-guide/#_11)를 통해서 알 수 있습니다.
- 레코드값은 필수이며 입력 방법으로 recordset.recordList[0].recordContent 필드 또는 상세 필드를 선택할 수 있습니다.
- recordContent 필드는 공백을 구분 문자로 하여 상세 필드를 한 줄로 표시한 내용입니다. 상세 필드는 [레코드 세트 생성](./api-guide/#_14)에 [레코드 세트 타입에 따른 상세 필드]에서 확인할 수 있습니다.
- 상세 필드와 recordContent 필드를 동시에 입력하면 recordContent 필드를 기준으로 수정됩니다.

```
curl -X POST 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/{recordsetId}' \
-H 'Content-Type: application/json' \
--data '{ "recordset": { "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset | Object |  | 필수 |  | 레코드 세트 |
| recordset.recordsetType | String | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, SPF, NS | 필수 |  | 레코드 세트 ID에 대한 레코드 세트 타입 |
| recordset.recordsetTtl | int | 최소 1, 최대 2147483647 | 필수 |  | 네임 서버에서 레코드 세트 정보의 갱신 주기 |
| recordset.recordList | List |  | 필수 |  | 레코드 목록 |
| recordset.recordList[0].recordDisabled | boolean |  | 필수 |  | 레코드 비활성화 여부 |
| recordset.recordList[0].recordContent | String |  | 필수 |  | 레코드 세트 타입에 따른 상세 필드를 한 줄로 표시한 내용 |


#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


### 레코드 세트 삭제

- 여러 개의 레코드 세트를 삭제하며, 레코드 세트의 레코드도 함께 삭제합니다.
- SOA 레코드 세트는 생성/수정/삭제할 수 없으며, NS 레코드 세트는 **DNS Zone 이름**으로 생성/수정/삭제할 수 없습니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| DELETE | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {zoneId}는 DNS Zone ID이며 [DNS Zone 조회](./api-guide/#dns-zone)를 통해서 알 수 있습니다.
- 레코드 세트 ID는 [레코드 세트 조회](./api-guide/#_11)를 통해서 알 수 있습니다.

```
curl -X DELETE 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets?
recordsetIdList=edb9512b-6e62-409c-99ee-092d340e0adf,edb9512b-6e62-409c-99ee-092d340e0adf'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordsetIdList | List | 최소 1개, 최대 3,000개 | 필수 |  | 레코드 세트 ID 목록 |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```
