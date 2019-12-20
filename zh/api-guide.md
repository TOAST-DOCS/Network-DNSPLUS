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
- 每个DNS区域最多可以创建5,000个记录集。

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
    - 对于相同的记录集合名，无其他记录集合类型时，可创建CNAME记录集合。

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

## GSLB API

### GSLB 조회

- GSLB 목록을 조회합니다.
- Pool에 헬스 체크가 연결되어 있는 경우 GSLB 정상 상태와 Pool 정상 상태, 엔드포인트 정상 상태를 알 수 있습니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| GET | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/gslbs |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X GET 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/gslbs?showHealthy=true'
```

[옵션]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| gslbIdList | List | 최대 3,000개 | 선택 |  | GSLB ID 목록 |
| searchGslbName | String |  | 선택 |  | 검색할 GSLB 이름 |
| gslbDomain | String |  | 선택 |  | GSLB 도메인 |
| showHealthy | boolean |  | 선택 |  | 헬스 체크 결과 보기 여부 |
| page | int | 최소 1 | 선택 | 1 | 페이지 번호 |
| limit | int | 최소 1, 최대 3,000 | 선택 | 50 | 조회 개수 |
| sortDirection | String | DESC, ASC | 선택 | DESC | 정렬 방향(DESC: 내림차순, ASC: 오름차순) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>GSLB_NAME, <br>GSLB_DOMAIN, <br>GSLB_TTL, <br>GSLB_ROUTING_RULE, <br>GSLB_DISABLED | 선택 | CREATED_AT | 정렬 대상 <br>(CREATED_AT: 생성일, <br>UPDATED_AT: 수정일, <br>GSLB_NAME: GSLB 이름, <br>GSLB_DOMAIN: GSLB 도메인, <br>GSLB_TTL: GSLB 도메인 갱신 주기, <br>GSLB_ROUTING_RULE: 라우팅 규칙, <br>GSLB_DISABLED: GSLB 비활성화 여부) |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "totalCount": 1,
    "gslbList": [
        {
            "gslbId": "91de0c6f-aeaa-44ec-b361-822acfcd5921",
            "gslbName": "GSLB-test",
            "gslbDomain": "rgpac3e7q9onlipdfg.toastgslb.com",
            "gslbTtl": 300,
            "gslbRoutingRule": "GEOLOCATION",
            "gslbDisabled": false,
            "healthy": true,
            "connectedPoolList": [
                {
                    "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
                    "connectedPoolOrder": 1,
                    "pool": {
                        // Pool 정보 생략
                    }
                },
                {
                    "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
                    "connectedPoolOrder": 2,
                    "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
                    "pool": {
                        // Pool 정보 생략
                    }
                }
            ],
            "createdAt": "2019-12-18T20:44:02.000+09:00",
            "updatedAt": "2019-12-18T21:01:05.000+09:00"
        }
    ]
}
```

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| totalCount | long | 전체 GSLB 개수 |
| gslbList | List | Pool 목록 |
| gslbList[0].gslbId | String | GSLB ID |
| gslbList[0].gslbName | String | GSLB 이름 |
| gslbList[0].gslbDomain | String | GSLB 도메인 |
| gslbList[0].gslbTtl | String | GLSB 도메인 갱신 주기 |
| gslbList[0].gslbRoutingRule | String | 라우팅 규칙 |
| gslbList[0].gslbDisabled | boolean | GSLB 비활성화 여부 |
| gslbList[0].healthy | boolean | GSLB 정상 여부 |
| gslbList[0].connectedPoolList | List | 연결된 Pool 목록 |
| gslbList[0].connectedPoolList[0].poolId | String | 연결된 Pool Id |
| gslbList[0].connectedPoolList[0].pool | Object | 연결된 Pool 정보 |
| gslbList[0].connectedPoolList[0].connectedPoolOrder | int | 연결된 Pool 우선순위 |
| gslbList[0].connectedPoolList[0].connectedPoolRegionContent | String | 연결된 Pool의 지역을 한 줄로 표시한 내용 |
| gslbList[0].createdAt | DateTime | 생성일 |
| gslbList[0].updatedAt | DateTime | 수정일 |


### GSLB 생성

