## Network > DNS Plus > API指南

介绍DNS Plus服务的API。


## API通用信息

### 提前准备

- 若欲使用API，需要Appkey。
- Appkey可在控制台上端**URL & Appkey**菜单中确认。

### 响应通用信息

- 对于所有API请求，以“200 OK”响应。详细响应结果请参考响应正文的标头。

[成功响应正文]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

[失败响应正文]

```
{
    "header": {
        "isSuccessful": false,
        "resultCode": 4010001,
        "resultMessage": "Invalid appKey."
    }
}
```


## DNS Zone API

### 查询DNS Zone

- 查询DNS Zone列表。

#### 请求

[URI]

| 方法 | URI |
|---|---|
| GET | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones |

[请求正文]

- {appkey}更改为在控制台中确认的值。

```
curl -X GET 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones'
```

[选项]

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| zoneIdList | List | 最大3,000个 | 选择 |  | DNS Zone ID列表 |
| zoneStatusList | List | CREATING, <br>DELETING, <br>DELETING_FAIL, <br> USE | 选择 | | DNS Zone状态列表 <br>(CREATING：正在创建，<br>DELETING：正在删除，<br>DELETING_FAIL：删除失败，<br>USE：使用) |
| searchZoneName | String |  | 选择 |  | 要搜索的DNS Zone名 |
| engineId | String | | 选择 |  | DNS服务器ID |
| page | int | 最小1 | 选择 | 1 | 页面编号 |
| limit | int | 最小1，最大3,000 | 选择 | 50 | 查询个数 |
| sortDirection | String | DESC, ASC | 选择 | DESC | 排列方向(DESC：降序，ASC：升序) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>ZONE_NAME, <br>ZONE_STATUS, <br>RECORDSET_COUNT | 选择 | CREATED_AT | 排列对象 <br>(CREATED_AT：创建日，<br>UPDATED_AT：修改日，<br>ZONE_NAME：DNS Zone名，<br>ZONE_STATUS：DNS Zone状态，<br>RECORDSET_COUNT：记录集合个数) |

#### 响应

[响应正文]

```
{
    "header": {
        // 省略
    },
    "totalCount": 1,
    "zoneList": [
        {
            "engineId": "e13a1bcf0aa8e07f6a4fae94ed869c39",
            "zoneId": "bff20a9a-24cf-4670-8b34-007622ec010e",
            "zoneName": "test.dnsplus.com.",
            "zoneStatus": "USE",
            “description”: “文本",
            "createdAt": "2019-06-04T12:32:50.000+09:00",
            "updatedAt": "2019-06-04T12:32:50.000+09:00",
            "recordsetCount": 2
        }
    ]
}
```

[字段]

| 名称 | 类型 | 说明 |
|---|---|---|
| totalCount | long | 全部DNS Zone个数 |
| zoneList | List | DNS Zone列表 |
| zoneList[0].engineId | boolean | DNS服务器ID |
| zoneList[0].zoneId | String | DNS Zone ID |
| zoneList[0].zoneName | String | DNS Zone名 |
| zoneList[0].zoneStatus | String | DNS Zone状态 |
| zoneList[0].description | String | 说明 |
| zoneList[0].createdAt | DateTime | 创建日 |
| zoneList[0].updatedAt | DateTime | 修改日 |
| zoneList[0].recordsetCount | long | 记录集合个数 |


### 创建DNS Zone

- 创建DNS Zone。
- **DNS Zone名**在DNS服务器中应唯一。
- 可按照DNS服务器数创建相同的**DNS Zone名**。DNS服务器为3台。

#### 请求

[URI]

| 方法 | URI |
|---|---|
| POST | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones |

[请求正文]

- {appkey}更改为在控制台中确认的值。

```
curl -X POST 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones' \
-H 'Content-Type:application/json' \
--data '{ "zone": { "zoneName": "test.dnsplus.com.", "description": "test" }}'
```

[字段]

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| zone | Object |  | 必需 |  | DNS Zone |
| zone.zoneName | String | 最大254个字符 | 必需 |  | 要创建的DNS Zone名，<br>域名以[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)输入 |
| zone.description | String | 最大255个字符 | 选择 |  | DNS Zone说明 |

#### 响应

