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
# Dynamic Secrets: Protecting Databases

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
# Database Secrets Engine: Plugins
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
# Database Secrets Engine Workflow

1. データベース シークレット エンジンのインスタンスを有効にします。
1. Vault 用に作成したサービス アカウントを使用して、正しいプラグインと接続 URL で構成します。
1. 必要なパーミッションを指定する TTL および SQL ステートメントを使用して、1 つ以上のロールを作成します。
1. アプリケーションとユーザーは、ロールのデフォルトの TTL で有効なクレデンシャルを Vault から取得し、最大 TTL まで更新することができます。
1. 1.Vault は、期限切れのクレデンシャルを自動的に削除します。
1. クレデンシャルが漏洩した場合、直ちにそのクレデンシャルを取り消すことができます。


???
* This slide lays out the basic workflow used for all of the Datbase secrets engine plugins.
* All of the plugins work the same basic way.
* A service account with permissions to manage users on the database server is required by each connection.
* User creation and revocation SQL statements are specified for roles to determine the permissions og generated users within various databases.
* Multiple connections and roles can be created for a single secrets engine instance to support connecting to multiple database servers with different levels of access.
* The TTL settings can be tuned to suit your needs.

---
name: sample-web-app
# Lab Environment for Chapters 7 and 8

* 第7章と第8章のラボでは、Vaultサーバ上で動作するMySQLデータベースサーバを使用します。
* 次のスライドでは、ワークショップで行う多くのステップの概要を説明します。

???
* Discuss the lab environment.

---
name: mysql-configuration-steps
# Configuration Steps for MySQL

1. いくつかのパスでデータベースのを有効にします。
1. 1. MySQL プラグイン、接続 URL、ユーザー名、パスワード、許可されるロールを設定します。
1. ルートのクレデンシャルをRotationさせます。Vault は、ステップ 2 で指定したパスワードを変更して、人間が知らないようにします。
1. 特定の期間有効な新しいクレデンシャルを作成できるロールを作成します。

???
* These are the basic steps for configuring the mysql plugin with Vault's database secrets engine.
* The username and password set on the config path must already exist and have permission to manage users.

---
name: mysql-config-connection
class: compact
# Configuring Connections for MySQL
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
####  これにより、localhost上のMySQLサーバに対して「wsmysqldatabase」という接続が作成されます。

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
# Rotating the Root Credentials for MySQL
#### 1. Rootユーザーの代わりに、ユーザを作成したりパスワードを変更したりするのに十分な権限を持つ別のユーザを作成してください。これは、`GRANT ALL PRIVILEGES on *.* to 'hashicorp'@'%' with grant option;`を実行することで行うことができます。
#### 2. 実際のユーザ名は、ホスト `'%'` のものでなければなりません。そのため、`'hashicorp'@'localhost'`ではなく、`'hashicorp'@'%'`のようなユーザを作成してください。
#### 3. ユーザのホストとして `'%'` を使用したくない場合は、パス `<database>/config/<connection>` に書き込む際に `root_rotation_statements` を指定することができます; 例えば、`"ALTER USER '{{username}}}'@'localhost' IDENTIFIED BY '{{password}}}';"`.


???
* We want to give some advice about rotating root credentials for the database secrets engine when using MySQL.

---
class:compact
# Configuring Roles for MySQL
#### このコマンドを実行して、MySQLのロールを構成します。
```sql
vault write lob_a/workshop/database/roles/workshop-app-long \
    db_name=wsmysqldatabase \
    creation_statements="CREATE USER '{{name}}'@'%' IDENTIFIED BY '{{password}}';
    GRANT ALL ON my_app.* TO '{{name}}'@'%';" \
    default_ttl="1h" \
    max_ttl="24h"
```
#### これは、"wsmysqldatabase "接続に対するロールを定義し、初期TTLが1時間のクレデンシャルを生成します。しかし、その有効期限は24時間まで延長することができます。


???
* We specified a number of things:
    * The creation statements that define the capabilities of the userd that are created
    * The default time to live for generated users
    * The maximum duration for generated users

