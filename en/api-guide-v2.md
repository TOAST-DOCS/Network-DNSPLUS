## Network > DNS Plus > API v2.0 Guide

The guide describes API v2.0 of the DNS Plus service.


## API Common Information

### Authentication and Authorization

DNS Plus API v2.0 supports Appkey and User Access Key tokens for API call authentication and authorization.

An Appkey is a unique authentication key issued for each NHN Cloud service, used to identify the service and validate API requests. The User Access Key token is a temporary, Bearer-type access token issued from a User Access Key.
For more information on how to check and use each authentication method, see [Appkey](/nhncloud/en/public-api/appkey/) and [User Access Key Token](/nhncloud/en/public-api/user-access-key-token).

The issued token must be included in the request header.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| X-NHN-AUTHORIZATION | Header | String | O | Bearer type token issued by the Public API |

### Response Common Information

- "200 OK" is returned for all API requests. For details on response results, refer to the header of each response.

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

### List DNS Zones

- Retrieves a list of DNS zones.

#### Request

[URI]

| Method | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/zones |

[Request Body]

- Replace {appkey} with the value found in the console.

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/zones'
```

[Options]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| zoneIdList | List | Up to 3,000 | Optional | | DNS zone ID list |
| zoneStatusList | List | CREATING, <br>DELETING, <br>DELETING_FAIL, <br>USE | Optional | | DNS zone status list <br>(CREATING: Creating, <br>DELETING: Deleting, <br>DELETING_FAIL: Deletion failed, <br>USE: In use) |
| searchZoneName | String | | Optional | | DNS zone name to search |
| engineId | String | | Optional | | DNS server ID |
| page | int | Minimum 1 | Optional | 1 | Page number |
| limit | int | Minimum 1, maximum 3,000 | Optional | 50 | Number of items to retrieve |
| sortDirection | String | DESC, ASC | Optional | DESC | Sort direction (DESC: Descending, ASC: Ascending) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>ZONE_NAME, <br>ZONE_STATUS, <br>RECORDSET_COUNT | Optional | CREATED_AT | Sort key <br>(CREATED_AT: Creation date, <br>UPDATED_AT: Last modified date, <br>ZONE_NAME: DNS zone name, <br>ZONE_STATUS: DNS zone status, <br>RECORDSET_COUNT: Record set count) |

#### Response

[Response Body]

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

[Fields]

| Name | Type | Description |
|---|---|---|
| totalCount | long | Total number of DNS zones |
| zoneList | List | DNS zone list |
| zoneList[0].engineId | String | DNS server ID |
| zoneList[0].zoneId | String | DNS zone ID |
| zoneList[0].zoneName | String | DNS zone name |
| zoneList[0].zoneStatus | String | DNS zone status |
| zoneList[0].description | String | Description |
| zoneList[0].createdAt | DateTime | Creation date |
| zoneList[0].updatedAt | DateTime | Last modified date |
| zoneList[0].recordsetCount | long | Record set count |


### Create DNS Zone

- Creates a DNS zone.
- The **DNS zone name** must be unique on the DNS server.
- The same **DNS zone name** can be created as many times as there are DNS servers. There are 3 DNS servers.

#### Request

[URI]

| Method | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/zones |

[Request Body]

- Replace {appkey} with the value found in the console.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/zones' \
-H 'Content-Type: application/json' \
--data '{ "zone": { "zoneName": "test.dnsplus.com.", "description": "test" }}'
```

