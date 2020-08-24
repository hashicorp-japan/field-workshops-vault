name: chapter-7
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Chapter 7    
## Dynamic Database Secrets

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???

* Chapter 7 introduces Vault's Database secrets engine which can dynamically generate short-lived credentials for various databases.

---
layout: true

.footer[
- Copyright 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: dynamic-database-secrets
# Dynamic Secrets: データベースのクレデンシャル

* データベースのクレデンシャルは通常長期間変更しません。
* Vault の Database Secrets Engine は、データベース用の短命のクレデンシャルを動的に生成します。
* Vault のDatabase Secrets Engineは、データベース用の短命のクレデンシャルを動的に生成します。
* ユーザーまたはアプリケーションは、Vault から特定のロールの資格情報を要求します。
* Vault は、これらの資格情報のライフサイクルを管理し、TTL の期限が切れるとデータベースから自動的に削除します。


???
* Vault's Database secrets engine supports dynamic generation of short-lived credentials (usernames and passwords) for databases.
* This avoids storing long-lived or permanent credentials on app servers that can easily be compromised.
* Short-lived credentials are much more secure since ex-employees and others are very unlikely to know the current values.

---
name: database-engine-plugins
# Database Secrets Engine: プラグイン
* Cassandra
* Elasticsearch
* Influxdb
* HanaDB
* MongoDB
* MSSQL
* MySQL/MariaDB
* PostgreSQL
* Oracle

???
* The database secrets engine has out-of-the-box plugins for many databases.
* Custom plugins can also be built.

---
name: database-engine-workflow
# Database Secrets Engineのワークフロー

1. データベース シークレット エンジンのインスタンスを有効にする
1. Vault 用に作成したサービス アカウントを使用して、正しいプラグインと接続URLを構成
1. 必要なパーミッション、TTL、SQL ステートメントを設定し、1 つ以上のロールを作成
1. ロールのデフォルトのTTLで有効なクレデンシャルをVaultから取得し、最大TTLまで利用可能
1. Vault は、期限切れのクレデンシャルを自動的に削除
1. また、必要があれば直ちにそのクレデンシャルを取り消すことが可能


???
* This slide lays out the basic workflow used for all of the Datbase secrets engine plugins.
* All of the plugins work the same basic way.
* A service account with permissions to manage users on the database server is required by each connection.
* User creation and revocation SQL statements are specified for roles to determine the permissions og generated users within various databases.
* Multiple connections and roles can be created for a single secrets engine instance to support connecting to multiple database servers with different levels of access.
* The TTL settings can be tuned to suit your needs.

---
name: mysql-configuration-steps
# MySQL用のSecret engineの設定

1. データベースのSecret engineを有効にします。
1. 1. MySQL プラグイン、接続URL、ユーザー名、パスワード、許可されるロールを設定
1. ロールのクレデンシャルをRotationさせます。これによりステップ2で指定したパスワードを変更して、Vaultだけがそれを知っている状態にする
1. 新しいクレデンシャルを作成できるロールを作成

???
* These are the basic steps for configuring the mysql plugin with Vault's database secrets engine.
* The username and password set on the config path must already exist and have permission to manage users.

---
name: mysql-config-connection
class: compact
# MySQLへの接続の設定
#### これらのコマンドを実行して、Database secret engineを有効にし、MySQL で使用するための接続を構成します：
```bash
vault secrets enable -path=lob_a/workshop/database database

vault write lob_a/workshop/database/config/wsmysqldatabase \
    plugin_name=mysql-database-plugin \
    connection_url="{{username}}:{{password}}@tcp(localhost:3306)/" \
    allowed_roles="workshop-app","workshop-app-long" \
    username="hashicorp" \
    password="Password123"

vault write -force lob_a/workshop/database/rotate-root/wsmysqldatabase
```
####  localhost上のMySQLサーバに対して「wsmysqldatabase」という接続が作成されます。

???
* This slide shows the commands to enable the Database secrets engine and configure a connection for MySQL.
* We specified a number of things in the configuration:
    * The path someone would call: "lob_a/workshop/database"
    * The name of the database the role can interact with: wsmysqldatabase
    * The connection URL
    * The initial username and password
    * The roles that can be used with this connection
* We then rotated the password for the "root" user so that only Vault knows it.

---
name: rotating-root-credentials
class: compact
# MySQLのユーザーの作成
### 1. Rootユーザーの代わりに、ユーザを作成したりパスワードを変更したりするのに十分な権限を持つ別のユーザを作成してください。
#### `GRANT ALL PRIVILEGES on *.* to 'hashicorp'@'%' with grant option;`
### 2. 実際のユーザ名は、ホスト `'%'` のものでなければなりません。そのため、`'hashicorp'@'localhost'`ではなく、`'hashicorp'@'%'`のようなユーザを作成してください。

???
* We want to give some advice about rotating root credentials for the database secrets engine when using MySQL.

---
class:compact
# MySQLへのロールの設定
#### このコマンドを実行して、MySQLへのロールを構成します。
```sql
vault write lob_a/workshop/database/roles/workshop-app-long \
    db_name=wsmysqldatabase \
    creation_statements="CREATE USER '{{name}}'@'%' IDENTIFIED BY '{{password}}';
    GRANT ALL ON my_app.* TO '{{name}}'@'%';" \
    default_ttl="1h" \
    max_ttl="24h"
```
#### "wsmysqldatabase "接続に対するロールを定義し、初期TTLが1時間のクレデンシャルを生成します。その有効期限は必要があれば24時間まで延長(Renew)することができます。


???
* We specified a number of things:
    * The creation statements that define the capabilities of the userd that are created
    * The default time to live for generated users
    * The maximum duration for generated users

---
name: mysql-generate-creds
class:compact
# Databaseへの動的シークレットの生成
#### このコマンドで、設定したロールに対してMySQLデータベースのクレデンシャルを生成します。
```bash
vault read lob_a/workshop/database/creds/workshop-app-long  
```
#### 以下のようなレスポンスが返ります。
```bash
Key                Value
---                -----
lease_id           lob_a/workshop/database/creds/workshop-app-long/JeUGIL2xD6BzXSneqity8UmF
lease_duration     1h
lease_renewable    true
password           A1a-zy4ENaf2kwpzGk9t
username           v-token-workshop-a-DM0BJ3eMlMhbf
```

???
* Now, we can begin generating credentials for our MySQL database.

---
name: mysql-renew-revoke-creds
class:compact
# DatabaseクレデンシャルのRenewとRevoke
#### Renew: `<lease_id>` を正しい lease_id に置き換えてクレデンシャルを更新します。
```bash
vault write sys/leases/renew lease_id="<lease_id>" increment="120"  
```
#### Revoke: `<lease_id>` を正しい lease_id に置き換えてクレデンシャルを失効させます。
```bash
vault write sys/leases/revoke lease_id="<lease_id>"
```
#### クレデンシャルの残りの寿命を取得します。
```bash
vault write sys/leases/lookup lease_id="<lease_id>"
```

???
* These are the commands to renew and revoke Vault leases.
* When you run the `renew` command, Vault extends the lifetime of the credentials.
* When you run the `revoke` command, Vault revokes the lease and removes the credentials from the database server.
* It is also possible to determine the remaining lifetime of credentials.

---
name: chapter-7-review-answers
# Chapter 7 Review

* Vault のDatabase secret engineを使用する主な利点は何ですか？
  * クレデンシャルを短命で扱うことで危殆化することを防ぐ
* クレデンシャルの有効期限が切れるとどうなりますか？
  * Vault は、データベースからクレデンシャルを削除します。
* Database secret engineは、この章でリストされているプラグインに限定されますか？
  * カスタム プラグインを作成することができます。
* 1 つの接続に対して複数のロールを使用できますか？
  * はい。これにより、異なるアプリが異なるTTLでクレデンシャルを取得できます。

???
* Here are the answers to the review questions.