[响应正文]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "zone": {
        "engineId": "e13a1bcf0aa8e07f6a4fae94ed869c39",
        "zoneId": "bff20a9a-24cf-4670-8b34-007622ec010e",
        "zoneName": "test.dnsplus.com.",
        "zoneStatus": "USE",
        "description": "test",
        "createdAt": "2019-06-04T12:32:50.000+09:00",
        "updatedAt": "2019-06-04T12:32:50.000+09:00",
        "recordsetCount": 2
    }
}
```


### 修改DNS Zone

- 修改DNS Zone。

#### 请求

[URI]

| 方法 | URI |
|---|---|
| PUT | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId} |

[请求正文]

- {appkey}更改为在控制台中确认的值。
- {zoneId}为DNS Zone ID，可通过[查询DNS Zone](./api-guide/#dns-zone)确认。

```
curl -X PUT 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}' \
-H 'Content-Type:application/json' \
--data '{ "zone": { "description": "test" }}'
```

[字段]

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| zone | Object |  | 必需 |  | DNS Zone |
| zone.description | String | 最大255个字符 | 选择 |  | DNS Zone说明 |

#### 响应

[响应正文]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "zone": {
        "engineId": "e13a1bcf0aa8e07f6a4fae94ed869c39",
        "zoneId": "bff20a9a-24cf-4670-8b34-007622ec010e",
        "zoneName": "test.dnsplus.com.",
        "zoneStatus": "USE",
        "description": "test",
        "createdAt": "2019-06-04T12:32:50.000+09:00",
        "updatedAt": "2019-06-04T12:42:00.000+09:00",
        "recordsetCount": 2
    }
}
```


### 删除DNS Zone (非同步)

- 删除多个DNS Zone，DNS Zone的记录集合也同时删除。
- 实际数据删除非同步处理。

#### 请求

[URI]

| 方法 | URI |
|---|---|
| DELETE | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/async |

[请求正文]

