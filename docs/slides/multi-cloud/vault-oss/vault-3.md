name: Chapter-3
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Chapter 3      
## Running a Production Vault Server

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
# Running a Production Vault Server
* ProdモードでVaultサーバを実行するには、複数の手順が必要です。
  * configファイルで構成を指定します。
  * サーバを起動します。
  * サーバーを初期化して、Unsealキーと初期ルート トークンを取得します。
  * Unsealキーを使用して、Vault サーバの暗号化を解除します。

???
* Describe the steps to run a production Vault server.

---
name: configuring-vault
# Configuring Vault Servers
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
# Starting a Production Vault Server
* Vault Productionサーバを起動するには、`vault server`コマンドを使用します。
* **注意** `-dev` オプションは使用しません。

???
* Describe the command to run a Vault production server.

---
name: initializing-vault
# Initializing Vault Clusters
* 1 つの Vault クラスタは、複数の Vault サーバで構成されます。
* 各 Vault クラスタは、一度初期化する必要があります。
* 初期化は`vault operator init`コマンドで行います。
* Unsealキーの数と鍵のしきい値は、`-key-shares` および `key-threshold` オプションで指定できます。
* このコマンドにより、Unsealキーとクラスタの初期ルートトークンを返します。

???
* Describe Vault's `init` command

---
name: unsealing-vault
# Unsealing Vault Servers
* 各 Vault サーバーは、起動するたびにUnsealされる必要があります。
* Unsealされるまでは、サーバーを使用することはできません。
* Unsealは、クラスタを初期化したときに返された unseal キーを使用して、`vault operator unseal` コマンドで行います。

???
* Describe Vault's `unseal` command.
---
name: lab-vault-basics-challenge-4
# Lab Challenge 3.1: Run a Vault "Prod" Server
* In this lab, you'll run your first Vault server in "Prod" mode.
* You'll learn how to initialize and unseal a Vault server.
* Instructions:
  * Click the "Run a Production Server" challenge of the "Vault Basics" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* Instruct the students to do the "Run a Production Server" challenge of the "Vault Basics" track.
* This challenge has them examine a Vault server configuration file, run a Prod server, initialize it, and unseal it.
* Remind students to save their unseal key and root token.

---
name: vault-status-command
# Determining the Status of a Vault Server
* Vault サーバの状態を取得するには、`vault status` コマンドを使用します。
* これにより、Vault サーバがSealされているか、Sealされていないかがわかります。
* また、以下の情報も表示されます。
  * 鍵の数と鍵のしきい値
  * HA モード（クラスタリング）が有効かどうか
  * サーバーがパフォーマンススタンバイとして動作しているかどうか。

???
Describe the `vault status` command

---
name: chapter-3-review-questions
# Chapter 3 Review

* ProdモードのVaultサーバを構成するために使用するものは何ですか？
* 新しいVaultクラスタに対して、どのようなVaultコマンドを一度だけ実行する必要がありますか？
* Vault サーバを起動するたびに、どのような Vault コマンドを実行する必要がありますか？

???
* Let's review what we learned in this chapter.

---
name: chapter-3-review-answers
# Chapter 3 Review

* Prod」モードのVaultサーバーを構成するために使用するものは何ですか？
  * Configファイル
* 新しいVaultクラスタに対して一度だけ実行する必要があるVaultコマンドは何ですか？
  * `vault operator init` 
* Vault サーバが起動するたびに実行しなければならない Vault コマンドは何ですか?
  * `vault operator unseal` 

???
* Here are the answers to the review questions.
