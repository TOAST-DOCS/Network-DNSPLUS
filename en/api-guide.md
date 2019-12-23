## Network > DNS Plus > API Guide

The API Guide describes the API of DNS Plus Service.


## Common Information of API

### Preparation

- An app key is required to use the API.
- Your app key is located in the **URL & Appkey** menu on the top of the console.

### Common Information of Responses

- '200 OK' is returned for All API requests. For details on response results, refer to the header of each response.

[Success response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

[Failure response body]

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

### DNS Zone lookup

- Looks up the DNS Zone list.

#### Request

[URI]

| Method | URI |
|---|---|
| GET | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones |

[Request body]

- Changes {appkey} to the value checked in the console.

```
curl -X GET 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones'
```

[Options]

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| zoneIdList | List | Max. 3,000 | Select |  | DNS Zone ID List |
| zoneStatusList | List | CREATING, <br>DELETING, <br>DELETING_FAIL, <br> USE | Select | | DNS Zone status list <br>(CREATING: Creating, <br>DELETING: Deleting, <br>DELETING_FAIL: Failed to delete, <br>USE: Use) |
| searchZoneName | String |  | Select |  | DNS Zone name to search for |
| engineId | String | | Select |  | DNS Server ID |
| page | int | Min. 1 | Select | 1 | Page No. |
| limit | int | Min. 1, Max. 3,000 | Select | 50 | Lookup count |
| sortDirection | String | DESC, ASC | Select | DESC | Sort order(DESC: Descending, ASC: Ascending) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>ZONE_NAME, <br>ZONE_STATUS, <br>RECORDSET_COUNT | Select | CREATED_AT | Object to sort <br>(CREATED_AT: Created date, <br>UPDATED_AT: Modified date, <br>ZONE_NAME: DNS Zone name, <br>ZONE_STATUS: DNS Zone status, <br>RECORDSET_COUNT: Record set count) |

#### Response

[Response body]

```
{
    "header": {
        // Omitted
    },
    "totalCount": 1,
    "zoneList": [
        {
            "engineId": "e13a1bcf0aa8e07f6a4fae94ed869c39",
            "zoneId": "bff20a9a-24cf-4670-8b34-007622ec010e",
            "zoneName": "test.dnsplus.com.",
            "zoneStatus": "USE",
            "description": "Test",
            "createdAt": "2019-06-04T12:32:50.000+09:00",
            "updatedAt": "2019-06-04T12:32:50.000+09:00",
            "recordsetCount": 2
        }
    ]
}
```

[Field]

| Name | Type | Description |
|---|---|---|
| totalCount | long | Total DNS Zone count |
| zoneList | List | DNS Zone list |
| zoneList[0].engineId | boolean | DNS Server ID |
| zoneList[0].zoneId | String | DNS Zone ID |
| zoneList[0].zoneName | String | DNS Zone name |
| zoneList[0].zoneStatus | String | DNS Zone status |
| zoneList[0].description | String | Description |
| zoneList[0].createdAt | DateTime | Created date |
| zoneList[0].updatedAt | DateTime | Modified date |
| zoneList[0].recordsetCount | long | Record set count |


### Create DNS Zone

- Create a DNS Zone.
- Its **DNS Zone name** must be unique within the DNS server.
- The same **DNS Zone name** can be created as many as the number of DNS servers. There are three DNS servers.

#### Request

[URI]

| Method | URI |
|---|---|
| POST | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones |

[Request body]

- Changes {appkey} to the value checked in the console.

```
curl -X POST 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones' \
-H 'Content-Type: application/json' \
--data '{ "zone": { "zoneName": "test.dnsplus.com.", "description": "test" }}'
```

[Field]

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| zone | Object |  | Required |  | DNS Zone |
| zone.zoneName | String | Max. 254 characters | Required |  | DNS Zone name to create, <br>type the domain as [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |
| zone.description | String | Max. 255 characters | Select |  | DNS Zone description |

#### Response

[Response body]

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


### Updating DNS Zone

- Update the DNS Zone.

#### Request

[URI]

| Method | URI |
|---|---|
| PUT | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId} |

[Request body]

- Changes {appkey} to the value checked in the console.
- {zoneId} is the DNS Zone ID, which can be checked by performing [DNS Zone lookup](./api-guide/#dns-zone-lookup).

```
curl -X PUT 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}' \
-H 'Content-Type: application/json' \
--data '{ "zone": { "description": "test" }}'
```

[Field]

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| zone | Object |  | Required |  | DNS Zone |
| zone.description | String | Max. 255 characters | Select |  | DNS Zone description |

#### Response

[Response body]

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


### Deleting DNS Zone (async)

- Deletes multiple DNS Zones along with their record sets.
- Deletion of actual data is processed asynchronously.

#### Request

[URI]

| Method | URI |
|---|---|
| DELETE | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/async |

[Request body]

- Changes {appkey} to the value checked in the console.
-  You can check the DNS Zone ID by performing [DNS Zone lookup](./api-guide/#dns-zone-lookup).

```
curl -X DELETE 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/async?
zoneIdList=bff20a9a-24cf-4670-8b34-007622ec010e,52bc0031-37eb-4b82-b4d7-eaab24188dc4'
```

[Field]

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| zoneIdList | List | Min. 1, Max. 3,000 | Required |  | DNS Zone ID List |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


## Record set API

### Lookup record set

- Looks up the record set list.

#### Request

[URI]

| Method | URI |
|---|---|
| GET | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[Request body]

- Changes {appkey} to the value checked in the console.
- {zoneId} is the DNS Zone ID, which can be checked by performing [DNS Zone lookup](./api-guide/#dns-zone-lookup).

```
curl -X GET 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets'
```

[Options]

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| recordsetIdList | List | Max. 3,000 | Select |  | Record set list |
| recordsetTypeList | List | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, SPF, NS, SOA | Select | | Record set type list |
| searchRecordsetName | String |  | Select |  | Record set name to search for |
| page | int | Min. 1 | Select | 1 | Page No. |
| limit | int | Min. 1, Max. 3,000 | Select | 50 | Lookup count |
| sortDirection | String | DESC, ASC | Select | DESC | Sort order(DESC: Descending, ASC: Ascending) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>RECORDSET_NAME, <br>RECORDSET_TYPE, <br>RECORDSET_TTL | Select | CREATED_AT | Object to sort <br>(CREATED_AT: Created date, <br>UPDATED_AT: Modified date, <br>RECORDSET_NAME: Record set name, <br>RECORDSET_TYPE: Record set type, <br>RECORDSET_TTL: TTL(sec)) |

#### Response

[Response body]

```
{
    "header": {
        // Omitted
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
                    // Omitted: Varies by record set type
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
                    // Omitted: Varies by record set type
                },
                {
                    "recordDisabled": false,
                    "recordContent": "ns.toastdns-jin.net.",
                    // Omitted: Varies by record set type
                }
            ]
        }
    ]
}
```

[Field]

| Name | Type | Description |
|---|---|---|
| totalCount | long | Total record set count |
| recordsetList | List | Record set list |
| recordsetList[0].recordsetId | String | Record set ID |
| recordsetList[0].recordsetName | String | Record set name |
| recordsetList[0].recordsetType | String | Record set type |
| recordsetList[0].recordsetTtl | int | Renew cycle of the record set data in the name server |
| recordsetList[0].recordsetStatus | String | Record set status |
| recordsetList[0].createdAt | DateTime | Created date |
| recordsetList[0].updatedAt | DateTime | Modified date |
| recordsetList[0].recordList | List | Record list |
| recordsetList[0].recordList[0].recordDisabled | boolean | Whether record is disabled or not |
| recordsetList[0].recordList[0].recordContent | String | A record value. It displays the detailed field by record set type in a line. |


### Create Record set

- Creates a record set.
- It supports the following **record set types**: A, AAAA, CAA, CNAME, MX, NAPTR, PTR, TXT, SRV, SPF, NS, and SOA.
- SOA record set cannot be created, modified, or deleted. NS record set cannot be created, modified, or deleted using the **DNS Zone name**.
- The length of the record list within the record set is up to 512 bytes.
- Up to 5,000 record sets can be created per DNS Zone.

#### Request

[URI]

| Method | URI |
|---|---|
| POST | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[Request body]

- Changes {appkey} to the value checked in the console.
- {zoneId} is the DNS Zone ID, which can be checked by performing [DNS Zone lookup](./api-guide/#dns-zone-lookup).
- Record value is required. You can enter the value by selecting either recordset.recordList[0].recordContent field or the detailed field.
- recordContent field displays the detailed field in one line separated by space. You can check the detailed field in [Detailed field by record set type].
- If you enter values in both the detailed field and the recordContent field at the same time, the value in the recordContent field will take priority.

```
curl -X POST 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets' \
-H 'Content-Type: application/json' \
--data '{ "recordset": { "recordsetName": "sub.test.dnsplus.com.", "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }}'
```

[Field]

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| recordset | Object |  | Required |  | Record set |
| recordset.recordsetName | String | Max. 254 characters<br>(DNS Zone name included) | Required |  |  record set name to create, <br>type the domain as [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |
| recordset.recordsetType | String | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, SPF, NS | Required |  | Record set type |
| recordset.recordsetTtl | int | Min. 1, Max. 2147483647 | Required |  | Renew cycle of the record set data in the name server |
| recordset.recordList | List |  | Required |  | Record list |
| recordset.recordList[0].recordDisabled | boolean |  | Select | false | Whether record is disabled or not |
| recordset.recordList[0].recordContent | String |  | Required |  | It displays the detailed field by record set type in a line. |

[Detailed field by record set type]

- A Record set
    - You can enter multiple records.
    - One domain name can be used for multiple IPv4 addresses.

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].ipV4 | String |  | Required |  | IPv4 type address |


- AAAA Record set
    - You can enter multiple records.
    - One domain name can be used for multiple IPv6 addresses.

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].ipV6 | String |  | Required |  | IPv6 type address |


- CAA Record set
    - You can enter multiple records.
    - Assign an authorized certificate authority (CA) to the domain to prevent any unauthorized certificate authority (CA) from issuing the certificate.
    - The issue tag is a certificate issuance permission for domain or sub-domain.
    - The issuewild tag is a wildcard certificate issuance permission for domain or sub-domain.
        - You can set the issue tag in the same way as the issuewild tag.
        - Allow the issuance of a certificate: Type the address of the certificate authority. If additional settings are required, use semicolon (;) as a separator and pair them in the following format: 'name=value'.
        - Do not allow the issuance of a certificate: Type a semicolon (;)
    - When an invalid request has been received to a certificate authority (CA), the iodef tag notifies via a specified e-mail or URL address.
        - Mail address input format: "mailto:*email-address*"
        - URL address input format: "http://*URL*" or "https://*URL*"
    - Custom tag can be used when the certificate authority (CA) supports additional features outside the RFC standard.

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].flags | int | 0 or 128 | Required |  | 0 for a defined tag, <br>128 for a custom tag |
| recordset.recordList[0].tag | String | TAG_ISSUE, <br>TAG_ISSUEWILD, <br>TAG_IODEF, <br>up to 15 custom tags | Required |  | TAG_ISSUE: issue tag, <br>TAG_ISSUEWILD: issuewild tag, <br>TAG_IODEF: iodef tag, <br>custom tag |
| recordset.recordList[0].stringValue | String | Max. 512 characters (including quotation marks) | Required |  | Tag-dependent contents |


- CNAME record set
    - You can enter one record.
    - Define the record set name as an alias of a canonical name (canonical).
    - CNAME record set can be created, if there is no other record set type for a same record set name.

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | Max. 255 characters | Required |  | Type the domain as [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |


- MX record set
    - You can enter multiple records.
    - Specify the mail server for the domain.

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].priority | int | Min. 0, Max. 65535 | Required |  | Priority |
| recordset.recordList[0].domainName | String | Max. 255 characters | Required |  | Type the domain as [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |


- NAPTR Record set
    - You can enter multiple records.
    - In the Dynamic Delegation Discovery System (DDDS) application, it is used to convert or replace one value with another.
    - Order is the order of records evaluation by the DDDS application.
    - Preferred order is the order of records evaluation when there are more than two records with exactly the same order.
    - Separator, which is set in the DDDS application, uses space, 'S', 'A', 'U', and 'P'; the other characters are reserved for something else.
    - Service is set in the DDDS application. For more detailed definition, see the RFC document.
        - URI DDDS Application [RFC 3404#section-4.4](https://tools.ietf.org/html/rfc3404#section-4.4)
        - S-NAPTR DDDS Application [RFC 3958#section-6.5](https://tools.ietf.org/html/rfc3958#section-6.5)
        - U-NAPTR DDDS Application [RFC 4848#section-4.5](https://tools.ietf.org/html/rfc4848#section-4.5)
    - Regexp is used to convert an input value to an output value in the DDDS application. For detailed definition, see [RFC 3402#section-3.2](https://tools.ietf.org/html/rfc3402#section-3.2).
    - Replacement replaces an input value with the name of a domain where the DDDS application will submit DNS queries. Set this as '.' in the case regexp is set.

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].order | int | Min. 0, Max. 65535 | Required |  | Order |
| recordset.recordList[0].preference | int | Min. 0, Max. 65535 | Required |  | Preference |
| recordset.recordList[0].flags | String | Max. 3 characters (including quotation marks) | Required |  | Classification |
| recordset.recordList[0].service | String | Max. 257 characters (including quotation marks) | Required |  | Service |
| recordset.recordList[0].regexp | String | Max. 257 characters (including quotation marks) | Required |  | regexp |
| recordset.recordList[0].replacement | String | Max. 255 characters | Required |  | Enter '.' as a replacement value or type the domain as [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |


- PTR record set
    - You can enter multiple records.
    - This is a reverse query to lookup the domain information with its IP address. You should set this by requesting to your ISP company.
    - The IP address must be typed in the record set name in the reverse order. (Example) 127.0.0.1, 1.0.0.127.in-addr.arpa

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | Max. 255 characters | Required |  | Type the domain as [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |


- TXT record set
    - You can enter multiple records.
    - Enter the text for the record set name.

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].stringValue | String | Max. 255 bytes (quotation marks included) | Required |  | Text |


- SRV record set
    - You can enter multiple records.
    - Find multiple servers which provide similar TCP/IP-based services with a single DNS query operation.

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].priority | int | Min. 0, Max. 65535 | Required |  | Priority |
| recordset.recordList[0].weight | int | Min. 0, Max. 65535 | Required |  | Weight |
| recordset.recordList[0].port | int | Min. 0, Max. 65535 | Required |  | Port |
| recordset.recordList[0].domainName | String | Max. 255 characters | Required |  | Type the domain as [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |


- SPF record set
    - You can enter multiple records.
    - Verify whether the incoming email server has the same email address as the outgoing mail server by authenticating the e-mail sender domain.
    - Type as follows. For detailed definition, see [RFC4408](https://tools.ietf.org/html/rfc4408).
    - The default value of the qualifier is '+', and you can add IP or domain name depending on the mechanism.
        - Format: "v=spf1 {qualifier}{mechanism}{contents} {modifier}={contents}"
        - qualifier: '+'(Pass), '-'(Fail), '~'(Soft Fail), '?'(Neutral)
        - mechanism: all, include, a, mx, prt, ip4, ip6, exists
        - modifier: redirect, exp, custom
        - (Example)
            - "v=spf1 mx -all"
            - "v=spf1 ip4:192.168.0.1/16 -all"
            - "v=spf1 a:toast.com -all"
            - "v=spf1 redirect=toast.com"

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].stringValue | String | Max. 255 bytes (quotation marks included) | Required |  | Contents according to the SPF format |


- NS Record set
    - You can enter multiple records.
    - Enter the name server for the record set name.
    - The record set name can be created or modified only with the sub domain of the DNS Zone name.

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | Max. 255 characters | Required |  | Type the domain as [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |


#### Response

[Response body]

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


### Modify record set

- Modify a record set.
- **Record set name** and **record set type** cannot be modified. However, **TTL(sec)** and **record value** can be modified.
- SOA record set cannot be created, modified, or deleted. NS record set cannot be created, modified, or deleted using the **DNS Zone name**.
- The length of the record list within the record set is up to 512 bytes.

#### Request

[URI]

| Method | URI |
|---|---|
| PUT | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/{recordsetId} |

[Request body]

- Changes {appkey} to the value checked in the console.
- {zoneId} is the DNS Zone ID, which can be checked by performing [DNS Zone lookup](./api-guide/#dns-zone-lookup).
- {recordsetId} is the record set ID and you can check this by performing [Lookup record set](./api-guide/#lookup-record-set).
- Record value is required. You can enter the value by selecting either recordset.recordList[0].recordContent field or the detailed field.
- recordContent field displays the detailed field in one line separated by space. You can check the detailed field in the [Detailed field by record set type] [Create record set](./api-guide/#create-record-set).
- If you enter values in both the detailed field and the recordContent field, the recordContent field will take priority.

```
curl -X POST 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/{recordsetId}' \
-H 'Content-Type: application/json' \
--data '{ "recordset": { "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }}'
```

[Field]

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| recordset | Object |  | Required |  | Record set |
| recordset.recordsetType | String | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, SPF, NS | Required |  | Record set type for the record set ID |
| recordset.recordsetTtl | int | Min. 1, Max. 2147483647 | Required |  | Renew cycle of the record set data in the name server |
| recordset.recordList | List |  | Required |  | Record list |
| recordset.recordList[0].recordDisabled | boolean |  | Required |  | Whether record is disabled or not |
| recordset.recordList[0].recordContent | String |  | Required |  | It displays the detailed field by record set type in a line. |


#### Response

[Response body]

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


### Delete record set

- Delete multiple record sets along with the records in the record sets.
- SOA record set cannot be created, modified, or deleted. NS record set cannot be created, modified, or deleted using the **DNS Zone name**.

#### Request

[URI]

| Method | URI |
|---|---|
| DELETE | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[Request body]

- Changes {appkey} to the value checked in the console.
- {zoneId} is the DNS Zone ID, which can be checked by performing [DNS Zone lookup](./api-guide/#dns-zone-lookup).
- You can check the record set ID by performing [Lookup record set](./api-guide/#lookup-record-set).

```
curl -X DELETE 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets?
recordsetIdList=edb9512b-6e62-409c-99ee-092d340e0adf,edb9512b-6e62-409c-99ee-092d340e0adf'
```

[Field]

| Name | Type | Valid range | Required? | Default | Description |
|---|---|---|---|---|---|
| recordsetIdList | List | Min. 1, Max. 3,000 | Required |  | Record set ID list |

#### Response

[Response body]

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
curl -X PUT 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId}' \
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
