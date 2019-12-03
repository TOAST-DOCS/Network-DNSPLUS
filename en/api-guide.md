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

