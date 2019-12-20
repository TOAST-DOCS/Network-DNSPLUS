## Network > DNS Plus > APIガイド

DNS PlusサービスのAPIを説明します。


## API共通情報

### 事前準備

- APIを使用するにはアプリケーションキーが必要です。
- アプリケーションキーは、コンソールの下にある**URL & Appkey**メニューで確認できます。

### レスポンス共通情報

- すべてのAPIリクエストに'200 OK'でレスポンスします。詳細なレスポンス結果は、レスポンス本文のヘッダを参照してください。

[成功レスポンス本文]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

[失敗レスポンス本文]

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

### DNS Zone照会

- DNS Zoneリストを照会します。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| GET | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones |

[リクエスト本文]

- {appkey}は、コンソールで確認した値に変更します。

```
curl -X GET 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones'
```

[オプション]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| zoneIdList | List | 最大3,000個 | 任意 |  | DNS Zone IDリスト |
| zoneStatusList | List | CREATING, <br>DELETING, <br>DELETING_FAIL, <br> USE | 任意 | | DNS Zone状態リスト <br>(CREATING：作成中、 <br>DELETING：削除中、<br>DELETING_FAIL：削除失敗、 <br>USE：使用) |
| searchZoneName | String |  | 任意 |  | 検索するDNS Zone名 |
| engineId | String | | 任意 |  | DNSサーバーID |
| page | int | 最小1 | 任意 | 1 | ページ番号 |
| limit | int | 最小1、最大3,000 | 任意 | 50 | 照会数 |
| sortDirection | String | DESC, ASC | 任意 | DESC | ソート方向(DESC：降順、ASC：昇順) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>ZONE_NAME, <br>ZONE_STATUS, <br>RECORDSET_COUNT | 任意 | CREATED_AT | ソート対象 <br>(CREATED_AT：作成日、 <br>UPDATED_AT：修正日、 <br>ZONE_NAME： DNS Zone名、 <br>ZONE_STATUS： DNS Zone状態、 <br>RECORDSET_COUNT：レコードセット数) |

#### レスポンス

[レスポンス本文]

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
            "description": "テスト",
            "createdAt": "2019-06-04T12:32:50.000+09:00",
            "updatedAt": "2019-06-04T12:32:50.000+09:00",
            "recordsetCount": 2
        }
    ]
}
```

[フィールド]

| 名前 | タイプ | 説明 |
|---|---|---|
| totalCount | long | 全DNS Zone数 |
| zoneList | List | DNS Zoneリスト |
| zoneList[0].engineId | boolean | DNSサーバーID |
| zoneList[0].zoneId | String | DNS Zone ID |
| zoneList[0].zoneName | String | DNS Zone名 |
| zoneList[0].zoneStatus | String | DNS Zone状態 |
| zoneList[0].description | String | 説明 |
| zoneList[0].createdAt | DateTime | 作成日 |
| zoneList[0].updatedAt | DateTime | 修正日 |
| zoneList[0].recordsetCount | long | レコードセット数 |


### DNS Zone作成

- DNS Zoneを作成します。
- **DNS Zone名**は、DNSサーバーで唯一のものにする必要があります。
- 同じ**DNS Zone名**は、DNSサーバーの数だけ作成可能です。DNSサーバーは3台です。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| POST | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。

```
curl -X POST 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones' \
-H 'Content-Type: application/json' \
--data '{ "zone": { "zoneName": "test.dnsplus.com.", "description": "test" }}'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| zone | Object |  | 必須 |  | DNS Zone |
| zone.zoneName | String | 最大254文字 | 必須 |  | 作成するDNS Zone名、<br>ドメインを[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)で入力 |
| zone.description | String | 最大255文字 | 任意 |  | DNS Zoneの説明 |

#### レスポンス

[レスポンス本文]

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


### DNS Zone修正

