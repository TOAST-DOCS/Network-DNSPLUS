
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
        "resultMessage": "Success"
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
        "resultMessage": "Success"
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
        "resultMessage": "Success"
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
        "resultMessage": "Success"
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
| recordsetList[0].recordList[0].recordContent | String | レコード値。レコードセットタイプによって複数のフィールドを1行で表示した内容 |


### レコードセット作成

- レコードセットを作成します。
- **レコードセットタイプ**としてA、AAAA、CAA、CNAME、MX、NAPTR、PTR、TXT、SRV、SPF、NS、SOAをサポートします。
- SOAレコードセットは、作成/修正/削除できず、NSレコードセットは、**DNS Zone名**で作成/修正/削除できません。
- レコードセット内のレコードリストの長さは、最大512バイトです。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| POST | https://api-dnsplus.cloud.toast.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- {zoneId}はDNS Zone IDで、[DNS Zone照会](./api-guide/#dns-zone)を通して確認できます。
- recordset.recordList[0].recordContentの代わりに、レコードセットタイプによってフィールドを詳細に分けて入力できます。
- 詳細フィールドとrecordContentを同時に入力すると、recordContentを基準に作成されます。

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
| recordset.recordList[0].recordContent | String |  | 必須 |  | レコードセットタイプによって複数のフィールドを1行で表示した内容 |

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
    - Aレコードを先に設定してから、CNAMEを設定する必要があります。

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
        "resultMessage": "Success"
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
- recordset.recordList[0].recordContentの代わりに、レコードセットタイプによってフィールドを詳細に分けて入力できます。
- 詳細フィールドとrecordContentを同時に入力した場合、recordContentを基準に修正されます。
- 詳細フィールドは[レコードセット作成](./api-guide/#_14)と同じです。

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
| recordset.recordList[0].recordContent | String |  | 必須 |  | レコードセットタイプによって複数のフィールドを1行で表示した内容 |


#### レスポンス

[レスポンス本文]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "Success"
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
        "resultMessage": "Success"
    }
}
```
