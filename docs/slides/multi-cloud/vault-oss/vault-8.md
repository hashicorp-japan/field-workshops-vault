name: chapter-8
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Chapter 8    
## Encryption as a Service

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???

* Chapter 8 introduces Vault's Transit secrets engine which functions as Vault's Encryption-as-a-Service (EaaS).

---
layout: true

.footer[
- Copyright 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: Vault-Transit-Engine

# Vault Transit Engine - Encryption as a Service
.center[![:scale 80%](images/vault-eaas.webp)]

* サービスとしての暗号化の機能を提供します。
* Vault の外部に保存されたデータの暗号化と復号化に使用します。

???
* Let's talk about Vault's Encryption-as-a-Service, the Transit secrets engine.
* It provides an encryption API & service that are secure, accessible and easy to implement.
* Instead of forcing developers to learn cryptography, we present them with a familiar API that can be used to encrypt and decrypt data that is stored outside of Vault.

---
name: transit-engine-benefits
# Transit Engineの利点

* 開発者が暗号化や暗号化の専門家にならなくても、簡単に使えるEaaS APIを提供
* 暗号鍵の一元管理
* 承認された暗号とアルゴリズムのみが使用されることを保証
* 自動化された鍵のローテーションと暗号データのリラップ
* 攻撃者が暗号化されたデータを読みだしたとしても、Vaultなしでは役に立たない暗号文

???
* There are seveal benefits of using the Transit engine.

---
name: Vault-Transit-Engine-1
# Vault Transit - サンプルアプリ

* 次のワークショップでは、データの暗号化と復号化にTransit engineを使用するWebアプリを使用します。
* アプリは暗号化されたデータをMySQLデータベースに保存します。
* データベースシークレットエンジンからMySQLのクレデンシャルを取得します。
* 最初に Vault なしで Web アプリを実行します。レコードは暗号化されません。
* 次に、Vaultを有効にして実行し、新しいレコードが暗号化されていることを確認します。

???
* Discuss the web app we will be using in this chapter's lab.
* Point out that it will use the same MySQL database from chapter 7.
* Point out that it will get its MySQL credentials from the Database secrets engine students set up in chapter 7.
* Indicate that we will first run without Vault and then with it.

---
name: web-app-screenshot
# Webアプリ
### Webアプリケーションのスクリーンショット

.center[![:scale 70%](images/transit_app.png)]

???
* Show the screenshot of the web app.

---
name: web-app-views
# WebアプリのView
###アプリケーションには主に2つのセクションがあります。
1. **レコードビュー**
  * 暗号化されたデータが復号化された後、ログインしているユーザーに見えるものを、プレーンテキストで表示します。

1. **データベースビュー**
  * データベース内の生のレコードが表示され、実際のSQLコマンドで返るものが表示されます。

---
name: records-view
# Webアプリのレコードビュー
.center[![:scale 90%](images/records_view.png)]

* アプリが暗号化されたデータを復号化しているため、ユーザーは機密データを見ることができます。

???
* Show the records view of the web app.

---
name: Vault-Transit-Engine-6
# Add User画面
* ユーザー情報を追加します。
.center[![:scale 60%](images/add_user.png)]

???
* Describe the Add User screen that students will use to add new records to the database.
* Point out again that when Vault is enabled, the records will be encrypted in the database.

---
name: database-record-without-vault
# データベースビューのレコードで、Vaultが有効になっていない場合
* レコードを追加した後、**Database View**メニューをクリック
* 入力したデータと全く同じデータが表示される
* これは、個人識別データ（PII）がデータベースのレコードにプレーンテキストで保存されていることを意味する。
* これを改善するためにVault のTransit engineを有効にします。

---
name: encrypted-record
# Vaultにより暗号化されたレコード
#### こちらが暗号化されたレコードです。
.center[![:scale 80%](images/database_view_with_encrypted_record.png)]
* Birth_dateとSocial_security_numberが暗号化されます。

???
* Show a record from the database encrypted by Vault's Transit engine.
* Point out that the birth_date and social_security_number field are encrypted as indicated by their starting with "vault:v1".
* Point out that the "v1" indicates that the first version of the encryption key was used.

---
name: encryption-key-rotation
# 暗号鍵のRotation
* Vaults Transit Engineの暗号化鍵はRotationさせることが可能
* 古い鍵は明示的に削除しない限り、Vaultに保管
* 新しいデータの暗号化には最新バージョンの鍵を使用
* 古いバージョンの暗号化キーは古いデータを復号化することは可能。
* 新しい鍵で暗号化されたデータを復号化することはできない。
* 暗号化鍵をRotationさせても、古い鍵がある限りTransitエンジンを使用するアプリは動作します。
* データは `rewrap` エンドポイントを使って新しい鍵で再暗号化することができます。


---
name: chapter-8-review-answers
# Chapter 8 Review
* Transit engineを使用する主な利点は何ですか？
  * 暗号技術の専門家でなくてもデータを暗号化することが可能
* Transit engineは、暗号化されたデータをどこに保存できますか？
  * どこでも。

???
* Here are the answers to the review questions.

---
name: chapter-8-review-answers
# Chapter 8 Review
* 暗号鍵をRotationしても、古い鍵で暗号化されたレコードを復号化することは可能ですか？
  * 可能です。
* どのバージョンの暗号化キーが使用されたかを知ることはできますか？
  * はい。バージョンは暗号化されたテキストに`v1`, `v2` などで表示されます。

---
name: conclusion
# ご参加ありがとうございました！
.center[![:scale 40%](images/vault_logo.png)]

### 以下のドキュメントなども是非参照くださいませ。
* https://www.vaultproject.io/docs/
* https://www.vaultproject.io/api/
* https://learn.hashicorp.com/vault

???
* Thank the students for their participation
* Share some Vault links

---
name: Feedback-Survey
# Workshop Feedback アンケート
* アンケートにお答えください。
  * http://bit.ly/hashiworkshopfeedback

???
* Ask them to fill out the online survey