---
name: mysql-generate-creds
class:compact
# Generating Database Credentials
#### このコマンドを実行して、前のスライドで設定したロールに対してMySQLデータベースの実際の資格情報を生成します。
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
# Renewing and Revoking Database Credentials
####このコマンドを実行して、`<lease_id>` を正しい lease_id に置き換えてクレデンシャルを更新します。
```bash
vault write sys/leases/renew lease_id="<lease_id>" increment="120"  
```
#### このコマンドを実行して、`<lease_id>` を正しい lease_id に置き換えてクレデンシャルを失効させます。
```bash
vault write sys/leases/revoke lease_id="<lease_id>"
```
#### また、クレデンシャルの残りの寿命を判断することもできます。
```bash
vault write sys/leases/lookup lease_id="<lease_id>"
```

???
* These are the commands to renew and revoke Vault leases.
* When you run the `renew` command, Vault extends the lifetime of the credentials.
* When you run the `revoke` command, Vault revokes the lease and removes the credentials from the database server.
* It is also possible to determine the remaining lifetime of credentials.

---
name: lab-database-challenge-1
# Lab Challenge 7.1: Enable the Engine
* In this lab challenge, you'll enable the database engine for MySQL and rotate its root credentials.
* You'll do this in the [Vault Dynamic Database Credentials](https://play.instruqt.com/hashicorp/invite/sryhqfdm6sgx) Instruqt track.
* Instructions:
  * Click the "Enable the Database Secrets Engine" challenge of the "Vault Dynamic Database Credentials" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* Instruct the students to do the "Enable the Database Secrets Engine" challenge of the "Vault Dynamic Database Credentials" track.
* This challenge has them enable the Database secrets engine on the path "lob_a/workshop/database" and rotate the root credentials for it.

---
name: lab-database-challenge-2
# Lab Challenge 7.2: Configure the Engine
* In this lab, you'll configure a connection and two roles for the database.
* Instructions:
  * Click the "Configure the Database Secrets Engine" challenge of the "Vault Dynamic Database Credentials" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* Instruct the students to do the "Configure the Database Secrets Engine" challenge of the "Vault Dynamic Database Credentials" track.
* This challenge has them configure a connection and two roles for the engine they created in the previous challenge.
* One role will have an initial TTL of 1 hour with a maximum TTL of 24 hours.
* The other will have an initial TTL of 3 minutes with a maximum TTL of 6 minutes.

---
name: lab-database-challenge-3
# Lab Challenge 7.3: Generate Credentials
* In this lab, you'll generate and use credentials against both roles that you configured in the previous challenge.
* Instructions:
  * Click the "Generate and Use Dynamic Database Credentials" challenge of the "Vault Dynamic Database Credentials" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* Instruct the students to do the "Generate and Use Dynamic Database Credentials" challenge of the "Vault Dynamic Database Credentials" track.
* This challenge has them generate short-lived credentials for the MySQL database.
* They will use the `mysql` utility to connect to the database with those credentials.
* They will see that the credentials are deleted after 3 minutes and that logging into MySQL with them is blocked.

---
name: lab-database-challenge-4
# Lab Challenge 7.4: Renew/Revoke Credentials
* In this lab, you'll renew and revoke credentials generated by the database secrets engine.
* Instructions:
  * Click the "Renew and Revoke Database Credentials" challenge of the "Vault Dynamic Database Credentials" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* Instruct the students to do the "Renew and Revoke Database Credentials" challenge of the "Vault Dynamic Database Credentials" track.
* This challenge has them extend the liftime of generated credentials with Vault's `sys/leases/renew` endpoint.
* They will also revoke credentials with Vault's `sys/leases/revoke` endpoint.

---
name: chapter-7-review-questions
# Chapter 7 Review
* Vault のDatabase secret engineを使用する主な利点は何ですか？
* クレデンシャルの有効期限が切れた場合はどうなりますか？
* Database secret engineは、この章に記載されているプラグインに限定されていますか？
* 1 つの接続に対して複数のロールを使用することはできますか？

???
* Let's review what we learned in this chapter.

---
name: chapter-7-review-answers
# Chapter 7 Review

* Vault のDatabase secret engineを使用する主な利点は何ですか？
  * クレデンシャルは短命であり、危殆化する可能性が低い。
* クレデンシャルの有効期限が切れるとどうなりますか？
  * Vault は、データベース サーバーからクレデンシャルを削除します。
* Database secret engineは、この章でリストされているプラグインに限定されますか？
  * カスタム プラグインを作成することができます。
* 1 つの接続に対して複数のロールを使用できますか？
  * はい。これにより、異なるアプリが異なるTTLでクレデンシャルを取得することができます。

???
* Here are the answers to the review questions.