[Fields]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| zone | Object | | Required | | DNS zone |
| zone.zoneName | String | Max 254 characters<br>Lowercase letters, numbers, '.', '-', '_'<br>Last character must be '.' | Required | | Name of the DNS zone to create;<br>enter the domain as an [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |
| zone.description | String | Max 255 characters | Optional | | DNS zone description |

#### Response

[Response Body]

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

---

### Modify DNS Zone

- Modifies a DNS zone.

#### Request

[URI]

| Method | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/zones/{zoneId} |

[Request Body]

- Replace {appkey} with the value found in the console.
- {zoneId} is the DNS zone ID, which can be found in [Query DNS Zones](./api-guide/#dns-zone).

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/zones/{zoneId}' \
-H 'Content-Type: application/json' \
--data '{ "zone": { "description": "test" }}'
```

[Fields]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| zone | Object | | Required | | DNS zone |
| zone.description | String | Max 255 characters | Optional | | DNS zone description |

#### Response

[Response Body]

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


### Delete DNS Zone (async)

- Deletes multiple DNS Zones along with their record sets.
- Actual deletion of data is processed asynchronously.

#### Request

[URI]

| Method | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/zones/async |

[Request Body]

- Change {appkey} to the value found in the console.
- DNS Zone ID can be found by [Query DNS Zone](./api-guide/#dns-zone).

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/zones/async?
zoneIdList=bff20a9a-24cf-4670-8b34-007622ec010e,52bc0031-37eb-4b82-b4d7-eaab24188dc4'
```

[Field]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| zoneIdList | List | Minimum 1, maximum 3,000 | Required | | DNS zone ID list |

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


## Record Set API

### Query Record Set

- Retrieves the list of record sets.

#### Request

[URI]

| Method | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[Request Body]

- Change {appkey} to the value found in the console.
- {zoneId} is the DNS zone ID, which can be found in [Query DNS Zones](./api-guide/#dns-zone).

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/zones/{zoneId}/recordsets'
```

[Options]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| recordsetIdList | List | Up to 3,000 | Optional | | Record set list |
| recordsetTypeList | List | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, NS, SOA | Optional | | Record set type list |
| searchRecordsetName | String | | Optional | | Record set name to search |
| page | int | Minimum 1 | Optional | 1 | Page number |
| limit | int | Minimum 1, maximum 3,000 | Optional | 50 | Number of items to retrieve |
| sortDirection | String | DESC, ASC | Optional | DESC | Sort direction (DESC: Descending, ASC: Ascending) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>RECORDSET_NAME, <br>RECORDSET_TYPE, <br>RECORDSET_TTL | Optional | CREATED_AT | Sort key <br>(CREATED_AT: Creation date, <br>UPDATED_AT: Last modified date, <br>RECORDSET_NAME: Record set name, <br>RECORDSET_TYPE: Record set type, <br>RECORDSET_TTL: TTL (seconds)) |

#### Response

[Response Body]

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
| totalCount | long | Total number of record sets |
| recordsetList | List | Record set list |
| recordsetList[0].recordsetId | String | Record set ID |
| recordsetList[0].recordsetName | String | Record set name |
| recordsetList[0].recordsetType | String | Record set type |
| recordsetList[0].recordsetTtl | int | Update interval for record set information on the name server |
| recordsetList[0].recordsetStatus | String | Record set status |
| recordsetList[0].createdAt | DateTime | Creation date |
| recordsetList[0].updatedAt | DateTime | Last modified date |
| recordsetList[0].recordList | List | Record list |
| recordsetList[0].recordList[0].recordDisabled | boolean | Whether record is disabled or not |
| recordsetList[0].recordList[0].recordContent | String | Record value displayed in a single line, as described in Detailed field by record set type |


### Create Record Set

- Creates a record set.
- The following **record set types** are supported: A, AAAA, CAA, CNAME, MX, NAPTR, PTR, TXT, SRV, NS, and SOA.
- SOA record sets cannot be created, modified, or deleted. NS record sets cannot be created, modified, or deleted using the **DNS zone name**.
- The maximum length of the record list within a record set is 512 bytes.
  - TXT record sets support up to 4,096 bytes.
- Up to 5,000 record sets can be created per DNS zone.
- The number of record sets that can be created is limited. Contact us if you need to increase the limit. [Contact Us](https://www.nhncloud.com/kr/support/inquiry)

#### Request

[URI]

| Method | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[Request Body]

- Change {appkey} to the value found in the console.
- {zoneId} is the DNS zone ID, which can be found in [Query DNS Zones](./api-guide/#dns-zone).
- Record value is required. You can enter the value by selecting either recordset.recordList[0].recordContent field or the detailed field.
- The recordContent field displays the detailed field in one line separated by space. You can check the detailed field in [Detailed field by record set type].
- If you enter values in both the detailed field and the recordContent field at the same time, the value in the recordContent field will take priority.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/zones/{zoneId}/recordsets' \
-H 'Content-Type: application/json' \
--data '{ "recordset": { "recordsetName": "sub.test.dnsplus.com.", "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }}'
```

[Fields]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset | Object | | Required | | Record set |
| recordset.recordsetName | String | Max 254 characters<br>Lowercase letters, numbers, '.', '-', '_'<br>(including DNS zone name) | Required | | Name of the record set to create;<br>enter the domain as an [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |
| recordset.recordsetType | String | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, NS | Required | | Record set type |
| recordset.recordsetTtl | int | Minimum 10, maximum 2,147,483,647 | Required | | Update interval for record set information on the name server |
| recordset.recordList | List | | Required | | Record list |
| recordset.recordList[0].recordDisabled | boolean | | Optional | false | Whether record is disabled or not |
| recordset.recordList[0].recordContent | String | | Required | | Record value displayed in a single line, as described in Detailed field by record set type |

[Detailed field by record set type]

- A record set
    - Multiple records can be entered.
    - Multiple IPv4 addresses can be registered for a single domain name.

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].ipV4 | String | | Required | | IPv4 format address |


- AAAA record set
    - Multiple records can be entered.
    - Multiple IPv6 addresses can be registered for a single domain name.

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].ipV6 | String | | Required | | IPv6 format address |


- CAA record set
    - Multiple records can be entered.
    - Specifying an authorized certificate authority (CA) for a domain prevents unauthorized CAs from issuing certificates.
    - The issue tag grants permission to issue certificates for the domain or subdomains.
    - The issuewild tag grants permission to issue wildcard certificates for the domain or subdomains.
        - The issue and issuewild tags are configured in the same way.
        - To allow certificate issuance: enter the CA address; if additional settings are required, separate them with a semicolon (;) and specify as 'name=value' pairs.
        - To prohibit certificate issuance: enter a semicolon (;).
    - The iodef tag sends a notification to the configured email or URL address when a CA receives an invalid request.
        - Email format: "mailto:*email-address*"
        - URL format: "http://*URL*" or "https://*URL*"
    - The custom tag is used when a CA supports additional features beyond the RFC standard.

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].flags | int | 0 or 128 | Required | | 0 for defined tags, <br>128 for custom tags |
| recordset.recordList[0].tag | String | TAG_ISSUE, <br>TAG_ISSUEWILD, <br>TAG_IODEF, <br>custom tag max 15 | Required | | TAG_ISSUE: issue tag, <br>TAG_ISSUEWILD: issuewild tag, <br>TAG_IODEF: iodef tag, <br>custom tag |
| recordset.recordList[0].stringValue | String | Max 512 characters (including quotation marks) | Required | | Content based on the tag |


- CNAME record set
    - Only one record can be entered.
    - Defines the record set name as an alias (canonical) for the canonical name.
    - A CNAME record set can be created when no other record set type exists for the same record set name.

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | Max 255 characters | Required | | Enter the domain as an [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |


- MX record set
    - Multiple records can be entered.
    - Specifies the mail server for the domain.

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].priority | int | Minimum 0, maximum 65,535 | Required | | Priority |
| recordset.recordList[0].domainName | String | Max 255 characters | Required | | Enter the domain as an [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |


- NAPTR record set
    - Multiple records can be entered.
    - Used in DDDS (Dynamic Delegation Discovery System) applications to convert or replace one value with another.
    - The order field specifies the order in which the DDDS application evaluates records.
    - The preference field specifies the order of evaluation when two or more records have the same order value.
    - The flags field is a DDDS application setting. Allowed values are a space, 'S', 'A', 'U', and 'P'; other characters are reserved.
    - The service field is a DDDS application setting. Detailed definitions can be found in the following RFC documents:
        - URI DDDS application [RFC 3404#section-4.4](https://tools.ietf.org/html/rfc3404#section-4.4)
        - S-NAPTR DDDS application [RFC 3958#section-6.5](https://tools.ietf.org/html/rfc3958#section-6.5)
        - U-NAPTR DDDS application [RFC 4848#section-4.5](https://tools.ietf.org/html/rfc4848#section-4.5)
    - The regexp field is used by the DDDS application to convert an input value to an output value. Detailed definitions can be found in [RFC 3402#section-3.2](https://tools.ietf.org/html/rfc3402#section-3.2).
    - The replacement field substitutes the input value with the domain name to which the DDDS application will submit a DNS query. Set to '.' when the regexp field is configured.

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].order | int | Minimum 0, maximum 65,535 | Required | | Order |
| recordset.recordList[0].preference | int | Minimum 0, maximum 65,535 | Required | | Preference |
| recordset.recordList[0].flags | String | Max 3 characters (including quotation marks) | Required | | Flags |
| recordset.recordList[0].service | String | Max 257 characters (including quotation marks) | Required | | Service |
| recordset.recordList[0].regexp | String | Max 257 characters (including quotation marks) | Required | | Regular expression |
| recordset.recordList[0].replacement | String | Max 255 characters | Required | | Enter '.' or the domain as an [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) as the replacement value |


- PTR record set
    - Multiple records can be entered.
    - A reverse lookup feature that queries domain information using an IP address. Must be configured by submitting a request to the ISP.
    - The IP address must be entered in reverse order in the record set name. Example: 127.0.0.1 → 1.0.0.127.in-addr.arpa

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | Max 255 characters | Required | | Enter the domain as an [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |


- TXT record set
    - Multiple records can be entered.
    - Enter text content for the record set name.
    - SPF records can be created using the TXT record set type.
        - An email sender domain authentication method that allows the receiving mail server to verify that the sending mail server matches the email address.
        - Can be entered in the following format. Detailed definitions can be found in [RFC4408](https://tools.ietf.org/html/rfc4408).
        - The default value for qualifiers is '+'. Enter additional information such as IP addresses or domain names depending on the mechanism.
            - Format: "v=spf1 {qualifier}{mechanism}{value} {modifier}={value}"
            - Qualifiers: '+'(Pass), '-'(Fail), '~'(Soft Fail), '?'(Neutral)
            - Mechanisms: all, include, a, mx, ptr, ip4, ip6, exists
            - Modifiers: redirect, exp, custom
            - Examples:
                - "v=spf1 mx -all"
                - "v=spf1 ip4:192.168.0.1/16 -all"
                - "v=spf1 a:toast.com -all"
                - "v=spf1 redirect=toast.com"

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].stringValue | String | Max 4,096 bytes (including quotation marks) | Required | | Text content |


- SRV record set
    - Multiple records can be entered.
    - Allows multiple servers providing similar TCP/IP-based services to be located using a single DNS query operation.

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].priority | int | Minimum 0, maximum 65,535 | Required | | Priority |
| recordset.recordList[0].weight | int | Minimum 0, maximum 65,535 | Required | | Weight |
| recordset.recordList[0].port | int | Minimum 0, maximum 65,535 | Required | | Port |
| recordset.recordList[0].domainName | String | Max 255 characters | Required | | Enter the domain as an [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |


- NS record set
    - Multiple records can be entered.
    - Specifies the name server for the record set name.
    - The record set name can only be created or modified as a subdomain of the DNS zone name.

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | Max 255 characters | Required | | Enter the domain as an [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |


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


### Bulk Create Record Sets

- You can create multiple record sets, up to 2,000 sets per request.
- The following **record set types** are supported: A, AAAA, CAA, CNAME, MX, NAPTR, PTR, TXT, SRV, NS, and SOA.
- The SOA record set cannot be created, modified, or deleted. The NS record set cannot be created, modified, or deleted using the **DNS zone name**.
- The maximum length of the record list within the record set is 512 bytes.
  - TXT record sets support up to 4,096 bytes.
- Up to 5,000 record sets can be created per DNS zone.
- The number of record sets to be created is limited, please contact us if you need an extension. [Contact us](https://www.nhncloud.com/kr/support/inquiry)

#### Request

[URI]

| Method | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/zones/{zoneId}/recordsets/list |

[Request Body]

- Change {appkey} to the value found in the console.
- {zoneId} is the DNS zone ID, which can be found in [Query DNS Zones](./api-guide/#dns-zone).
- Record value is required. You can enter the value by selecting either recordset.recordList[0].recordContent field or the detailed field.
- The recordContent field displays the detailed field in one line separated by space. You can check the detailed field in [Detailed field by record set type] from [Create Record Set](./api-guide/#_14).
- If you enter values in both the detailed field and the recordContent field at the same time, the value in the recordContent field will take priority.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/zones/{zoneId}/recordsets/list' \
-H 'Content-Type: application/json' \
--data '{ "recordsetList": [{ "recordsetName": "sub.test.dnsplus.com.", "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }]}'
```

[Fields]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| recordsetList | List | | Required | | Record set list |
| recordsetList[0].recordsetName | String | Max 254 characters<br>Lowercase letters, numbers, '.', '-', '_'<br>(including DNS zone name) | Required | | Name of the record set to create;<br>enter the domain as an [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |
| recordsetList[0].recordsetType | String | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, NS | Required | | Record set type |
| recordsetList[0].recordsetTtl | int | Minimum 10, maximum 2,147,483,647 | Required | | Update interval for record set information on the name server |
| recordsetList[0].recordList | List | | Required | | Record list |
| recordsetList[0].recordList[0].recordDisabled | boolean | | Optional | false | Whether record is disabled or not |
| recordsetList[0].recordList[0].recordContent | String | | Required | | Record value displayed in a single line, as described in Detailed field by record set type |

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


### Modify Record Set

- Modifies a record set.
- The **record set name** cannot be modified. The **record set type**, **TTL (seconds)**, and **record value** can be modified.
- The SOA record set cannot be created, modified, or deleted. The NS record set cannot be created, modified, or deleted using the **DNS zone name**.
- The maximum length of the record list within the record set is 512 bytes.
  - TXT record sets support up to 4,096 bytes.

#### Request

[URI]

| Method | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/zones/{zoneId}/recordsets/{recordsetId} |

[Request Body]

- Replace {appkey} with the value found in the console.
- {zoneId} is the DNS zone ID, which can be found in [List DNS Zones](./api-guide/#dns-zone).
- {recordsetId} is the record set ID, which can be found in [List Record Sets](./api-guide/#_11).
- Record value is required. You can enter the value by selecting either the recordset.recordList[0].recordContent field or the detailed field.
- The recordContent field displays the detailed fields in a single line, separated by spaces. The detailed fields can be found in [Detailed field by record set type] in [Create Record Set](./api-guide/#_14).
- If both the detailed field and recordContent field are entered simultaneously, the recordContent field takes precedence.

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/zones/{zoneId}/recordsets/{recordsetId}' \
-H 'Content-Type: application/json' \
--data '{ "recordset": { "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }}'
```

[Fields]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset | Object | | Required | | Record set |
| recordset.recordsetType | String | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, NS | Required | | Record set type |
| recordset.recordsetTtl | int | Minimum 10, maximum 2,147,483,647 | Required | | Update interval for record set information on the name server |
| recordset.recordList | List | | Required | | Record list |
| recordset.recordList[0].recordDisabled | boolean | | Required | | Whether record is disabled or not |
| recordset.recordList[0].recordContent | String | | Required | | Record value displayed in a single line, as described in Detailed field by record set type |


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


### Delete Record Set

- Deletes multiple record sets along with the records in the record sets.
- SOA record set cannot be created, modified, or deleted. NS record set cannot be created, modified, or deleted using the **DNS Zone name**.

#### Reqest

[URI]

| Method | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[Request Body]

- Change {appkey} to the value found in the console.
- {zoneId} is the DNS zone ID, which can be found in [Query DNS Zones](./api-guide/#dns-zone).
- You can check the record set ID by performing [Query Record Set](./api-guide/#_11).

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/zones/{zoneId}/recordsets?
recordsetIdList=edb9512b-6e62-409c-99ee-092d340e0adf,edb9512b-6e62-409c-99ee-092d340e0adf'
```

[Fields]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| recordsetIdList | List | Minimum 1, maximum 3,000 | Required | | Record set ID list |

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

### Retrieve GSLB

- Retrieves the list of GSLBs.
- When a health check is connected to pools, you can check the health status of GSLB, pools, and endpoints.

#### Request

[URI]

| Method | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/gslbs |

[Request Body]

- Change {appkey} to the value found in the console.

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/gslbs?showHealthy=true'
```

[Options]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| gslbIdList | List | Up to 3,000 | Optional | | GSLB ID list |
| searchGslbName | String | | Optional | | GSLB name to search |
| gslbDomain | String | | Optional | | GSLB domain |
| showHealthy | boolean | | Optional | | Whether to display health check results |
| page | int | Minimum 1 | Optional | 1 | Page number |
| limit | int | Minimum 1, maximum 3,000 | Optional | 50 | Number of items to retrieve |
| sortDirection | String | DESC, ASC | Optional | DESC | Sort direction (DESC: Descending, ASC: Ascending) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>GSLB_NAME, <br>GSLB_DOMAIN, <br>GSLB_TTL, <br>GSLB_ROUTING_RULE, <br>GSLB_DISABLED | Optional | CREATED_AT | Sort key <br>(CREATED_AT: Creation date, <br>UPDATED_AT: Last modified date, <br>GSLB_NAME: GSLB name, <br>GSLB_DOMAIN: GSLB domain, <br>GSLB_TTL: GSLB domain update interval, <br>GSLB_ROUTING_RULE: Routing rule, <br>GSLB_DISABLED: Whether GSLB is disabled) |

#### Response

[Response body]

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
                        // Omitting Pool information
                    }
                },
                {
                    "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
                    "connectedPoolOrder": 2,
                    "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
                    "pool": {
                        // Omitting Pool information
                    }
                }
            ],
            "createdAt": "2019-12-18T20:44:02.000+09:00",
            "updatedAt": "2019-12-18T21:01:05.000+09:00"
        }
    ]
}
```

[Fields]

| Name | Type | Description |
|---|---|---|
| totalCount | long | Total number of GSLBs |
| gslbList | List | GSLB list |
| gslbList[0].gslbId | String | GSLB ID |
| gslbList[0].gslbName | String | GSLB name |
| gslbList[0].gslbDomain | String | GSLB domain |
| gslbList[0].gslbTtl | String | GSLB domain update interval |
| gslbList[0].gslbRoutingRule | String | Routing rule |
| gslbList[0].gslbDisabled | boolean | Whether GSLB is disabled |
| gslbList[0].healthy | boolean | Whether GSLB is healthy |
| gslbList[0].connectedPoolList | List | Connected pool list |
| gslbList[0].connectedPoolList[0].poolId | String | Connected pool ID |
| gslbList[0].connectedPoolList[0].pool | Object | Connected pool information |
| gslbList[0].connectedPoolList[0].connectedPoolOrder | int | Connected pool priority |
| gslbList[0].connectedPoolList[0].connectedPoolRegionContent | String | Connected pool region displayed in a single line |
| gslbList[0].createdAt | DateTime | Creation date |
| gslbList[0].updatedAt | DateTime | Last modified date |


### Create GSLB

- Creates GSLB and pool connection settings.
- The **routing rule** is the load balancing method for the GSLB domain. FAILOVER, RANDOM, and GEOLOCATION are available.
    - FAILOVER: Routes based on the priority of the connected pool.
    - RANDOM: Routes by randomly selecting an available pool from the connected pools.
    - GEOLOCATION: Routes traffic from the configured region to the corresponding connected pool. If no region is configured, routing is based on priority.
- The lower the **priority** of a **connected pool**, the higher the routing order. Duplicate priorities are not allowed.
- There are limits to the maximum number of GSLBs that can be created and to the maximum number of pools that can be connected. If you want to raise the limits, please contact us. [Contact us](https://www.nhncloud.com/kr/support/inquiry)

#### Request

[URI]

| Method | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/gslbs |

[Request Body]

- Change {appkey} to the value found in the console.
- For the connectedPoolRegionContent field, enter **regions** in one line with commas (,) as delimiters.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/gslbs' \
-H 'Content-Type: application/json' \
--data '{ "gslb": { "gslbName": "GSLB-test", "gslbTtl": 300, "gslbRoutingRule": "FAILOVER", "connectedPoolList": [ { "poolId": "8e4326d4-3862-4b46-819e-83a786add570", "connectedPoolOrder": 1 }, { "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818", "connectedPoolOrder": 2 } ] }}'
```

[Fields]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| gslb | Object | | Required | | GSLB |
| gslb.gslbName | String | Max 100 characters,<br>uppercase and lowercase letters, numbers, '-', '_' | Required | | GSLB name |
| gslb.gslbTtl | int | | Required | | GSLB domain update interval |
| gslb.gslbRoutingRule | String | FAILOVER, RANDOM, GEOLOCATION | Required | | Routing rule |
| gslb.gslbDisabled | boolean | | Optional | false | Whether GSLB is disabled |
| gslb.connectedPoolList | List | | Optional | | Connected pool list |
| gslb.connectedPoolList[0].poolId | String | | Required | | Connected pool ID |
| gslb.connectedPoolList[0].connectedPoolOrder | int | Minimum 1, maximum 2,147,483,647 | Required | | Connected pool priority |
| gslb.connectedPoolList[0].connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | Optional | | Connected pool region settings |

#### Response

[Response body]

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
                    // Omitting Pool information
                }
            },
            {
                "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
                "connectedPoolOrder": 2,
                "pool": {
                    // Omitting Pool information
                }
            }
        ],
        "createdAt": "2019-12-18T20:44:02.000+09:00",
        "updatedAt": "2019-12-18T20:44:03.000+09:00"
    }
}
```


### GSLB 수정

- Updates GSLB and pool connection settings.
- Updates the items entered in [Create GSLB](./api-guide/#gslb_1).

#### Request

[URI]

| Method | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/gslbs/{gslbId} |

[Request Body]

- Change {appkey} to the value found in the console.
- {gslbId} is a GSLB ID and can be found by [Retrieve GSLB](./api-guide/#gslb).
- For the connectedPoolRegionContent field, enter **regions** in one line with commas (,) as delimiters.

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/gslbs/{gslbId}' \
-H 'Content-Type: application/json' \
--data '{ "gslb": { "gslbName": "GSLB-test", "gslbTtl": 300, "gslbDisabled": true, "gslbRoutingRule": "GEOLOCATION", "connectedPoolList": [ { "poolId": "8e4326d4-3862-4b46-819e-83a786add570", "connectedPoolOrder": 1 }, { "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818", "connectedPoolOrder": 2, "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA" } ] }}'
```

[Fields]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| gslb | Object | | Required | | GSLB |
| gslb.gslbName | String | Max 100 characters,<br>uppercase and lowercase letters, numbers, '-', '_' | Required | | GSLB name |
| gslb.gslbTtl | int | | Required | | GSLB domain update interval |
| gslb.gslbRoutingRule | String | FAILOVER, RANDOM, GEOLOCATION | Required | | Routing rule |
| gslb.gslbDisabled | boolean | | Optional | false | Whether GSLB is disabled |
| gslb.connectedPoolList | List | | Optional | | Connected pool list |
| gslb.connectedPoolList[0].poolId | String | | Required | | Connected pool ID |
| gslb.connectedPoolList[0].connectedPoolOrder | int | Minimum 1, maximum 2,147,483,647 | Required | | Connected pool priority |
| gslb.connectedPoolList[0].connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | Optional | | Connected pool region settings |

#### Response

[Response body]

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
                    // Omitting Pool information
                }
            },
            {
                "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
                "connectedPoolOrder": 2,
                "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
                "pool": {
                    // Omitting Pool information
                }
            }
        ],
        "createdAt": "2019-12-18T20:44:02.000+09:00",
        "updatedAt": "2019-12-18T20:59:49.000+09:00"
    }
}
```


### Delete GSLB

- Deletes multiple GSLBs.

#### Request

[URI]

| Method | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/gslbs |

[Request Body]

- Change {appkey} to the value found in the console.

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/gslbs?
gslbIdList=91de0c6f-aeaa-44ec-b361-822acfcd5921,269eff10-f3c0-4b11-b072-ec53e7c604bf'
```

