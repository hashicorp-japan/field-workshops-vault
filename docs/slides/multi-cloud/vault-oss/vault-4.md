name: chapter-4
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Chapter 4      
## Vault Secrets Engines

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???

* Chapter 4 introduces Vault secrets engines
* It focuses on the KV v2 engine.

---
layout: true

.footer[
- Copyright 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: vault-secrets-engines-1
# Vault Secrets Engines

.center[![:scale 65%](images/vault-secrets-engines.png)]
.center[Vaultには多くのsecrets enginesがあります。]

???
* Use this screenshot from the Vault UI to talk about Vault's many secrets engines but note that the next slide lists them too.
* Some are for storing static secrets.
* Others can dynamically generate secrets such as database and cloud credentials.
* There is even one called "Transit" that provides encryption as a service.

---
name:vault-secrets-engines-2
# Important Vault Secrets Engines
* Key/Value (KV)
* PKI
* SSH
* TOTP
* Databases
* AWS, Azure, and Google
* Active Directory
* Transit

???
Spend some time pointing out what some of these do:
* KV - Used to manage generic, static secrets. KV v2 supports versioning.
* PKI - Used to generate dynamic X.509 certificates
* SSH - Take all the pain and drudgery out of securing your SSH infrastructure. Vault can provide key signing services that make securing SSH a snap.
* TOTP - The TOTP tool allows Vault to either act as a code-generating device for MFA logins or to provide TOTP server capabilities for MFA infrastructure.
* Databases - Generate dynamic, short-lived database credentials.
* Cloud credentials engines - Generate dynamic, short-lived cloud credentials for major clouds.
* Active Directory - Vault can rotate AD passwords.
* Transit - Implement's Vault's encryption-as-a-service. Provides an API that can handle all your encryption and decryption needs, based on policy, so that you don't have to manage a complicated key infrastructure.

---
name: enabling-secrets-engines
# Enabling Secrets Engines

* ほとんどの Vault secrets engineは、明示的に有効にする必要があります。
	* `vault secrets enable`コマンドで行います。
* 各エンジンはデフォルトのPathがあります。
	* カスタムパスは、CLIコマンドやAPIコールで指定する必要があります。
	* `vault secrets enable -path=aws-east aws` というコマンドで行います。
* カスタムパスはCLIやAPIのエンドポイントとなります。
	* デフォルト：　`vault write aws-east/config/root`
	* カスタムパス：　`vault write aws/config/root` 

???

* Talk about enabling secrets engines.
* Talk about default and custom paths
* Explain the examples

---
name: vault-kv-engine
# Vault's KV Secrets Engine
* VaultのKV Secret engineには2つのバージョンがあります。
  * KV v1 (バージョニングなし)
  * KV v2 (バージョニング付き)
* Vault は、「Prod」モードのサーバでは、KV シークレットエンジンのインスタンスを有効にしません。
* 自分で有効にする必要があります。

???
* We already used Vault's Key/Value (KV) engine in the second challenge of the "Vault Basics" Instruqt track that had been automatically enabled for the "Dev" mode server.
* But we'll need to mount it ourselves for the "Prod" mode server.


---
name: vault-kv-commands
# KV Secrets Engine Commands
* 以下のコマンドを使用して、KV v2 secrets engineのインスタンスをデフォルトパス `kv`にマウントします。
	* `vault secrets enable -version=2 kv` のようにしてください。
* `vault kv` コマンドでKV seacret engineを操作することができます。
  * `vault kv list` は指定されたパスのシークレットをリストアップします。
  * `vault kv put` は指定したパスにシークレットを書き込みます。
  * `vault kv get` は指定したパスにあるシークレットを読み込みます。
  * `vault kv delete` は指定されたパスにあるシークレットを削除します。

???
* サンプル

---
name: lab-vault-basics-challenge-5
# Lab Challenge 4.1: KV v2 Secrets Engine
* In this lab, you'll enable and use the KV v2 secrets engine.
* Note that the path will be `kv` instead of `secret`.
* Instructions:
  * Click the "Use the KV V2 Secrets Engine" challenge of the "Vault Basics" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* Instruct the students to do the "Use the KV V2 Secrets Engine" challenge of the "Vault Basics" track.
* This challenge has them enable an instance of the KV v2 secrets engine.
* Emphasize that the path will be `kv` instead of `secret` as was the case for the challenges with the Dev mode server.

---
name: chapter-4-review-questions
# Chapter 4 Review

* 複数のインスタンスを有効にするために、`vault secrets enable`コマンドにはどのようなオプションが追加されますか?
* KVのSecret engineの2つのバージョンの違いは何ですか?
* KV v2シークレットの古いバージョンは取得できますか?

???
* Let's review what we learned in this chapter.

---
name: chapter-4-review-answers
# Chapter 4 Review

* 複数のインスタンスを有効にするには、`vault secrets enable` コマンドにどのようなオプションを追加しますか?
  * `-path=<path>` オプションを追加し、CLIとAPIで`<path>`を使用してください。
* KV secrets engineの2つのバージョンの違いは何ですか?
  * KV V2はシークレットのバージョニングをサポートしています。
* 古いバージョンのKV v2シークレットを取得することはできますか?
  * KV V2はシークレットのバージョニングをサポートしています。

???
* Here are the answers to the review questions.
