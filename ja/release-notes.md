<!-- pre-align:aligned sig=464bf6032da9 -->

<a id="network-dns-plus-release-notes"></a>
## Network > DNS Plus > リリースノート { #network-dns-plus-release-notes }

<a id="april-14-2026"></a>
### 2026. 04. 14. { #april-14-2026 }

<a id="april-14-2026-added-features"></a>
#### 機能追加
* API v2.0の追加
    * User Access Keyトークンをサポートします。
    
<a id="november-25-2025"></a>
### 2025. 11. 25. { #november-25-2025 }

<a id="november-25-2025-feature-updates"></a>
#### 機能変更
* TXTレコードセットタイプのレコード値の最大長を、255バイトから4096バイトに変更しました。

<a id="april-29-2025"></a>
### 2025. 04. 29. { #april-29-2025 }

<a id="april-29-2025-feature-updates"></a>
#### 機能変更
* レコードセットTTLの最小値を1から10に変更しました。

<a id="may-28-2024"></a>
### 2024. 05. 28. { #may-28-2024 }

<a id="may-28-2024-added-features"></a>
#### 機能追加 
* GSLBヘルスチェックでヘルスチェックリクエストのヘッダ、ヘルスチェック周期、最大レスポンス待機時間、最大再試行回数設定機能が追加されました。

<a id="march-12-2024"></a>
### 2024.03.12 { #march-12-2024 }

<a id="march-12-2024-feature-updates"></a>
#### 機能改善

* SPF レコードセットタイプのサポートは終了しました。代わりにTXTレコードセットタイプを使用できます。
    * 詳細は[[RFC 7208#section-14.1]](https://datatracker.ietf.org/doc/html/rfc7208#section-14.1)で確認できます。

<a id="august-24-2021"></a>
### 2021. 08. 24. { #august-24-2021 }

<a id="august-24-2021-added-features"></a>
#### 機能追加

<a id="september-22-2020"></a>
### 2020. 09. 22. { #september-22-2020 }

<a id="september-22-2020-feature-updates"></a>
#### 機能変更

* レコードセットを修正時にレコードセットタイプを修正できるようになりました。


<a id="december-24-2019"></a>
### 2019. 12. 24. { #december-24-2019 }

<a id="december-24-2019-added-features"></a>
#### 機能追加

* エンドポイントサーバーのトラフィックを安定的にロードバランシングすることができるGSLB(Global Server Load Balancing)機能を追加しました。
* 作成されるGSLBドメインは、ルーティングルールに従ってDR(Disaster Recovery)、ランダムロードバランシング、全世界的なロードバランシングで構成できます。
* Poolはルーティングルールを適用することができる最小単位で、エンドポイントサーバーをグルーピングする要素です。
* Poolに含まれたエンドポイントサーバーで周期的にヘルスチェックを行い、安定的なサービスをサポートします。ヘルスチェックはHTTP/HTTPS/TCPをサポートします。

<a id="december-24-2019-feature-updates"></a>
#### 機能改善

* レコードセットの作成/修正時、ユーザーのGSLBドメインを選択してCNAMEレコードセットタイプを入力できるように改善しました。


<a id="august-27-2019"></a>
### 2019. 08. 27. { #august-27-2019 }

<a id="august-27-2019-feature-updates"></a>
#### 機能改善

* レコードセットを作成できる最大数を追加しました。DNS Zone1つ当たり、レコードセットは最大5,000個まで作成できます。
* レコードセット統計照会時、CNAMEレコードセットタイプはAレコードセットタイプとAAAAレコードセットタイプを一緒に照会するように修正しました。


<a id="june-25-2019"></a>
### 2019. 06. 25. { #june-25-2019 }

<a id="june-25-2019-release-of-a-new-product"></a>
#### 新規サービスリリース

* DNS Plusは、ドメイン管理機能を提供するサービスです。
* DNS Serverを簡単に設定できます。