- DNS Zoneを修正します。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| PUT | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId} |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- {zoneId}はDNS Zone IDで、[DNS Zone照会](./api-guide/#dns-zone)を通して確認できます。

```
curl -X PUT 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}' \
-H 'Content-Type: application/json' \
--data '{ "zone": { "description": "test" }}'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| zone | Object |  | 必須 |  | DNS Zone |
| zone.description | String | 最大255文字 | 任意 |  | DNS Zoneの説明 |

#### レスポンス

[レスポンス本文]

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


### DNS Zone削除(非同期)

- 複数のDNS Zoneを削除し、DNS Zoneのレコードセットも一緒に削除します。
- 実際のデータ削除は、非同期で処理されます。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| DELETE | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/async |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- DNS Zone IDは、[DNS Zone照会](./api-guide/#dns-zone)を通して確認できます。

```
curl -X DELETE 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/async?
zoneIdList=bff20a9a-24cf-4670-8b34-007622ec010e,52bc0031-37eb-4b82-b4d7-eaab24188dc4'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| zoneIdList | List | 最小1個、最大3,000個 | 必須 |  | DNS Zone IDリスト |

#### レスポンス

[レスポンス本文]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


## レコードセットAPI

### レコードセット照会

- レコードセットリストを照会します。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| GET | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- {zoneId}はDNS Zone IDで、[DNS Zone照会](./api-guide/#dns-zone)を通して確認できます。

```
curl -X GET 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets'
```

[オプション]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordsetIdList | List | 最大3,000個 | 任意 |  | レコードセットリスト |
| recordsetTypeList | List | A、AAAA、CAA、CNAME、MX、<br>NAPTR、PTR、TXT、SRV、SPF、NS、SOA | 任意 | | レコードセットタイプリスト |
| searchRecordsetName | String |  | 任意 |  | 検索するレコードセット名 |
| page | int | 最小1 | 任意 | 1 | ページ番号 |
| limit | int | 最小1、最大3,000 | 任意 | 50 | 照会数 |
| sortDirection | String | DESC, ASC | 任意 | DESC | ソート方向(DESC：降順、ASC：昇順) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>RECORDSET_NAME, <br>RECORDSET_TYPE, <br>RECORDSET_TTL | 任意 | CREATED_AT | ソート対象<br>(CREATED_AT：作成日、 <br>UPDATED_AT：修正日、 <br>RECORDSET_NAME：レコードセット名、 <br>RECORDSET_TYPE：レコードセットタイプ、 <br>RECORDSET_TTL： TTL(秒)) |

#### レスポンス

[レスポンス本文]

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
                    "recordContent": "ns1.dnsplus.com. hostmaster.dnsplus.com. 2019060401 10800 3600 604800 1200",
                    // 省略：レコードセットタイプによって異なる
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
                    // 省略：レコードセットタイプによって異なる
                },
                {
                    "recordDisabled": false,
                    "recordContent": "ns.toastdns-jin.net.",
                    // 省略：レコードセットタイプによって異なる
                }
            ]
        }
    ]
}
```

[フィールド]

| 名前 | タイプ | 説明 |
|---|---|---|
| totalCount | long | 全レコードセット数 |
| recordsetList | List | レコードセットリスト |
| recordsetList[0].recordsetId | String | レコードセットID |
| recordsetList[0].recordsetName | String | レコードセット名 |
| recordsetList[0].recordsetType | String | レコードセットタイプ |
| recordsetList[0].recordsetTtl | int | ネームサーバーでレコードセット情報の更新周期 |
| recordsetList[0].recordsetStatus | String | レコードセット状態 |
| recordsetList[0].createdAt | DateTime | 作成日 |
| recordsetList[0].updatedAt | DateTime | 修正日 |
| recordsetList[0].recordList | List | レコードリスト |
| recordsetList[0].recordList[0].recordDisabled | boolean | レコードを無効にするかどうか |
| recordsetList[0].recordList[0].recordContent | String | レコード値。レコードセットタイプに応じて詳細フィールドを1行で表示した内容 |


### レコードセット作成

- レコードセットを作成します。
- **レコードセットタイプ**としてA、AAAA、CAA、CNAME、MX、NAPTR、PTR、TXT、SRV、SPF、NS、SOAをサポートします。
- SOAレコードセットは、作成/修正/削除できず、NSレコードセットは、**DNS Zone名**で作成/修正/削除できません。
- レコードセット内のレコードリストの長さは、最大512バイトです。
- DNS Zoneつ当たり、レコードセットは最大5,000個まで作成できます。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| POST | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- {zoneId}はDNS Zone IDで、[DNS Zone照会](./api-guide/#dns-zone)を通して確認できます。
- レコード値は必須で、入力方法にrecordset.recordList[0].recordContentフィールドまたは詳細フィールドを選択できます。
- recordContentフィールドは、空白を区切り文字にして詳細フィールドを1行で表示した内容です。詳細フィールドは、[レコードセットタイプ別の詳細フィールド]で確認できます。
- 詳細フィールドとrecordContentフィールドを同時に入力すると、recordContentフィールドを基準に作成されます。

```
curl -X POST 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets' \
-H 'Content-Type: application/json' \
--data '{ "recordset": { "recordsetName": "sub.test.dnsplus.com.", "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }}'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset | Object |  | 必須 |  | レコードセット |
| recordset.recordsetName | String | 最大254文字<br>(DNS Zone名含む) | 必須 |  | 作成するレコードセット名、<br>ドメインを[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)で入力 |
| recordset.recordsetType | String | A、AAAA、CAA、CNAME、MX、<br>NAPTR、PTR、TXT、SRV、SPF、NS | 必須 |  | レコードセットタイプ |
| recordset.recordsetTtl | int | 最小1、最大2147483647 | 必須 |  | ネームサーバーでレコードセット情報の更新周期 |
| recordset.recordList | List |  | 必須 |  | レコードリスト |
| recordset.recordList[0].recordDisabled | boolean |  | 任意 | false | レコードを無効にするかどうか |
| recordset.recordList[0].recordContent | String |  | 必須 |  | レコードセットタイプに応じて詳細フィールドを1行で表示した内容 |

[レコードセットタイプ別の詳細フィールド]

- Aレコードセット
    - 複数のレコードを入力できます。
    - 1つのドメイン名に複数のIPv4アドレスを登録できます。

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].ipV4 | String |  | 必須 |  | IPv4形式のアドレス |


- AAAAレコードセット
    - 複数のレコードを入力できます。
    - 1つのドメイン名に複数のIPv6アドレスを登録できます

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].ipV6 | String |  | 必須 |  | IPv6形式のアドレス |


