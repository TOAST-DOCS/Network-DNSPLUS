## Network > DNS Plus > API 가이드

DNS Plus 서비스의 API를 설명합니다.


## API 공통 정보

### 사전 준비

- API 사용을 위해서는 앱 키가 필요합니다.
- 앱 키는 콘솔 상단 'URL & Appkey' 메뉴에서 확인이 가능합니다.

### 응답 공통 정보

- 모든 API 요청에 '200 OK'로 응답합니다. 자세한 응답 결과는 응답 본문의 헤더를 참고합니다.

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

- {appKey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X GET 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones'
```

[옵션]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| zoneIdList | List | 최대 3,000 | 선택 |  | DNS Zone ID 목록 |
| zoneStatusList | List |  | 선택 | | DNS Zone 상태 목록 <br>(CREATING: 생성중, <br>DELETING: 삭제중, <br>DELETING_FAIL: <br>삭제실패, <br>USE: 사용) |
| searchZoneName | String |  | 선택 |  | 검색할 DNS Zone 이름 |
| engineId | String | | 선택 |  | DNS 서버 ID |
| page | int | 최소 1 | 선택 | 1 | 페이지 번호 |
| limit | int | 최소 1, 최대 3,000 | 선택 | 50 | 조회 개수 |
| sortKey | String |  | 선택 | CREATED_AT | 정렬 대상 <br>(CREATED_AT: 생성일, <br>UPDATED_AT: 수정일, <br>ZONE_NAME: DNS Zone 이름, <br>ZONE_STATUS: DNS Zone 상태, <br>RECORDSET_COUNT: 레코드셋 개수) |
| sortDirection | String |  | 선택 | DESC | 정렬 방향 (DESC: 내림차순, ASC: 올름차순) |

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
| zoneList[0].description | DateTime | 설명 |
| zoneList[0].createdAt | DateTime | 생성일 |
| zoneList[0].updatedAt | DateTime | 수정일 |
| zoneList[0].recordsetCount | long | 레코드셋 개수 |


### DNS Zone 생성

- DNS Zone를 생성합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| POST | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X POST 'https://alpha-api-dnsplus.cloud.toast.com:10443/dnsplus/v1.0/appkeys/9SpMLmHn1O91RM8b/zones' \
-H 'Content-Type: application/json' \
--data '{ "zone": { "zoneName": "test.dnsplus.com.", "description": "test" }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| zoneName | String | 최대 254자 | 필수 |  | 생성할 DNS Zone 이름 |
| description | String | 최대 255자 | 선택 |  | DNS Zone 설명 |

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

- DNS Zone를 수정합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| PUT | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId} |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {zoneId}는 DNS Zone ID이며 [DNS Zone 조회](./api-guide/#_5)를 통해서 알 수 있습니다.

```
curl -X PUT 'https://alpha-api-dnsplus.cloud.toast.com:10443/dnsplus/v1.0/appkeys/9SpMLmHn1O91RM8b/zones' \
-H 'Content-Type: application/json' \
--data '{ "zone": { "description": "test" }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| description | String | 최대 255자 | 선택 |  | DNS Zone 설명 |

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

- 여러 개의 DNS Zone을 삭제하며, DNS Zone의 레코드셋도 함께 삭제합니다.
- 실제 데이터 삭제는 비동기로 처리됩니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| DELETE | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- DNS Zone ID는 [DNS Zone 조회](./api-guide/#_5)를 통해서 알 수 있습니다.

```
curl -X DELETE 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/async?
zoneIdList=bff20a9a-24cf-4670-8b34-007622ec010e,52bc0031-37eb-4b82-b4d7-eaab24188dc4'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| zoneIdList | List | 최소 1, 최대 3,000 | 필수 |  | DNS Zone ID 목록 |

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

## 레코드셋 API