[Fields]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| gslbIdList | List | Minimum 1, maximum 3,000 | Required | | GSLB ID list |

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


### Connect Pool

- Connects a pool to a GSLB.
- The lower the **priority** of a **connected pool**, the higher the routing order. If the same priority as an existing connected pool is entered, the routing order of the existing pool is lowered.
- There is a limit to the maximum number of pools that can be connected. If you want to raise the limit, contact us. [Contact Us](https://www.nhncloud.com/kr/support/inquiry)

#### Request

[URI]

| Method | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId} |

[Request Body]

- Replace {appkey} with the value found in the console.
- {gslbId} is the GSLB ID, which can be found in [List GSLBs](./api-guide/#gslb).
- {poolId} is the pool ID, which can be found in [List Pools](./api-guide/#pool_3).
- For the connectedPoolRegionContent field, enter **regions** in one line with commas (,) as delimiters.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId}' \
-H 'Content-Type: application/json' \
--data '{ "connectedPool": { "connectedPoolOrder": 1 } }'
```

[Fields]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| connectedPool | Object | | Required | | Connected pool |
| connectedPool.connectedPoolOrder | int | Minimum 1, maximum 2,147,483,647 | Required | | Connected pool priority |
| connectedPool.connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | Optional | | Connected pool region settings |

#### Response

[Response body]

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
                // Omitting Pool information
            }
        },
        {
            "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
            "connectedPoolOrder": 2,
            "pool": {
                // Omitting Pool information
            }
        },
        {
            "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
            "connectedPoolOrder": 3,
            "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
            "pool": {
                // Omitting Pool information
            }
        }
    ]
}
```

### Modify Pool Connection

- Updates the settings of a pool connected to GSLB.
- Modifies the pool settings entered in [Create GSLB](./api-guide/#gslb_1) or [Connect Pool](./api-guide/#pool).

#### Request

[URI]

| Method | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId} |

[Request Body]

- Replace {appkey} with the value found in the console.
- {gslbId} is the GSLB ID, which can be found in [List GSLBs](./api-guide/#gslb).
- {poolId} is the pool ID, which can be found in [List Pools](./api-guide/#pool_3).
- For the connectedPoolRegionContent field, enter **regions** in one line with commas (,) as delimiters.

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId}' \
-H 'Content-Type: application/json' \
--data '{ "connectedPool": { "connectedPoolOrder": 1, "connectedPoolRegionContent": "WESTERN_NORTH_AMERICA" } }'
```

[Fields]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| connectedPool | Object | | Required | | Connected pool |
| connectedPool.connectedPoolOrder | int | Minimum 1, maximum 2,147,483,647 | Required | | Connected pool priority |
| connectedPool.connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | Optional | | Connected pool region settings |

#### Response

[Response body]

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
                // Omitting Pool information
            }
        },
        {
            "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
            "connectedPoolOrder": 2,
            "pool": {
                // Omitting Pool information
            }
        },
        {
            "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
            "connectedPoolOrder": 3,
            "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
            "pool": {
                // Omitting Pool information
            }
        }
    ]
}
```

### Detach Pool

- Detaches multiple pools connected to GSLB.

#### Request

[URI]

| Method | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools |

[Request Body]

- Change {appkey} to the value found in the console.
- {gslbId} is the GSLB ID, which can be found in [List GSLBs](./api-guide/#gslb).

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools?
poolIdList=52da0e48-9062-43f7-bef8-8aec4b795bfe,12bc396a-eb97-4a6b-ab4c-73d1a1dfb093'
```

[Fields]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| poolIdList | List | Minimum 1, maximum 3,000 | Required | | Pool ID list |

#### Response

[Response body]

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
                // Omitting Pool information
            }
        },
        {
            "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
            "connectedPoolOrder": 3,
            "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
            "pool": {
                // Omitting Pool information
            }
        }
    ]
}
```


## Pool API

### List Pools

- Retrieves a list of pools.
- If a health check is connected, the health status of the pool and endpoints can be checked.

#### Request

[URI]

| Method | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/pools |

[Request Body]

- Replace {appkey} with the value found in the console.

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/pools?showHealthy=true'
```

[Options]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| poolIdList | List | Up to 3,000 | Optional | | Pool ID list |
| searchPoolName | String | | Optional | | Pool name to search |
| healthCheckId | String | | Optional | | Connected health check ID |
| showHealthy | boolean | | Optional | | Whether to display health check results |
| page | int | Minimum 1 | Optional | 1 | Page number |
| limit | int | Minimum 1, maximum 3,000 | Optional | 50 | Number of items to retrieve |
| sortDirection | String | DESC, ASC | Optional | DESC | Sort direction (DESC: Descending, ASC: Ascending) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>POOL_NAME, <br>POOL_DISABLED, <br>HEALTH_CHECK_ID | Optional | CREATED_AT | Sort key <br>(CREATED_AT: Creation date, <br>UPDATED_AT: Last modified date, <br>POOL_NAME: Pool name, <br>POOL_DISABLED: Whether the pool is disabled, <br>HEALTH_CHECK_ID: Connected health check ID) |

#### Response

[Response body]

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
                // Omitting health check information
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

[Fields]

| Name | Type | Description |
|---|---|---|
| totalCount | long | Total number of pools |
| poolList | List | Pool list |
| poolList[0].poolId | String | Pool ID |
| poolList[0].poolName | String | Pool name |
| poolList[0].poolDisabled | boolean | Whether the pool is disabled |
| poolList[0].healthy | boolean | Whether the pool is healthy |
| poolList[0].healthCheckId | String | Connected health check ID |
| poolList[0].healthCheck | Object | Connected health check information |
| poolList[0].endpointList | List | Endpoint list |
| poolList[0].endpointList[0].endpointAddress | String | Endpoint address |
| poolList[0].endpointList[0].endpointWeight | double | Endpoint weight |
| poolList[0].endpointList[0].healthy | boolean | Whether the endpoint is healthy |
| poolList[0].endpointList[0].failureReason | String | Reason for endpoint failure |
| poolList[0].createdAt | DateTime | Creation date |
| poolList[0].updatedAt | DateTime | Last modified date |


### Create Pool

- Creates a pool and endpoints within the pool.
- A **health check** can be configured to verify the accessibility of endpoints within the pool.
- The **endpoint address** can be entered as a domain address or IPv4, with the following restrictions:
    - Cannot start with a hyphen or period, and cannot end with a hyphen. Periods and hyphens cannot appear consecutively.
    - [Reserved IP addresses](https://en.wikipedia.org/wiki/Reserved_IP_addresses) cannot be entered.
    - Duplicate addresses are not allowed within the pool.
- The **weight** of an endpoint operates relative to the weights of other endpoints in the pool. Endpoints with the same weight have equal priority within the pool.
- The number of pools that can be created, the number of endpoints within a pool, and the total number of endpoints are limited. Contact us if you need to increase the limit. [Contact Us](https://www.nhncloud.com/en/support/inquiry)

#### Request

[URI]

| Method | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/pools |

[Request Body]

- Change {appkey} to the value found in the console.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/pools' \
-H 'Content-Type: application/json' \
--data '{ "pool": { "poolName": "POOL-test", "endpointList": [ { "endpointAddress": "test.dnsplus.com" }, { "endpointAddress": "123.123.123.123" } ] }}'
```

[Fields]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| pool | Object | | Required | | Pool |
| pool.poolName | String | Max 100 characters,<br>uppercase and lowercase letters, numbers, '-', '_' | Required | | Pool name |
| pool.poolDisabled | boolean | | Optional | false | Whether the pool is disabled |
| pool.healthCheckId | String | | Optional | | Health check ID |
| pool.endpointList | List | | Required | | Endpoint list |
| pool.endpointList[0].endpointAddress | String | Max 254 characters,<br>lowercase letters, numbers, '.', '-', '_' | Required | | Endpoint address |
| pool.endpointList[0].endpointWeight | double | Minimum 0, maximum 1.00 | Optional | 1.00 | Endpoint weight |
| pool.endpointList[0].endpointDisabled | boolean | | Optional | false | Whether the endpoint is disabled |

#### Response

[Response body]

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


### Modify Pool

- Modifies a pool and endpoints within the pool.
- Modifies the items entered in [Create Pool](./api-guide/#pool_4).

#### Request

[URI]

| Method | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/pools/{poolId} |

[Request Body]

- Replace {appkey} with the value found in the console.
- {poolId} is the pool ID, which can be found in [List Pools](./api-guide/#pool_3).

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/pools/{poolId}' \
-H 'Content-Type: application/json' \
--data '{ "pool": { "poolName": "POOL-test", "poolDisabled": true, "healthCheckId": "b9165853-7859-4309-8059-48f12ebdbc17", "endpointList": [ { "endpointAddress": "test.dnsplus.com", "endpointWeight": 1.00, "endpointDisabled": true }, { "endpointAddress": "123.123.123.123", "endpointWeight": 0.5, "endpointDisabled": true } ] }}'
```

[Fields]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| pool | Object | | Required | | Pool |
| pool.poolName | String | Max 100 characters,<br>uppercase and lowercase letters, numbers, '-', '_' | Required | | Pool name |
| pool.poolDisabled | boolean | | Optional | false | Whether the pool is disabled |
| pool.healthCheckId | String | | Optional | | Health check ID |
| pool.endpointList | List | | Required | | Endpoint list |
| pool.endpointList[0].endpointAddress | String | Max 254 characters,<br>lowercase letters, numbers, '.', '-', '_' | Required | | Endpoint address |
| pool.endpointList[0].endpointWeight | double | Minimum 0, maximum 1.00 | Optional | 1.00 | Endpoint weight |
| pool.endpointList[0].endpointDisabled | boolean | | Optional | false | Whether the endpoint is disabled |

#### Response

[Response body]

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
            // Omitting health check information
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


### Delete Pools

- Deletes multiple pools, including the endpoints within each pool.
- Pools connected to a GSLB cannot be deleted.

#### Request

[URI]

| Method | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/pools |

[Request Body]

- Replace {appkey} with the value found in the console.

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/pools?
poolIdList=8e4326d4-3862-4b46-819e-83a786add570,2f89d3fe-03bc-4711-826e-db2c89c12818'
```

[Fields]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| poolIdList | List | Minimum 1, maximum 3,000 | Required | | Pool ID list |

#### Response

[Response Body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


## Health Check API

### List Health Checks

- Retrieves a list of health checks.

#### Request

[URI]

| Method | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/health-checks |

[Request Body]

- Replace {appkey} with the value found in the console.

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/health-checks'
```

[Options]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| healthCheckIdList | List | Up to 3,000 | Optional | | Health check ID list |
| searchHealthCheckName | String | | Optional | | Health check name to search |
| page | int | Minimum 1 | Optional | 1 | Page number |
| limit | int | Minimum 1, maximum 3,000 | Optional | 50 | Number of items to retrieve |
| sortDirection | String | DESC, ASC | Optional | DESC | Sort direction (DESC: Descending, ASC: Ascending) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>HEALTH_CHECK_NAME, <br>PROTOCOL, <br>PORT | Optional | CREATED_AT | Sort key <br>(CREATED_AT: Creation date, <br>UPDATED_AT: Last modified date, <br>HEALTH_CHECK_NAME: Health check name, <br>PROTOCOL: Protocol, <br>PORT: Port) |

#### Response

[Response body]

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
            "interval": 60,
            "timeout": 5,
            "retries": 2,
            "path": "/",
            "expectedCodes": "2xx",
            "expectedBody": "OK",
            "allowInsecure": false,
            "requestHeaderList": [
                { "Host": "nhncloud.com" }
            ],
            "createdAt": "2019-12-18T12:31:34.000+09:00",
            "updatedAt": "2019-12-18T14:19:20.000+09:00"
        }
    ]
}
```

[Fields]

| Name | Type | Description |
|---|---|---|
| totalCount | long | Total number of health checks |
| healthCheckList | List | Health check list |
| healthCheckList[0].healthCheckId | String | Health check ID |
| healthCheckList[0].healthCheckName | String | Health check name |
| healthCheckList[0].protocol | String | Protocol |
| healthCheckList[0].port | int | Port |
| healthCheckList[0].interval | int | Health check interval |
| healthCheckList[0].timeout | int | Maximum response wait time |
| healthCheckList[0].retries | int | Maximum retry count |
| healthCheckList[0].path | String | Path |
| healthCheckList[0].expectedCodes | String | Expected status code |
| healthCheckList[0].expectedBody | String | Expected response body |
| healthCheckList[0].allowInsecure | boolean | Skip certificate verification |
| healthCheckList[0].requestHeaderList | List | Request header list |
| healthCheckList[0].requestHeaderList[0] | Object | Request header name and value object |
| healthCheckList[0].createdAt | DateTime | Creation date |
| healthCheckList[0].updatedAt | DateTime | Last modified date |


### Create Health Check

- Creates a health check.
- For the health check **protocol**, HTTPS, HTTP, and TCP are supported, and the information that can be entered differs depending on the selected protocol.
    - HTTPS input items: Skip certificate verification, port, health check interval, maximum response wait time, maximum retry count, path, expected status code, expected response body, and request header
    - HTTP input items: Port, health check interval, maximum response wait time, maximum retry count, path, expected status code, expected response body, and request header
    - TCP input items: Port, health check interval, maximum response wait time, and maximum retry count
- Enabling **Skip certificate verification** allows the health check to ignore invalid TLS/SSL certificates on endpoints.
- Redirected pages from endpoints are not supported when evaluating **expected status codes** and **expected response bodies**.
- The number of health checks that can be created is limited. Contact us if you need to increase the limit. [Contact Us](https://www.nhncloud.com/kr/support/inquiry)

#### Request

[URI]

| Method | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/health-checks |

[Request Body]

- Replace {appkey} with the value found in the console.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/health-checks' \
-H 'Content-Type: application/json' \
--data '{ "healthCheck": { "healthCheckName": "HTTPS-443", "protocol": "HTTPS", "port": 443, "interval": 60, "timeout": 5, "retries": 2, "path": "/", "expectedCodes": "2xx", "allowInsecure": false, "requestHeaderList": [{ "Host": "nhncloud.com" }] }}'
```

[Fields]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| healthCheck | Object | | Required | | Health check |
| healthCheck.healthCheckName | String | Max 100 characters,<br>uppercase and lowercase letters, numbers, '-', '_' | Required | | Health check name |
| healthCheck.protocol | String | HTTPS, HTTP, TCP | Required | | Health check protocol |
| healthCheck.port | int | Minimum 1, maximum 65,535 | Required | | Health check port |
| healthCheck.interval | int | Minimum 10 or (retries+1)*timeout, maximum 3,600 | Optional | 60 | Health check interval |
| healthCheck.timeout | int | Minimum 1, maximum 10 | Optional | 5 | Maximum response wait time |
| healthCheck.retries | int | Minimum 0, maximum 5 | Optional | 2 | Maximum retry count |
| healthCheck.path | String | Max 254 characters,<br>must start with '/' | Optional | | Health check path;<br>used for HTTPS and HTTP |
| healthCheck.expectedCodes | String | Numbers and wildcard 'x' | Optional | | Expected health check status code;<br>used for HTTPS and HTTP<br>Example: 2xx, 20x, 200 |
| healthCheck.expectedBody | String | Max 10 KB | Optional | | Expected health check response body;<br>used for HTTPS and HTTP |
| healthCheck.allowInsecure | boolean | | Optional | | Skip certificate verification for health check;<br>used for HTTPS |
| healthCheck.requestHeaderList | List | | Optional | | Request header list;<br>used for HTTPS and HTTP;<br>items in the list must be entered in the format `{ "header name": "header value" }` |

#### Response

[Response body]

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
        "interval": 60,
        "timeout": 5,
        "retries": 2,
        "path": "/",
        "expectedCodes": "2xx",
        "allowInsecure": false,
        "requestHeaderList": [
            { "Host": "nhncloud.com" }
        ],
        "createdAt": "2019-12-18T12:31:34.000+09:00",
        "updatedAt": "2019-12-18T12:31:34.000+09:00"
    }
}
```


### Modify Health Check

- Modifies a health check.
- Modifies the items entered in [Create Health Check](./api-guide/#_48).

#### Request

[URI]

| Method | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/health-checks/{healthCheckId} |

[Request Body]

- Replace {appkey} with the value found in the console.
- {healthCheckId} is the health check ID, which can be found in [List Health Checks](./api-guide/#_45).

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/health-checks/{healthCheckId}' \
-H 'Content-Type: application/json' \
--data '{ "healthCheck": { "healthCheckName": "HTTPS-443", "protocol": "HTTPS", "port": 443, "interval": 60, "timeout": 5, "retries": 2, "path": "/", "expectedCodes": "3xx", "allowInsecure": false }}'
```

[Fields]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| healthCheck | Object | | Required | | Health check |
| healthCheck.healthCheckName | String | Max 100 characters,<br>uppercase and lowercase letters, numbers, '-', '_' | Required | | Health check name |
| healthCheck.protocol | String | HTTPS, HTTP, TCP | Required | | Health check protocol |
| healthCheck.port | int | Minimum 1, maximum 65,535 | Required | | Health check port |
| healthCheck.interval | int | Minimum 10 or (retries+1)*timeout, maximum 3,600 | Optional | | Health check interval |
| healthCheck.timeout | int | Minimum 1, maximum 10 | Optional | | Maximum response wait time |
| healthCheck.retries | int | Minimum 0, maximum 5 | Optional | | Maximum retry count |
| healthCheck.path | String | Max 254 characters,<br>must start with '/' | Optional | | Health check path;<br>used for HTTPS and HTTP |
| healthCheck.expectedCodes | String | Numbers and wildcard 'x' | Optional | | Expected health check status code;<br>used for HTTPS and HTTP<br>Example: 2xx, 20x, 200 |
| healthCheck.expectedBody | String | Max 10 KB | Optional | | Expected health check response body;<br>used for HTTPS and HTTP |
| healthCheck.allowInsecure | boolean | | Optional | | Skip certificate verification for health check;<br>used for HTTPS |
| healthCheck.requestHeaderList | List | | Optional | | Request header list;<br>used for HTTPS and HTTP;<br>items in the list must be entered in the format `{ "header name": "header value" }` |

#### Response

[Response Body]

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
        "interval": 60,
        "timeout": 5,
        "retries": 2,
        "path": "/",
        "expectedCodes": "3xx",
        "allowInsecure": false,
        "requestHeaderList": [
            { "Host": "nhncloud.com" }
        ],
        "createdAt": "2019-12-18T12:31:34.000+09:00",
        "updatedAt": "2019-12-18T12:36:20.000+09:00"
    }
}
```


### Delete Health Checks

- Deletes multiple health checks.
- Health checks connected to a pool cannot be deleted.

#### Request

[URI]

| Method | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/health-checks |

[Request Body]

- Replace {appkey} with the value found in the console.

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v2.0/appkeys/{appkey}/health-checks?
healthCheckIdList=b9165853-7859-4309-8059-48f12ebdbc17,d2629d6b-9381-4645-9cf3-43d7ad491e2b'
```

[Fields]

| Name | Type | Valid Range | Required | Default | Description |
|---|---|---|---|---|---|
| healthCheckIdList | List | Minimum 1, maximum 3,000 | Required | | Health check ID list |

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