- CAAレコードセット
    - 複数のレコードを入力できます。
    - ドメインに発行が許可されている認証機関(CA)を指定すると、許可されていない認証機関(CA)が証明書を発行することを防止できます。
    - issueタグは、ドメインまたはサブドメインに対する証明書発行権限です。
    - issuewildタグは、ドメインまたはサブドメインに対するワイルドカード証明書発行権限です。
        - issueタグとissuewildタグの設定方法は同じです。
        - 証明書発行許可：認証機関アドレスの入力、付加設定が必要な場合は、セミコロン(;)で区切って'名前=値'のペアで指定
        - 証明書発行禁止：セミコロン(;)入力
    - iodefタグは、認証機関(CA)が無効なリクエストを受け取った場合、設定されたメールまたはURLに通知します。
        - メール入力形式："mailto:*email-address*"
        - URL入力形式："http://*URL*"または"https://*URL*"
    - ユーザー指定タグは、認証機関(CA)でRFC標準外の付加機能をサポートする場合の設定です。

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].flags | int | 0または128 | 必須 |  | 定義されたタグの場合0、<br>ユーザー指定タグの場合128 |
| recordset.recordList[0].tag | String | TAG_ISSUE, <br>TAG_ISSUEWILD, <br>TAG_IODEF, <br>ユーザー指定タグ最大15 | 必須 |  | TAG_ISSUE：issueタグ、 <br>TAG_ISSUEWILD：issuewildタグ、 <br>TAG_IODEF：iodefタグ、 <br>ユーザー指定タグ |
| recordset.recordList[0].stringValue | String | 最大512文字(引用符号含む) | 必須 |  | タグに応じた内容 |


