<!-- machine_translated: true -->

<!-- pre-align:aligned sig=fe9851c62a0b -->

<a id="network-dns-plus-release-notes"></a>
## Network > DNS Plus > リリースノート { #network-dns-plus-release-notes }

<a id="april-14-2026"></a>
### 2026. 04. 14. { #april-14-2026 }

<a id="april-14-2026-added-features"></a>
#### 新規機能追加

* API v2.0の追加
    * User Access Keyトークンをサポートします。
    
<a id="november-25-2025"></a>
### 2025. 11. 25. { #november-25-2025 }

<a id="november-25-2025-feature-updates"></a>
#### 機能改善/変更

*  TXTレコードセットタイプのレコード値の最大長を255バイトから4096バイトに変更しました。
{% if "gov" not in build_flags %}

<a id="april-29-2025"></a>
### 2025. 04. 29. { #april-29-2025 }

<a id="april-29-2025-feature-updates"></a>
#### 機能改善/変更

*  レコードセット TTL の最小値を 1 から 10 に変更しました。
{% endif %}

<a id="may-28-2024"></a>
### 2024. 05. 28. { #may-28-2024 }

<a id="may-28-2024-added-features"></a>
#### 新規機能追加

* GSLBヘルスチェックでヘルスチェックリクエストのヘッダ、ヘルスチェック周期、最大レスポンス待機時間、最大再試行回数設定機能が追加されました。

<a id="march-12-2024"></a>
### 2024.03.12 { #march-12-2024 }

<a id="march-12-2024-feature-updates"></a>
#### 機能改善/変更

* SPFレコードセットタイプのサポートは終了しました。代わりにTXTレコードセットタイプをご利用ください。
    * 詳細については、[[RFC 7208#section-14.1]](https://datatracker.ietf.org/doc/html/rfc7208#section-14.1)を参照してください。
{% if "gov" in build_flags %}

<a id="december-07-2021"></a>
### 2021. 12. 07. { #december-07-2021 }

<a id="december-07-2021-added-features"></a>
#### 新規機能追加

##### DNS Plus

* レコードセット大量作成機能が追加されました。


<a id="november-03-2020"></a>
### 2020. 11. 03. { #november-03-2020 }

<a id="november-03-2020-feature-updates"></a>
#### 機能改善/変更

* レコードセット修正時にレコードセットタイプを修正できるように改善されました。


<a id="april-07-2020"></a>
### 2020. 04. 07. { #april-07-2020 }

<a id="april-07-2020-release-of-a-new-product"></a>
#### 新規商品リリース

* DNS Plus は、ドメイン管理機能とサーバーのトラフィックを安定的にロードバランシングする機能を提供します。
* DNS (Domain Name System) を使用して、ドメインを簡単に設定および管理できます。
* GSLB (Global Server Load Balancing) を使用して、ルーティングルールに従い、エンドポイントサーバーを DR (Disaster Recovery)、ランダムロードバランシング、グローバルロードバランシングで構成できます。
{% else %}
<a id="august-24-2021"></a>
### 2021. 08. 24. { #august-24-2021 }

<a id="august-24-2021-added-features"></a>
#### 新規機能追加

* レコードセット大量作成機能が追加されました。

<a id="september-22-2020"></a>
### 2020. 09. 22. { #september-22-2020 }

<a id="september-22-2020-feature-updates"></a>
#### 機能改善/変更

* レコードセット修正時にレコードセットタイプを修正できるように改善されました。

<a id="december-24-2019"></a>
### 2019. 12. 24. { #december-24-2019 }

<a id="december-24-2019-added-features"></a>
#### 新規機能追加

* エンドポイントサーバーのトラフィックを安定的にロードバランシングできる GSLB (Global Server Load Balancing) 機能が追加されました。
* 生成される GSLBドメインは、ルーティングルールに従って DR (Disaster Recovery)、ランダムロードバランシング、グローバルロードバランシングとして構成できます。
* Pool はルーティングルールを適用できる最小単位であり、エンドポイントサーバーをグループ化する要素です。
* 定期的に Pool に含まれるエンドポイントサーバーにヘルスチェックを実行することで、安定したサービスを提供できます。ヘルスチェックは HTTP/HTTPS/TCP をサポートします。

<a id="december-24-2019-feature-updates"></a>
#### 機能改善/変更

* レコードセットの作成/修正時、ユーザーのGSLBドメインを選択してCNAMEレコードセットタイプを入力できるように改善しました。


<a id="august-27-2019"></a>
### 2019. 08. 27. { #august-27-2019 }

<a id="august-27-2019-feature-updates"></a>
#### 機能改善/変更

* レコードセットを作成できる最大数を追加しました。DNS Zone1つ当たり、レコードセットは最大5,000個まで作成できます。
* レコードセット統計照会時、CNAMEレコードセットタイプはAレコードセットタイプとAAAAレコードセットタイプを一緒に照会するように修正しました。


<a id="june-25-2019"></a>
### 2019. 06. 25. { #june-25-2019 }

<a id="june-25-2019-release-of-a-new-product"></a>
#### 新規サービスリリース

* DNS Plusはドメイン管理機能を提供するサービスです。
* DNSサーバーを簡単に設定できます。
{%- endif %}