- GSLB와 Pool 연결 설정을 생성합니다.
- **라우팅 규칙**은 GSLB 도메인에 대한 로드밸런싱 방법으로 FAILOVER, RANDOM, GEOLOCATION을 선택할 수 있습니다.
    - FAILOVER: 연결된 Pool의 우선순위로 라우팅 합니다.
	- RANDOM: 연결된 Pool 중 사용 가능한 Pool을 무작위로 선택하여 라우팅 합니다.
	- GEOLOCATION: 설정된 지역의 트래픽을 해당 연결된 Pool로 라우팅합니다. 지역 설정이 없는 경우 우선순위로 라우팅합니다.
- **연결된 Pool**의 **우선순위**는 작을수록 라우팅 순서가 높으며, 중복될 수 없습니다.
- GSLB 생성 개수와 Pool 연결 개수는 제한되어 있으며 연장이 필요한 경우 별도로 문의해 주시기 바랍니다. [1:1 문의](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| POST | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/gslbs |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- connectedPoolRegionContent 필드는 쉼표(,)를 구분 문자로 하여 **지역**을 한 줄로 작성합니다.

```
curl -X POST 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/gslbs' \
-H 'Content-Type: application/json' \
--data '{ "gslb": { "gslbName": "GSLB-test", "gslbTtl": 300, "gslbRoutingRule": "FAILOVER", "connectedPoolList": [ { "poolId": "8e4326d4-3862-4b46-819e-83a786add570", "connectedPoolOrder": 1 }, { "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818", "connectedPoolOrder": 2 } ] }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| gslb | Object |  | 필수 |  | GSLB |
| gslb.gslbName | String | 최대 100자,<br>영대소문자와 숫자, '-', '_' | 필수 |  | GSLB 이름 |
| gslb.gslbTtl | int |  | 필수 | false | GSLB 도메인 갱신 주기 |
| gslb.gslbRoutingRule | String | FAILOVER, RANDOM, GEOLOCATION  | 필수 |  | 라우팅 규칙 |
| gslb.gslbDisabled | boolean |  | 선택 | false | GSLB 비활성화 여부 |
| gslb.connectedPoolList | List |  | 필수 |  | 연결된 Pool 목록 |
| gslb.connectedPoolList[0].poolId | String |  | 필수 |  | 연결된 Pool ID |
| gslb.connectedPoolList[0].connectedPoolOrder | int | 최소 1, 최대 2,147,483,647 | 필수 |  | 연결된 Pool 우선순위 |
| gslb.connectedPoolList[0].connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | 선택 |  | 연결된 Pool 지역 설정 |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "gslb": {
        "gslbId": "91de0c6f-aeaa-44ec-b361-822acfcd5921",
        "gslbName": "GSLB-test",
        "gslbDomain": "rgpac3e7q9onlipdfg.toastgslb.com",
        "gslbTtl": 300,
        "gslbRoutingRule": "FAILOVER",
        "gslbDisabled": false,
        "connectedPoolList": [
            {
                "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
                "connectedPoolOrder": 1,
                "pool": {
                    // Pool 정보 생략
                }
            },
            {
                "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
                "connectedPoolOrder": 2,
                "pool": {
                    // Pool 정보 생략
                }
            }
        ],
        "createdAt": "2019-12-18T20:44:02.000+09:00",
        "updatedAt": "2019-12-18T20:44:03.000+09:00"
    }
}
```


### GSLB 수정

- GSLB와 Pool 연결 설정을 수정합니다.
- [GSLB 생성](./api-guide/#gslb_1)에서 입력한 항목을 수정합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| PUT | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId} |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {gslbId}는 GSLB ID이며 [GSLB 조회](./api-guide/#gslb)를 통해서 알 수 있습니다.
- connectedPoolRegionContent 필드는 쉼표(,)를 구분 문자로 하여 **지역**을 한 줄로 작성합니다.

```
curl -X PUT 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}' \
-H 'Content-Type: application/json' \
--data '{ "gslb": { "gslbName": "GSLB-test", "gslbTtl": 300, "gslbDisabled": true, "gslbRoutingRule": "GEOLOCATION", "connectedPoolList": [ { "poolId": "8e4326d4-3862-4b46-819e-83a786add570", "connectedPoolOrder": 1 }, { "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818", "connectedPoolOrder": 2, "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA" } ] }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| gslb | Object |  | 필수 |  | GSLB |
| gslb.gslbName | String | 최대 100자,<br>영대소문자와 숫자, '-', '_' | 필수 |  | GSLB 이름 |
| gslb.gslbTtl | int |  | 필수 | false | GSLB 도메인 갱신 주기 |
| gslb.gslbRoutingRule | String | FAILOVER, RANDOM, GEOLOCATION  | 필수 |  | 라우팅 규칙 |
| gslb.gslbDisabled | boolean |  | 선택 | false | GSLB 비활성화 여부 |
| gslb.connectedPoolList | List |  | 필수 |  | 연결된 Pool 목록 |
| gslb.connectedPoolList[0].poolId | String |  | 필수 |  | 연결된 Pool ID |
| gslb.connectedPoolList[0].connectedPoolOrder | int | 최소 1, 최대 2,147,483,647 | 필수 |  | 연결된 Pool 우선순위 |
| gslb.connectedPoolList[0].connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | 선택 |  | 연결된 Pool 지역 설정 |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "gslb": {
        "gslbId": "91de0c6f-aeaa-44ec-b361-822acfcd5921",
        "gslbName": "GSLB-test",
        "gslbDomain": "rgpac3e7q9onlipdfg.toastgslb.com",
        "gslbTtl": 300,
        "gslbRoutingRule": "GEOLOCATION",
        "gslbDisabled": true,
        "connectedPoolList": [
            {
                "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
                "connectedPoolOrder": 1,
                "pool": {
                    // Pool 정보 생략
                }
            },
            {
                "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
                "connectedPoolOrder": 2,
                "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
                "pool": {
                    // Pool 정보 생략
                }
            }
        ],
        "createdAt": "2019-12-18T20:44:02.000+09:00",
        "updatedAt": "2019-12-18T20:59:49.000+09:00"
    }
}
```


### GSLB 삭제

- 여러 개의 GSLB를 삭제합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| DELETE | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/gslbs |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X DELETE 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/gslbs?
gslbIdList=91de0c6f-aeaa-44ec-b361-822acfcd5921,269eff10-f3c0-4b11-b072-ec53e7c604bf'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| gslbIdList | List | 최소 1개, 최대 3,000개 | 필수 |  | GSLB ID 목록 |

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


### Pool 연결

- GSLB에 Pool을 연결합니다.
- **연결된 Pool**의 **우선순위**는 작을수록 라우팅 순서가 높으며, 기존에 연결된 Pool과 동일한 우선순위를 입력한 경우 기존 Pool의 라우팅 순서가 낮아집니다.
- Pool 연결 개수는 제한되어 있으며 연장이 필요한 경우 별도로 문의해 주시기 바랍니다. [1:1 문의](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| POST | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId} |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {gslbId}는 GSLB ID이며 [GSLB 조회](./api-guide/#gslb)를 통해서 알 수 있습니다.
- {poolId}는 Pool ID이며 [Pool 조회](./api-guide/#pool_3)를 통해서 알 수 있습니다.
- connectedPoolRegionContent 필드는 쉼표(,)를 구분 문자로 하여 **지역**을 한 줄로 작성합니다.

```
curl -X POST 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId}' \
-H 'Content-Type: application/json' \
--data '{ "connectedPool": { "connectedPoolOrder": 1 } }'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| connectedPool | Object |  | 필수 |  | 연결된 Pool |
| connectedPool.connectedPoolOrder | int | 최소 1, 최대 2,147,483,647 | 필수 |  | 연결된 Pool 우선순위 |
| connectedPool.connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | 선택 |  | 연결된 Pool 지역 설정 |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "connectedPoolList": [
        {
            "poolId": "52da0e48-9062-43f7-bef8-8aec4b795bfe",
            "connectedPoolOrder": 1,
            "pool": {
                // Pool 정보 생략
            }
        },
        {
            "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
            "connectedPoolOrder": 2,
            "pool": {
                // Pool 정보 생략
            }
        },
        {
            "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
            "connectedPoolOrder": 3,
            "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
            "pool": {
                // Pool 정보 생략
            }
        }
    ]
}
```

### Pool 연결 수정

- GSLB에 연결된 Pool 설정을 수정합니다.
- [GSLB 생성](./api-guide/#gslb_1)의 Pool 설정 또는 [Pool 연결](./api-guide/#pool)에서 입력한 항목을 수정합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| PUT | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId} |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {gslbId}는 GSLB ID이며 [GSLB 조회](./api-guide/#gslb)를 통해서 알 수 있습니다.
- {poolId}는 Pool ID이며 [Pool 조회](./api-guide/#pool_3)를 통해서 알 수 있습니다.
- connectedPoolRegionContent 필드는 쉼표(,)를 구분 문자로 하여 **지역**을 한 줄로 작성합니다.

```
curl -X POST 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId}' \
-H 'Content-Type: application/json' \
--data '{ "connectedPool": { "connectedPoolOrder": 1, "connectedPoolRegionContent": "WESTERN_NORTH_AMERICA" } }'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| connectedPool | Object |  | 필수 |  | 연결된 Pool |
| connectedPool.connectedPoolOrder | int | 최소 1, 최대 2,147,483,647 | 필수 |  | 연결된 Pool 우선순위 |
| connectedPool.connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | 선택 |  | 연결된 Pool 지역 설정 |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "connectedPoolList": [
        {
            "poolId": "52da0e48-9062-43f7-bef8-8aec4b795bfe",
            "connectedPoolOrder": 1,
            "connectedPoolRegionContent": "WESTERN_NORTH_AMERICA",
            "pool": {
                // Pool 정보 생략
            }
        },
        {
            "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
            "connectedPoolOrder": 2,
            "pool": {
                // Pool 정보 생략
            }
        },
        {
            "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
            "connectedPoolOrder": 3,
            "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
            "pool": {
                // Pool 정보 생략
            }
        }
    ]
}
```

### Pool 연결 해제

- GSLB에 연결된 여러 개의 Pool을 해제합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| DELETE | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {gslbId}는 GSLB ID이며 [GSLB 조회](./api-guide/#gslb)를 통해서 알 수 있습니다.

```
curl -X DELETE 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools?
poolIdList=52da0e48-9062-43f7-bef8-8aec4b795bfe,12bc396a-eb97-4a6b-ab4c-73d1a1dfb093'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| poolIdList | List | 최소 1개, 최대 3,000개 | 필수 |  | Pool ID 목록 |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "connectedPoolList": [
        {
            "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
            "connectedPoolOrder": 2,
            "pool": {
                // Pool 정보 생략
            }
        },
        {
            "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
            "connectedPoolOrder": 3,
            "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
            "pool": {
                // Pool 정보 생략
            }
        }
    ]
}
```


## Pool API

### Pool 조회

- Pool 목록을 조회합니다.
- 헬스 체크가 연결되어 있는 경우 Pool 정상 상태와 엔드포인트 정상 상태를 알 수 있습니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| GET | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/pools |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X GET 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/pools?showHealthy=true'
```

[옵션]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| poolIdList | List | 최대 3,000개 | 선택 |  | Pool ID 목록 |
| searchPoolName | String |  | 선택 |  | 검색할 Pool 이름 |
| healthCheckId | String |  | 선택 |  | 연결 된 헬스 체크 ID |
| showHealthy | boolean |  | 선택 |  | 헬스 체크 결과 보기 여부 |
| page | int | 최소 1 | 선택 | 1 | 페이지 번호 |
| limit | int | 최소 1, 최대 3,000 | 선택 | 50 | 조회 개수 |
| sortDirection | String | DESC, ASC | 선택 | DESC | 정렬 방향(DESC: 내림차순, ASC: 오름차순) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>POOL_NAME, <br>POOL_DISABLED, <br>HEALTH_CHECK_ID | 선택 | CREATED_AT | 정렬 대상 <br>(CREATED_AT: 생성일, <br>UPDATED_AT: 수정일, <br>POOL_NAME: Pool 이름, <br>POOL_DISABLED: Pool 비활성화 여부, <br>HEALTH_CHECK_ID: 연결된 헬스 체크 ID) |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "totalCount": 1,
    "poolList": [
        {
            "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
            "poolName": "POOL-test",
            "poolDisabled": false,
            "healthy": true,
            "healthCheckId": "b9165853-7859-4309-8059-48f12ebdbc17",
            "healthCheck": {
                // 헬스 체크 정보 생략
            },
            "endpointList": [
                {
                    "endpointAddress": "test.dnsplus.com",
                    "endpointWeight": 1.0,
                    "endpointDisabled": false,
                    "healthy": true,
                    "failureReason": "No failures"
                },
                {
                    "endpointAddress": "123.123.123.123",
                    "endpointWeight": 1.0,
                    "endpointDisabled": false,
                    "healthy": false,
                    "failureReason": "HTTP timeout occurred"
                },
                {
                    "endpointAddress": "test2.dnsplus.com",
                    "endpointWeight": 1.0,
                    "endpointDisabled": true
                }
            ],
            "createdAt": "2019-12-18T18:36:02.000+09:00",
            "updatedAt": "2019-12-18T18:43:31.000+09:00"
        }
    ]
}
```

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| totalCount | long | 전체 Pool 개수 |
| poolList | List | Pool 목록 |
| poolList[0].poolId | String | Pool ID |
| poolList[0].poolName | String | Pool 이름 |
| poolList[0].poolDisabled | boolean | Pool 비활성화 여부 |
| poolList[0].healthy | boolean | Pool 정상 여부 |
| poolList[0].healthCheckId | String | 연결된 헬스 체크 ID |
| poolList[0].healthCheck | Object | 연결된 헬스 체크 정보 |
| poolList[0].endpointList | List | 엔드포인트 목록 |
| poolList[0].endpointList[0].endpointAddress | String | 엔드포인트 주소 |
| poolList[0].endpointList[0].endpointWeight | double | 엔드포인트 가중치 |
| poolList[0].endpointList[0].healthy | boolean | 엔드포인트 정상 여부 |
| poolList[0].endpointList[0].failureReason | String | 엔드포인트 비정상 이유 |
| poolList[0].createdAt | DateTime | 생성일 |
| poolList[0].updatedAt | DateTime | 수정일 |


### Pool 생성

- Pool과 Pool 내에 엔드포인트를 생성합니다.
- Pool 내의 엔드포인트의 접근성을 확인할 **헬스 체크**를 설정할 수 있습니다.
- **엔드포인트 주소**는 도메인 주소 또는 IPv4로 입력할 수 있으며 제한된 입력이 있습니다.
    - 하이픈과 마침표로 시작할 수 없으며 하이픈으로 끝날 수 없습니다. 마침표와 하이픈을 연이어서 입력할 수 없습니다.
    - [예약된 IP 주소](https://en.wikipedia.org/wiki/Reserved_IP_addresses)는 입력할 수 없습니다.
    - Pool 내에서 중복될 수 없습니다.
- 엔드포인트의 **가중치**는 Pool 내의 다른 엔드포인트 가중치와 상대적으로 동작합니다. 동일한 가중치는 Pool 내에서 동일한 비중을 가집니다.
- Pool의 생성 개수, Pool 내의 엔드포인트 개수, 전체 엔드포인트의 개수는 제한되어 있으며 연장이 필요한 경우 별도로 문의해 주시기 바랍니다. [1:1 문의](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| POST | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/pools |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X POST 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/pools' \
-H 'Content-Type: application/json' \
--data '{ "pool": { "poolName": "POOL-test", "endpointList": [ { "endpointAddress": "test.dnsplus.com" }, { "endpointAddress": "123.123.123.123" } ] }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| pool | Object |  | 필수 |  | Pool |
| pool.poolName | String | 최대 100자,<br>영대소문자와 숫자, '-', '_' | 필수 |  | Pool 이름 |
| pool.poolDisabled | boolean |  | 선택 | false | Pool 비활성화 여부 |
| pool.healthCheckId | String |  | 선택 |  | 헬스 체크 ID |
| pool.endpointList | List |  | 필수 |  | 엔드포인트 목록 |
| pool.endpointList[0].endpointAddress | String | 최대 254자,<br>소문자와 숫자, '.', '-', '_' | 필수 |  | 엔드포인트 주소 |
| pool.endpointList[0].endpointWeight | double | 최소 0, 최대 1.00 | 선택 | 1.00 | 엔드포인트 가중치 |
| pool.endpointList[0].endpointDisabled | boolean |  | 선택 | false | 엔드포인트 비활성화 여부 |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "pool": {
        "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
        "poolName": "POOL-test",
        "poolDisabled": false,
        "healthCheckId": "",
        "endpointList": [
            {
                "endpointAddress": "test.dnsplus.com",
                "endpointWeight": 1.0,
                "endpointDisabled": false
            },
            {
                "endpointAddress": "123.123.123.123",
                "endpointWeight": 1.0,
                "endpointDisabled": false
            }
        ],
        "createdAt": "2019-12-18T18:36:02.000+09:00",
        "updatedAt": "2019-12-18T18:36:02.000+09:00"
    }
}
```


### Pool 수정

- Pool과 Pool 내에 엔드포인트를 수정합니다.
- [Pool 생성](./api-guide/#pool_4)에서 입력한 항목을 수정합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| PUT | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/pools/{poolId} |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {poolId}는 Pool ID이며 [Pool 조회](./api-guide/#pool_3)를 통해서 알 수 있습니다.

```
curl -X PUT 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/pools/{poolId}' \
-H 'Content-Type: application/json' \
--data '{ "pool": { "poolName": "POOL-test", "poolDisabled": true, "healthCheckId": "b9165853-7859-4309-8059-48f12ebdbc17", "endpointList": [ { "endpointAddress": "test.dnsplus.com", "endpointWeight": 1.00, "endpointDisabled": true }, { "endpointAddress": "123.123.123.123", "endpointWeight": 0.5, "endpointDisabled": true } ] }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| pool | Object |  | 필수 |  | Pool |
| pool.poolName | String | 최대 100자,<br>영대소문자와 숫자, '-', '_' | 필수 |  | Pool 이름 |
| pool.poolDisabled | boolean |  | 선택 | false | Pool 비활성화 여부 |
| pool.healthCheckId | String |  | 선택 |  | 헬스 체크 ID |
| pool.endpointList | List |  | 필수 |  | 엔드포인트 목록 |
| pool.endpointList[0].endpointAddress | String | 최대 254자,<br>소문자와 숫자, '.', '-', '_' | 필수 |  | 엔드포인트 주소 |
| pool.endpointList[0].endpointWeight | double | 최소 0, 최대 1.00 | 선택 | 1.00 | 엔드포인트 가중치 |
| pool.endpointList[0].endpointDisabled | boolean |  | 선택 | false | 엔드포인트 비활성화 여부 |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "pool": {
        "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
        "poolName": "POOL-test",
        "poolDisabled": true,
        "healthCheckId": "b9165853-7859-4309-8059-48f12ebdbc17",
        "healthCheck": {
            // 헬스 체크 정보 생략
        },
        "endpointList": [
            {
                "endpointAddress": "test.dnsplus.com",
                "endpointWeight": 1.0,
                "endpointDisabled": true
            },
            {
                "endpointAddress": "123.123.123.123",
                "endpointWeight": 0.5,
                "endpointDisabled": true
            }
        ],
        "createdAt": "2019-12-18T18:36:02.000+09:00",
        "updatedAt": "2019-12-18T18:37:45.000+09:00"
    }
}
```


### Pool 삭제

- 여러 개의 Pool을 삭제하며, Pool의 엔드포인트도 함께 삭제합니다.
- GSLB에 연결되어 있는 Pool은 삭제할 수 없습니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| DELETE | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/pools |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X DELETE 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/pools?
poolIdList=8e4326d4-3862-4b46-819e-83a786add570,2f89d3fe-03bc-4711-826e-db2c89c12818'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| poolIdList | List | 최소 1개, 최대 3,000개 | 필수 |  | Pool ID 목록 |

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


## 헬스 체크 API

### 헬스 체크 조회

- 헬스 체크 목록을 조회합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| GET | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/health-checks |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X GET 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/health-checks'
```

[옵션]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| healthCheckIdList | List | 최대 3,000개 | 선택 |  | 헬스 체크 ID 목록 |
| searchHealthCheckName | String |  | 선택 |  | 검색할 헬스 체크 이름 |
| page | int | 최소 1 | 선택 | 1 | 페이지 번호 |
| limit | int | 최소 1, 최대 3,000 | 선택 | 50 | 조회 개수 |
| sortDirection | String | DESC, ASC | 선택 | DESC | 정렬 방향(DESC: 내림차순, ASC: 오름차순) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>HEALTH_CHECK_NAME, <br>PROTOCOL, <br>PORT | 선택 | CREATED_AT | 정렬 대상 <br>(CREATED_AT: 생성일, <br>UPDATED_AT: 수정일, <br>HEALTH_CHECK_NAME: 헬스 체크 이름, <br>PROTOCOL: 프로토콜, <br>PORT: 포트) |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "totalCount": 1,
    "healthCheckList": [
        {
            "healthCheckId": "b9165853-7859-4309-8059-48f12ebdbc17",
            "healthCheckName": "HTTPS-443",
            "protocol": "HTTPS",
            "port": 443,
            "path": "/",
            "expectedCodes": "2xx",
            "expectedBody": "OK",
            "allowInsecure": false,
            "createdAt": "2019-12-18T12:31:34.000+09:00",
            "updatedAt": "2019-12-18T14:19:20.000+09:00"
        }
    ]
}
```

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| totalCount | long | 전체 헬스 체크 개수 |
| healthCheckList | List | 헬스 체크 목록 |
| healthCheckList[0].healthCheckId | String | 헬스 체크 ID |
| healthCheckList[0].healthCheckName | String | 헬스 체크 이름 |
| healthCheckList[0].protocol | String | 프로토콜 |
| healthCheckList[0].port | int | 포트 |
| healthCheckList[0].path | String | 경로 |
| healthCheckList[0].expectedCodes | String | 예상 상태 코드 |
| healthCheckList[0].expectedBody | String | 예상 응답 본문 |
| healthCheckList[0].allowInsecure | boolean | 인증서 검증 안함 |
| healthCheckList[0].createdAt | DateTime | 생성일 |
| healthCheckList[0].updatedAt | DateTime | 수정일 |


### 헬스 체크 생성

- 헬스 체크를 생성합니다.
- 헬스 체크 **프로토콜**은 HTTPS, HTTP, TCP를 지원하며 선택한 프로토콜에 따라 입력 할 수 있는 정보가 다릅니다.
    - HTTPS 입력 가능 항목: 인증서 검증 안함, 포트, 경로, 예상 상태 코드, 예상 응답 본문
	- HTTP 입력 가능 항목: 포트, 경로, 예상 상태 코드, 예상 응답 본문
	- TCP 입력 가능 항목: 포트
- **인증서 검증 안함**을 사용하면 헬스 체크가 수행될 때 엔드포인트의 TLS/SSL 인증서가 유효하지 않아도 무시할 수 있습니다.
- **예상 상태 코드**와 **예상 응답 본문**을 판단 할 때 엔드포인트에서 리다이렉션 된 페이지에 대해서는 지원하지 않습니다.
- 헬스 체크 생성 개수는 제한되어 있으며 연장이 필요한 경우 별도로 문의해 주시기 바랍니다. [1:1 문의](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| POST | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/health-checks |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X POST 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/health-checks' \
-H 'Content-Type: application/json' \
--data '{ "healthCheck": { "healthCheckName": "HTTPS-443", "protocol": "HTTPS", "port": 443, "path": "/", "expectedCodes": "2xx", "allowInsecure": false }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| healthCheck | Object |  | 필수 |  | 헬스 체크 |
| healthCheck.healthCheckName | String | 최대 100자,<br>영대소문자와 숫자, '-', '_' | 필수 |  | 헬스 체크 이름 |
| healthCheck.protocol | String | HTTPS, HTTP, TCP | 필수 |  | 헬스 체크 수행 프로토콜 |
| healthCheck.port | int | 최소 1, 최대 65535 | 필수 |  | 헬스 체크 수행 포트 |
| healthCheck.path | String | 최대 254자,<br>시작 문자 '/' | 선택 |  | 헬스 체크 수행 경로,<br>HTTPS, HTTP 일 때 사용 |
| healthCheck.expectedCodes | String | 숫자와 와일드카드 'x' | 선택 |  | 헬스 체크 예상 상태 코드,<br>HTTPS, HTTP 일 때 사용<br>(예제) 2xx, 20x, 200 |
| healthCheck.expectedBody | String | 최대 10KB | 선택 |  | 헬스 체크 예상 응답 본문,<br>HTTPS, HTTP 일 때 사용 |
| healthCheck.allowInsecure | boolean |  | 선택 |  | 헬스 체크 인증서 검증 안함,<br>HTTPS 일 때 사용 |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "healthCheck": {
        "healthCheckId": "b9165853-7859-4309-8059-48f12ebdbc17",
        "healthCheckName": "HTTPS-443",
        "protocol": "HTTPS",
        "port": 443,
        "path": "/",
        "expectedCodes": "2xx",
        "allowInsecure": false,
        "createdAt": "2019-12-18T12:31:34.000+09:00",
        "updatedAt": "2019-12-18T12:31:34.000+09:00"
    }
}
```


### 헬스 체크 수정

- 헬스 체크를 수정합니다.
- [헬스 체크 생성](./api-guide/#_48)에서 입력한 항목을 수정합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| PUT | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/health-checks/{healthCheckId} |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {healthCheckId}는 헬스 체크 ID이며 [헬스 체크 조회](./api-guide/#_45)를 통해서 알 수 있습니다.

```
curl -X PUT 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/health-checks/{healthCheckId}' \
-H 'Content-Type: application/json' \
--data '{ "healthCheck": { "healthCheckName": "HTTPS-443", "protocol": "HTTPS", "port": 443, "path": "/", "expectedCodes": "3xx", "allowInsecure": false }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| healthCheck | Object |  | 필수 |  | 헬스 체크 |
| healthCheck.healthCheckName | String | 최대 100자,<br>영대소문자와 숫자, '-', '_' | 필수 |  | 헬스 체크 이름 |
| healthCheck.protocol | String | HTTPS, HTTP, TCP | 필수 |  | 헬스 체크 수행 프로토콜 |
| healthCheck.port | int | 최소 1, 최대 65535 | 필수 |  | 헬스 체크 수행 포트 |
| healthCheck.path | String | 최대 254자,<br>시작 문자 '/' | 선택 |  | 헬스 체크 수행 경로,<br>HTTPS, HTTP 일 때 사용 |
| healthCheck.expectedCodes | String | 숫자와 와일드카드 'x' | 선택 |  | 헬스 체크 예상 상태 코드,<br>HTTPS, HTTP 일 때 사용<br>(예제) 2xx, 20x, 200 |
| healthCheck.expectedBody | String | 최대 10KB | 선택 |  | 헬스 체크 예상 응답 본문,<br>HTTPS, HTTP 일 때 사용 |
| healthCheck.allowInsecure | boolean |  | 선택 |  | 헬스 체크 인증서 검증 안함,<br>HTTPS 일 때 사용 |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "healthCheck": {
        "healthCheckId": "b9165853-7859-4309-8059-48f12ebdbc17",
        "healthCheckName": "HTTPS-443",
        "protocol": "HTTPS",
        "port": 443,
        "path": "/",
        "expectedCodes": "3xx",
        "allowInsecure": false,
        "createdAt": "2019-12-18T12:31:34.000+09:00",
        "updatedAt": "2019-12-18T12:36:20.000+09:00"
    }
}
```


### 헬스 체크 삭제

- 여러 개의 헬스 세트를 삭제합니다.
- Pool에 연결되어 있는 헬스 체크는 삭제할 수 없습니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| DELETE | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/health-checks |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X DELETE 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/health-checks?
healthCheckIdList=b9165853-7859-4309-8059-48f12ebdbc17,d2629d6b-9381-4645-9cf3-43d7ad491e2b'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| healthCheckIdList | List | 최소 1개, 최대 3,000개 | 필수 |  | 헬스 체크 ID 목록 |

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