- CNAMEレコードセット
    - 1つのレコードを入力できます。
    - レコードセット名を正規名のカノニカル(canonical)で定義します。
    - 同じレコードセット名の他のレコードセットタイプがない場合は、CNAMEレコードセットを作成できます。

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | 最大255文字 | 必須 |  | ドメインを[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)で入力 |


- MXレコードセット
    - 複数のレコードを入力できます。
    - ドメインに対するメールサーバーを指定します。

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].priority | int | 最小0、最大65535 | 必須 |  | 優先順位 |
| recordset.recordList[0].domainName | String | 最大255文字 | 必須 |  | ドメインを[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)で入力 |


- NAPTRレコードセット
    - 複数のレコードを入力できます。
    - DDDS(Dynamic Delegation Discovery System)アプリケーションで、1つの値を別の値に変換または代替するために使用します。
    - 順序項目は、DDDSアプリケーションがレコードを評価する順序です。
    - 優先順序項目は、2個以上のレコードの順序項目が同じ場合、優先して評価する順序です。
    - 区分項目は、DDDSアプリケーション設定で空白、'S', 'A', 'U', 'P'を使用でき、それ以外の文字は予約されています。
    - サービス項目はDDDSアプリケーション設定で、詳細定義はRFC文書で確認できます。
        - URI DDDSアプリケーション[RFC 3404#section-4.4](https://tools.ietf.org/html/rfc3404#section-4.4)
        - S-NAPTR DDDSアプリケーション[RFC 3958#section-6.5](https://tools.ietf.org/html/rfc3958#section-6.5)
        - U-NAPTR DDDSアプリケーション[RFC 4848#section-4.5](https://tools.ietf.org/html/rfc4848#section-4.5)
    - 正規表現項目は、DDDSアプリケーションで入力値を出力値に変換するのに使用します。詳細定義は、[RFC 3402#section-3.2](https://tools.ietf.org/html/rfc3402#section-3.2)で確認できます。
    - 代替値項目は、DDDSアプリケーションがDNSクエリーを提出するドメイン名で入力値を代替します。正規表現項目を設定する場合は、'.'に設定します。

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].order | int | 最小0、最大65535 | 必須 |  | 順序 |
| recordset.recordList[0].preference | int | 最小0、最大65535 | 必須 |  | 優先順序 |
| recordset.recordList[0].flags | String | 最大3文字(引用符号含む) | 必須 |  | 区分 |
| recordset.recordList[0].service | String | 最大257文字(引用符号含む) | 必須 |  | サービス |
| recordset.recordList[0].regexp | String | 最大257文字(引用符号含む) | 必須 |  | 正規表現 |
| recordset.recordList[0].replacement | String | 最大255文字 | 必須 |  | 代替値に'.'またはドメインを[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)で入力 |


- PTRレコードセット
    - 複数のレコードを入力できます。
    - IPアドレスを利用してドメイン情報を照会する逆方向クエリ機能です。ISP業者にリクエストして設定する必要があります。
    - IPアドレスは、逆順でレコードセット名に入力する必要があります。(例) 127.0.0.1, 1.0.0.127.in-addr.arpa

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | 最大255文字 | 必須 |  | ドメインを[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)で入力 |


- TXTレコードセット
    - 複数のレコードを入力できます。
    - レコードセット名に対するテキストの内容を入力します。

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].stringValue | String | 最大255バイト(引用符号含む) | 必須 |  | テキスト内容 |


- SRVレコードセット
    - 複数のレコードを入力できます。
    - 類似したTCP/IP基盤サービスを提供する複数のサーバーを、単一DNSクエリー動作を使用して検索できます。

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].priority | int | 最小0、最大65535 | 必須 |  | 優先順位 |
| recordset.recordList[0].weight | int | 最小0、最大65535 | 必須 |  | 重み |
| recordset.recordList[0].port | int | 最小0、最大65535 | 必須 |  | ポート |
| recordset.recordList[0].domainName | String | 最大255文字 | 必須 |  | ドメインを[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)で入力 |


- SPFレコードセット
    - 複数のレコードを入力できます。
    - メール送信者のドメイン認証方式です。受信メールサーバーが送信メールサーバーとメールアドレスが一致しているかを確認する機能です。
    - 下記のような形式で入力でき、詳細定義は[RFC4408](https://tools.ietf.org/html/rfc4408)で確認できます。
    - 修飾子のデフォルト値は'+'で、メカニズムによってIP、ドメイン名などを追加で入力します。
        - 形式："v=spf1 {修飾子}{メカニズム}{内容} {変更者}={内容}"
        - 修飾子: '+'(Pass), '-'(Fail), '~'(Soft Fail), '?'(Neutral)
        - メカニズム：all、include、a、mx、prt、ip4、ip6、exists
        - 変更者：redirect、exp、ユーザー指定
        - (例)
            - "v=spf1 mx -all"
            - "v=spf1 ip4:192.168.0.1/16 -all"
            - "v=spf1 a:toast.com -all"
            - "v=spf1 redirect=toast.com"

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].stringValue | String | 最大255バイト(引用符号含む) | 必須 |  | SPF形式に応じた内容 |


- NSレコードセット
    - 複数のレコードを入力できます。
    - レコードセット名に対するネームサーバーを指定します。
    - レコードセット名はDNS Zone名のサブドメインでのみ、作成や修正ができます。

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | 最大255文字 | 必須 |  | ドメインを[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)で入力 |


#### レスポンス

[レスポンス本文]

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


### レコードセット修正

- レコードセットを修正します。
- **レコードセット名**と**レコードセットタイプ**は修正できず、**TTL(秒)**と**レコード値**は修正できます。
- SOAレコードセットは作成/修正/削除できず、NSレコードセットは**DNS Zone名**で作成/修正/削除できません。
- レコードセット内のレコードリストの長さは、最大512バイトです。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| PUT | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/{recordsetId} |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- {zoneId}はDNS Zone IDで、[DNS Zone照会](./api-guide/#dns-zone)を通して確認できます。
- {recordsetId}はレコードセットIDで、[レコードセット照会](./api-guide/#_11)を通して確認できます。
- レコード値は必須で、入力方法にrecordset.recordList[0].recordContentフィールドまたは詳細フィールドを選択できます。
- recordContentフィールドは、空白を区切り文字にして詳細フィールドを1行で表示した内容です。詳細フィールドは、[レコードセット作成](./api-guide/#_14)に[レコードセットタイプ別の詳細フィールド]で確認できます。
- 詳細フィールドとrecordContentフィールドを同時に入力すると、recordContentフィールドを基準に修正されます。

```
curl -X POST 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/{recordsetId}' \
-H 'Content-Type: application/json' \
--data '{ "recordset": { "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }}'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset | Object |  | 必須 |  | レコードセット |
| recordset.recordsetType | String | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, SPF, NS | 必須 |  | レコードセットIDに対するレコードセットタイプ |
| recordset.recordsetTtl | int | 最小1、最大2147483647 | 必須 |  | ネームサーバーでレコードセット情報の更新周期 |
| recordset.recordList | List |  | 必須 |  | レコードリスト |
| recordset.recordList[0].recordDisabled | boolean |  | 必須 |  | レコードを無効にするかどうか |
| recordset.recordList[0].recordContent | String |  | 必須 |  | レコードセットタイプに応じて詳細フィールドを1行で表示した内容 |


#### レスポンス

[レスポンス本文]

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


### レコードセット削除

- 複数のレコードセットを削除し、レコードセットのレコードも一緒に削除します。
- SOAレコードセットは作成/修正/削除できず、NSレコードセットは**DNS Zone名**で作成/修正/削除できません。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| DELETE | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- {zoneId}はDNS Zone IDで、[DNS Zone照会](./api-guide/#dns-zone)を通して確認できます。
- レコードセットIDは、[レコードセット照会](./api-guide/#_11)を通して確認できます。

```
curl -X DELETE 'https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets?
recordsetIdList=edb9512b-6e62-409c-99ee-092d340e0adf,edb9512b-6e62-409c-99ee-092d340e0adf'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordsetIdList | List | 最小1個、最大3,000個 | 必須 |  | レコードセットIDリスト |

#### レスポンス

[レスポンス本文]

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
