name: Chapter-2
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false



# Chapter 2      
## Vaultへのインターフェース

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???
Chapter 2 focuses on interacting with Vault

---
layout: true

.footer[
- Copyright 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: Interacting-With-Vault
# Vaultへのインターフェース

Vaultとのやり取りはいくつかの方法があります。
* The Vault [CLI](https://www.vaultproject.io/docs/commands/index.html)
* The Vault [UI](https://learn.hashicorp.com/vault/getting-started/ui)
* The Vault [API](https://www.vaultproject.io/api-docs/index/)

???

* Chapter 2 focuses on interacting with Vault

---
name: Vault-CLI
# The Vault CLI
* Vault CLIは、Goアプリケーションです。
* MacOS、Windows、Linux、およびその他のオペレーティングシステムで動作します。
* こちらから最新バージョンをダウンロードできます。[here](https://www.vaultproject.io/downloads/)	

???
* The Vault CLI is distributed as a Go binary.
* It runs on multiple operating systems.

---
name: installing-Vault-CLI
# Vaultのインストール
* Vaultをインストールするのは簡単です。
  * zip ファイルをダウンロードします。
  * バイナリを解凍します。
  * バイナリをパスに配置します。


詳細は[こちら](https://learn.hashicorp.com/vault/getting-started/install)を参照ください。 

???
Installing Vault is easy.

---
name: some-cli-commands
# Vaultの基本的なコマンド（いくつか）
* `vault`は、多くのVault CLIサブコマンドがあります。
* `vault version` は、実行している Vault のバージョンを教えてくれます。
* `vault read` は、Vaultからシークレットを読み取るために使用します。
* vault にシークレットを書き込むには `vault write` を使用します。

`h`, `help`, `--help` フラグは、Vault CLI コマンドのヘルプを取得するために追加することができます。


???
Let's discuss some of the basic Vault CLI commands.

---
name: vault-server-modes
# Vault Server Modes
Vault サーバは、2 つのモードで動作させることができます。
* 開発や機能テストのみを目的とした"Dev"モード
* QAや本番環境で使用できる "Prod "モード

???
* Discuss Vault's two server modes

---
name: vault-dev-server
# Vault's "Dev" Mode
* 安全ではありません。
* メモリにすべてを保存します。
	* Vaultサーバーを落とすと全てのデータは消滅します。
* VaultはSealが解除された状態で起動します。
	* Unsealの手間が入りません。
* 起動前にルートトークンを指定することができます。
	* `root` など簡単なトークンIDを指定します。



**Devモードは本番環境では使わないでください。**

???
* Discuss limitations of Vault's "Dev" mode.
* Warn students to never store real secrets on a Dev server.

---
name: Vault-UI
# The Vault UI
* Vault UI を使用するには、まずログインします。
* Vault は複数の認証方法をサポートしています。
* インストールした直後のVault サーバーはToken auth メソッドのみが有効です。	


???

* Let's talk about the Vault UI a bit, including ways of signing into it.
* While you used the token "root" in the last challenge, you'll be running a Vault server in "Prod"  mode in the rest of the track and will have to use the token generated when you initialize that server in the next challenge.
---
name: signing-into-the-vault-ui
# VaultへのSign in
.center[![:scale 70%](images/vault_login_page.png)]

???
* This slide shows a screenshot of the login dialog for the Vault server.

---
name: welcome-to-vault
# "Welcome to Vault" ツアー
.center[![:scale 60%](images/take_a_tour.png)]
* 最初にVaultにログインすると"Welcome to Vault"ツアーがあります。

???
* Explain the "Welcome to Vault" tour.
* Explain how to get rid of it.
* Point out that they can restart the tour with the "Restart guide" menu under their user icon in the upper right corner of the UI.

---
name: vault-api-1
# The Vault API

* Vaultの全てのオペレーションはHTTP APIで可能です。
	* Vault の設定や秘密の管理もAPIで行います。
* Vault の健全性をチェックするには、単純な `curl` コマンドと `jq` コマンドを実行し、JSON 出力をフォーマットします。

Command:
```bash
curl http://localhost:8200/v1/sys/health | jq
```
???
* Let's talk about the Vault HTTP API

---
name: vault-api-2
# The Vault API

```json
{
  "initialized": true,
  "sealed": false,
  "standby": false,
  "performance_standby": false,
  "replication_performance_mode": "disabled",
  "replication_dr_mode": "disabled",
  "server_time_utc": 1557180149,
  "version": "1.1.1",
  "cluster_name": "vault-cluster-db6f271d",
  "cluster_id": "33e85d7c-63bb-7523-0165-9d1aee722d70"
}
```

???
Here is the output from Vault's sys/health endpoint

---
name: vault-api-3
# 認証用のVault API
* sys/health エンドポイントは認証は必要ありません。
* ただし、ほとんどの Vault API 呼び出しは認証を必要とします。
* 認証は`X-Vault-Token` ヘッダーで提供される Vault トークンで行われます。

???
* Talk about how most Vault HTTP API calls will require authentication with a Vault token.

---
name: chapter-2-review-answers
# Chapter 2 Review
* Vaultへのインターフェースは何があるか？
  * Vault CLI
  * Vault UI
  * Vault API
* Vaultコマンドのヘルプはどう取得できるか？
  * `-h`, `-help`, `--help`
* Vaultの２種類のサーバーモードは何か？
  * DevとProd

???
* Here are the answers to the review questions.
