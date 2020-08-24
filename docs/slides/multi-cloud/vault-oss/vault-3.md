name: Chapter-3
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Chapter 3      
## Vault Server Prodモード

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???

Chapter 3 focuses on running a production Vault server

---
layout: true

.footer[
- Copyright 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: vault-production-serves
# Vault server Prodモード
* ProdモードでVaultサーバを実行するには、複数の手順が必要です。
  * configファイルで構成を指定します。
  * サーバを起動します。
  * サーバーを初期化して、Unsealキーと初期ルート トークンを取得します。
  * Unsealキーを使用して、Vault サーバの暗号化を解除します。

???
* Describe the steps to run a production Vault server.

---
name: configuring-vault
# Vault Serversの設定
* VaultのConfigファイルは[HCL](https://github.com/hashicorp/hcl) もしくは JSONで記述されます。
* Common configuration settings:
  * listener
  * storage
  * seal
  * log_level
  * ui
  * api_addr
  * cluster_addr

???
* Discuss Vault configuration files and common settings.

---
name: running-vault
# Vault serverの起動
* Vault Productionサーバを起動するには、`vault server`コマンドを使用します。
* **注意** `-dev` オプションは使用しません。

???
* Describe the command to run a Vault production server.

---
name: initializing-vault
# Vaultクラスタの初期化
* 1 つの Vault クラスタは、複数の Vault サーバで構成されます。
* 各 Vault クラスタは、一度初期化する必要があります。
* 初期化は`vault operator init`コマンドで行います。
* Unsealキーの数と鍵のしきい値は、`-key-shares` および `key-threshold` オプションで指定できます。
* このコマンドにより、Unsealキーとクラスタの初期ルートトークンを返します。

???
* Describe Vault's `init` command

---
name: unsealing-vault
# Vault ServerのUnseal
* 各 Vault サーバーは、起動するたびにUnsealされる必要があります。
* Unsealされるまでは、サーバーを使用することはできません。
* Unsealは、クラスタを初期化したときに返された unseal キーを使用して、`vault operator unseal` コマンドで行います。

???
* Describe Vault's `unseal` command.

---
name: vault-status-command
# Vault serverの状態確認
* Vault サーバの状態を取得するには、`vault status` コマンドを使用します。
* これにより、Vault サーバがSealされているか、Sealされていないかがわかります。
* また、以下の情報も表示されます。
  * 鍵の数と鍵のしきい値
  * HA モード（クラスタリング）が有効かどうか
  * サーバーがパフォーマンススタンバイとして動作しているかどうか。

???
Describe the `vault status` command

---
name: chapter-3-review-answers
# Chapter 3 Review

* ProdモードのVaultサーバーを構成するために使用するものは？
  * Configファイル
* 新しいVaultクラスタに対して一度だけ実行する必要があるVaultコマンドは？
  * `vault operator init` 
* Vault サーバが起動するたびに実行しなければならない Vault コマンドは？
  * `vault operator unseal` 

???
* Here are the answers to the review questions.