- {appkey}更改为在控制台确认的值。
- DNS Zone ID可通过[查询DNS Zone](./api-guide/#dns-zone)确认。

```
curl -X DELETE 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/async?
zoneIdList=bff20a9a-24cf-4670-8b34-007622ec010e,52bc0031-37eb-4b82-b4d7-eaab24188dc4'
```

[字段]

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| zoneIdList | List | 最小1个，最大3,000个 | 必需 |  | DNS Zone ID列表 |

#### 响应

[响应正文]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


## 记录集合API

### 查询记录集合

- 查询记录集合列表。

#### 请求

[URI]

| 方法 | URI |
|---|---|
| GET | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[请求正文]

- {appkey}更改为在控制台中确认的值。
- {zoneId}为DNS Zone ID，可通过[查询DNS Zone](./api-guide/#dns-zone)确认。

```
curl -X GET 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets'
```

[选项]

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| recordsetIdList | List | 最大3,000个 | 选择 |  | 记录集合列表 |
| recordsetTypeList | List | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, SPF, NS, SOA | 选择 | | 记录集合类型列表 |
| searchRecordsetName | String |  | 选择 |  | 要搜索的记录集合名 |
| page | int | 最小1 | 选择 | 1 | 页面编号 |
| limit | int | 最小1，最大3,000 | 选择 | 50 | 查询个数 |
| sortDirection | String | DESC, ASC | 选择 | DESC | 排列方向(DESC：降序，ASC：升序) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>RECORDSET_NAME, <br>RECORDSET_TYPE, <br>RECORDSET_TTL | 选择 | CREATED_AT | 排列对象<br>(CREATED_AT：创建日，<br>UPDATED_AT：修改日，<br>RECORDSET_NAME：记录集合名，<br>RECORDSET_TYPE：记录集合类型，<br>RECORDSET_TTL：TTL(秒)) |

#### 响应

[响应正文]

```
{
    "header": {
        // 省略
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
                    "recordContent": "ns1.dnsplus.com.hostmaster.dnsplus.com.2019060401 10800 3600 604800 1200",
                    // 省略：根据记录集合类型有所不同
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
                    // 省略：根据记录集合类型有所不同
                },
                {
                    "recordDisabled": false,
                    "recordContent": "ns.toastdns-jin.net.",
                    // 省略：根据记录集合类型有所不同
                }
            ]
        }
    ]
}
```

[字段]

| 名称 | 类型 | 说明 |
|---|---|---|
| totalCount | long | 全部记录集合个数 |
| recordsetList | List | 记录集合列表 |
| recordsetList[0].recordsetId | String | 记录集合ID |
| recordsetList[0].recordsetName | String | 记录集合名 |
| recordsetList[0].recordsetType | String | 记录集合类型 |
| recordsetList[0].recordsetTtl | int | 名称服务器中记录集合信息的更新周期 |
| recordsetList[0].recordsetStatus | String | 记录集合状态 |
| recordsetList[0].createdAt | DateTime | 创建日 |
| recordsetList[0].updatedAt | DateTime | 修改日 |
| recordsetList[0].recordList | List | 记录列表 |
| recordsetList[0].recordList[0].recordDisabled | boolean | 记录是否禁用 |
| recordsetList[0].recordList[0].recordContent | String | 为记录值，且以一行显示的不同记录集合类型的具体字段内容 |


### 创建记录集合

- 创建记录集合。
- 以**记录集合类型**支持 A, AAAA, CAA, CNAME, MX, NAPTR, PTR, TXT, SRV, SPF, NS, SOA。
- SOA的记录集合无法创建/修改/删除，NS记录集合无法以**DNS Zone名**创建/修改/删除。
- 记录集合内的记录列表的长度最大为512个字节。
- DNS Zone당 레코드 세트는 최대 5,000개까지 생성할 수 있습니다.

#### 请求

[URI]

| 方法 | URI |
|---|---|
| POST | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[请求正文]

- {appkey}更改为在控制台中确认的值。
- {zoneId}为DNS Zone ID，可通过[查询DNS Zone](./api-guide/#dns-zone)确认。
- 记录值为必需，输入方法可选择recordset.recordList[0].recordContent字段或具体字段。
- recordContent字段是将空格作为区分字符，以一行显示具体字段的内容。具体字段可在 [不同记录集合类型的具体字段] 中确认。
- 若同时输入具体字段与recordContent字段，以recordContent字段为准创建。

```
curl -X POST 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets' \
-H 'Content-Type:application/json' \
--data '{ "recordset": { "recordsetName": "sub.test.dnsplus.com.", "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }}'
```

[字段]

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| recordset | Object |  | 必需 |  | 记录集合 |
| recordset.recordsetName | String | 最大254个字符<br>(含DNS Zone名) | 必需 |  | 要创建的记录集合名，<br>域名以[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)输入 |
| recordset.recordsetType | String | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, SPF, NS | 必需 |  | 记录集合类型 |
| recordset.recordsetTtl | int | 最小1，最大2147483647 | 必需 |  | 名称服务器中记录集合信息的更新周期 |
| recordset.recordList | List |  | 必需 |  | 记录列表 |
| recordset.recordList[0].recordDisabled | boolean |  | 选择 | false | 记录是否禁用 |
| recordset.recordList[0].recordContent | String |  | 必需 |  | 以一行显示的不同记录集合类型的具体字段内容 |

[不同记录集合类型的具体字段]

- A记录集合
    - 可以输入多个记录。
    - 一个域名可绑定多个IPv4地址。

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| recordset.recordList[0].ipV4 | String |  | 必需 |  | IPv4格式的地址 |


- AAAA记录集合
    - 可以输入多个记录。
    - 一个域名可绑定多个IPv6地址

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| recordset.recordList[0].ipV6 | String |  | 必需 |  | IPv6格式的地址 |


- CAA记录集合
    - 可以输入多个记录。
    - 域名若指定允许发放证书的认证机构(CA)，则可防止未经许可的认证机构(CA)发放证书。
    - issue标签是发放与域名或下级域名相关的证书的权限。
    - issuewild标签是发放与域名或下级域名相关的通配符证书的权限。
        - issue标签与issuewild标签的设置方法相同。
        - 允许签发认证书：输入认证机构地址，若需要附加设置，以分号(;)隔开，指定成对“名=值”
        - 禁止签发认证书：输入分号(;)
    - iodef标签在认证机构(CA)收到错误请求时，向设置的邮箱或URL地址进行通知。
        - 邮箱输入格式："mailto:*email-address*"
        - URL地址输入格式：“http://*URL*”或"https://*URL*"
    - 用户指定标签是认证机构(CA)支持除RFC标准外的附加功能时的设置。

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| recordset.recordList[0].flags | int | 0或128 | 必需 |  | 为已定义标签时为0,<br>为用户指定标签时为128 |
| recordset.recordList[0].tag | String | TAG_ISSUE, <br>TAG_ISSUEWILD, <br>TAG_IODEF, <br>用户指定标签最大15 | 必需 |  | TAG_ISSUE: issue标签，<br>TAG_ISSUEWILD：issuewild标签，<br>TAG_IODEF：iodef标签，<br>用户指定标签 |
| recordset.recordList[0].stringValue | String | 最大512个字符(含引用符号) | 必需 |  | 以标签为准的内容 |


- CNAME记录集合
    - 可以输入一个记录。
    - 记录集合名定义为正规名的别名(canonical)。
    - 应首先设置A记录后再设置CNAME。

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | 最大255个字符 | 必需 |  | 域名以[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)输入 |


- MX记录集合
    - 可以输入多个记录。
    - 指定域名相关邮件服务器。

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| recordset.recordList[0].priority | int | 最小0，最大65535 | 必需 |  | 优先顺序 |
| recordset.recordList[0].domainName | String | 最大255个字符 | 必需 |  | 域名以[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)输入 |


- NAPTR记录集合
    - 可以输入多个记录。
    - 用于在DDDS(Dynamic Delegation Discovery System)应用程序中将一个值更改或替代为其他值。
    - 顺序项目是DDDS应用程序评估记录的顺序。
    - 偏好顺序项目是两个以上记录的顺序项目相同时优先评估的顺序。
    - 分类项目作为DDDS应用程序设置，可使用空白、’S’、’A’、’U’、’P’，其他文字为预约。
    - 服务项目为DDDS应用程序设置，具体定义可在RFC文件中确认。
        - URI DDDS应用程序[RFC 3404#section-4.4](https://tools.ietf.org/html/rfc3404#section-4.4)
        - S-NAPTR DDDS应用程序[RFC 3958#section-6.5](https://tools.ietf.org/html/rfc3958#section-6.5)
        - U-NAPTR DDDS应用程序[RFC 4848#section-4.5](https://tools.ietf.org/html/rfc4848#section-4.5)
    - 正规式项目用于在DDDS应用程序中将输入值转换为输出值。具体定义可在[RFC 3402#section-3.2](https://tools.ietf.org/html/rfc3402#section-3.2)中确认。
    - 替代值项目以DDDS应用程序提交DNS Query的域名替代输入值。设置正规式项目时，设置为‘.’。

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| recordset.recordList[0].order | int | 最小0，最大65535 | 必需 |  | 顺序 |
| recordset.recordList[0].preference | int | 最小0，最大65535 | 必需 |  | 偏好顺序 |
| recordset.recordList[0].flags | String | 最大3个字符(含引用符号) | 必需 |  | 分类 |
| recordset.recordList[0].service | String | 最大257个字符(含引用符号) | 必需 |  | 服务 |
| recordset.recordList[0].regexp | String | 最大257个字符(含引用符号) | 必需 |  | 正规式 |
| recordset.recordList[0].replacement | String | 最大255个字符 | 必需 |  | 替代值 '.'或域名以[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)输入 |


- PTR记录集合
    - 可以输入多个记录。
    - 是使用IP地址查询域名信息的逆向答疑功能应向ISP企业申请设置。
    - IP地址应逆序输入到记录集合名中。(范例) 127.0.0.1, 1.0.0.127.in-addr.arpa

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | 最大255个字符 | 必需 |  | 域名以[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)输入 |


- TXT记录集合
    - 可以输入多个记录。
    - 输入与记录集合名相关的文本内容。

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| recordset.recordList[0].stringValue | String | 最大255字节(含引用符号) | 必需 |  | 文本内容 |


- SRV记录集合
    - 可以输入多个记录。
    - 可使用单一DNS Query操作查找提供类似基于TCP/IP服务的多种服务器。

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| recordset.recordList[0].priority | int | 最小0，最大65535 | 必需 |  | 优先顺序 |
| recordset.recordList[0].weight | int | 最小0，最大65535 | 必需 |  | 加权值 |
| recordset.recordList[0].port | int | 最小0，最大65535 | 必需 |  | 端口 |
| recordset.recordList[0].domainName | String | 最大255个字符 | 必需 |  | 域名以[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)输入 |


- SPF记录集合
    - 可以输入多个记录。
    - 利用邮件发送人域名认证方式，确认收件服务器与发件服务器的邮箱地址是否一致的功能。
    - 可以如下形式输入，具体定义可在[RFC4408](https://tools.ietf.org/html/rfc4408)中确认。
    - 限定符的默认值为‘+’，根据机制可补充输入IP、域名等。
        - 形态：“v=spf1 {限定符}{机制}{内容} {更正符}={内容}"
        - 限定符：'+'(Pass), '-'(Fail), '~'(Soft Fail), '?'(Neutral)
        - 机制：all, include, a, mx, prt, ip4, ip6, exists
        - 更正符：redirect, exp, 用户指定
        - (范例)
            - "v=spf1 mx -all"
            - "v=spf1 ip4:192.168.0.1/16 -all"
            - "v=spf1 a:toast.com -all"
            - "v=spf1 redirect=toast.com"

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| recordset.recordList[0].stringValue | String | 最大255字节(含引用符号) | 必需 |  | 以SPF格式为准内容 |


- NS记录集合
    - 可以输入多个记录。
    - 指定与记录集合名相关的名称服务器。
    - 记录集合名仅可以DNS Zone名的下级域名创建或修改。

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | 最大255个字符 | 必需 |  | 域名以[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)输入 |


#### 响应

[响应正文]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "recordset": {
        "recordsetId": "d0b7ee57-8e41-438f-ad04-d4b316793d42",
        "recordsetName": "sub.test.dnsplus.com.",
        "recordsetType": "A",
        "recordsetTtl": 86400,
        "recordsetStatus": "USE",
        "createdAt": "2019-06-04T12:32:50.000+09:00",
        "updatedAt": "2019-06-04T12:32:50.000+09:00",
        "recordList": [
            {
                "recordDisabled": false,
                "recordContent": "1.1.1.1",
                "ipV4": "1.1.1.1"
            }
        ]
    }
}
```


### 修改记录集合

- 修改记录集合。
- **记录集合名**与**记录集合类型**无法修改，**TTL(秒)**与**记录值**可修改。
- SOA的记录集合无法创建/修改/删除，NS记录集合无法以**DNS Zone名**创建/修改/删除。
- 记录集合内记录列表的长度最大为512个字节。

#### 请求

[URI]

| 方法 | URI |
|---|---|
| PUT | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/{recordsetId} |

[请求正文]

- {appkey}更改为在控制台中确认的值。
- {zoneId}为DNS Zone ID，可通过[查询DNS Zone](./api-guide/#dns-zone)确认。
- {recordsetId}为记录集合ID，可通过[查询记录集合](./api-guide/#_11)确认。
- 记录值为必需，输入方法可选择recordset.recordList[0].recordContent字段或具体字段。
- recordContent字段是将空格作为区分字符，以一行显示具体字段的内容。具体字段可在 [创建记录集合](./api-guide/#_14) 的 [不同记录集合类型的具体字段] 中确认。
- 若同时输入具体字段与recordContent字段，以recordContent字段为准修改。

```
curl -X POST 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/{recordsetId}' \
-H 'Content-Type:application/json' \
--data '{ "recordset": { "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }}'
```

[字段]

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| recordset | Object |  | 必需 |  | 记录集合 |
| recordset.recordsetType | String | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, SPF, NS | 必需 |  | 与记录集合ID相关的记录集合类型 |
| recordset.recordsetTtl | int | 最小1，最大2147483647 | 必需 |  | 名称服务器中记录集合信息的更新周期 |
| recordset.recordList | List |  | 必需 |  | 记录列表 |
| recordset.recordList[0].recordDisabled | boolean |  | 必需 |  | 记录是否禁用 |
| recordset.recordList[0].recordContent | String |  | 必需 |  | 以一行显示的不同记录集合类型的具体字段内容 |


#### 响应

[响应正文]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "recordset": {
        "recordsetId": "d0b7ee57-8e41-438f-ad04-d4b316793d42",
        "recordsetName": "sub.test.dnsplus.com.",
        "recordsetType": "A",
        "recordsetTtl": 86400,
        "recordsetStatus": "USE",
        "createdAt": "2019-06-04T12:32:50.000+09:00",
        "updatedAt": "2019-06-04T12:42:00.000+09:00",
        "recordList": [
            {
                "recordDisabled": false,
                "recordContent": "1.1.1.1",
                "ipV4": "1.1.1.1"
            }
        ]
    }
}
```


### 删除记录集合

- 删除多个记录集合，记录集合的记录也一同删除。
- SOA的记录集合无法创建/修改/删除，NS记录集合无法以**DNS Zone名**创建/修改/删除。

#### 请求

[URI]

| 方法 | URI |
|---|---|
| DELETE | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[请求正文]

- {appkey}更改为在控制台中确认的值。
- {zoneId}为DNS Zone ID，可通过[查询DNS Zone](./api-guide/#dns-zone)确认。
- 记录集合ID可通过[查询记录集合](./api-guide/#_11)确认。

```
curl -X DELETE 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets?
recordsetIdList=edb9512b-6e62-409c-99ee-092d340e0adf,edb9512b-6e62-409c-99ee-092d340e0adf'
```

[字段]

| 名称 | 类型 | 有效范围 | 是否必需 | 默认值 | 说明 |
|---|---|---|---|---|---|
| recordsetIdList | List | 最小1个，最大3,000个 | 必需 |  | 记录集合ID列表 |

#### 响应

[响应正文]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```
