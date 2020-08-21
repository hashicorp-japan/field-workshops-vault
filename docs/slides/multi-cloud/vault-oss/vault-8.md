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

* Vault の Transit Secrets エンジンは、サービスとしての暗号化の機能を提供します。
* 開発者は、Vault の外部に保存されたデータの暗号化と復号化に使用します。

???
* Let's talk about Vault's Encryption-as-a-Service, the Transit secrets engine.
* It provides an encryption API & service that are secure, accessible and easy to implement.
* Instead of forcing developers to learn cryptography, we present them with a familiar API that can be used to encrypt and decrypt data that is stored outside of Vault.

---
name: transit-engine-benefits
# Transit Engine Benefits

* VaultのTransit engineは、開発者が暗号化や暗号化の専門家になる必要がないように、簡単に使えるEaaS APIを提供します。
* 鍵の一元管理を提供します。
* 承認された暗号とアルゴリズムのみが使用されることを保証します。
* 自動化された鍵のローテーションとリラップをサポートします。
* 攻撃者が暗号化されたデータを読みだしたとしても、それはVaultなしでは役に立たない暗号文だけです。

???
* There are seveal benefits of using the Transit engine.

---
name: Vault-Transit-Engine-1
# Vault Transit - Example Application

* 次のワークショップでは、データの暗号化と復号化にTransit engineを使用するWebアプリケーションを使用します。
* アプリは暗号化されたデータを第7章で使用したのと同じMySQLデータベースに保存します。
* また、第7章のラボで設定したデータベースシークレットエンジンからMySQLのクレデンシャルを取得します。
* 最初に Vault なしで Web アプリを実行します。レコードは暗号化されません。
* 次に、Vaultを有効にして実行し、新しいレコードが暗号化されていることを確認します。

???
* Discuss the web app we will be using in this chapter's lab.
* Point out that it will use the same MySQL database from chapter 7.
* Point out that it will get its MySQL credentials from the Database secrets engine students set up in chapter 7.
* Indicate that we will first run without Vault and then with it.

---
name: web-app-screenshot
# The Web App
### Webアプリケーションのスクリーンショット

.center[![:scale 70%](images/transit_app.png)]

???
* Show the screenshot of the web app.

---
name: web-app-views
# The Web App's Views
###アプリケーションには主に2つのセクションがあります。
1. **レコードビュー
  * レコードビューでは、暗号化されたデータが復号化された後、ログインしているユーザーが見ることができるものを、プレーンテキストで表示します。

1. **データベースビュー***。
  * データベースビューでは、データベース内の生のレコードが表示され、実際のSQLコマンドで返されるものが表示されます。

---
name: records-view
# The Web App's Records View
.center[![:scale 90%](images/records_view.png)]

* アプリが暗号化されたデータを復号化しているため、権限のあるユーザーは機密データの一部を見ることができます。

???
* Show the records view of the web app.

---
name: Vault-Transit-Engine-6
# The Add User Screen
* ユーザー情報を追加します。
.center[![:scale 60%](images/add_user.png)]

???
* Describe the Add User screen that students will use to add new records to the database.
* Point out again that when Vault is enabled, the records will be encrypted in the database.

---
name: database-record-without-vault
# データベースビューのレコードで、Vaultが有効になっていない場合
* ワークショップでレコードを追加した後、**Database View**メニューをクリックするように指示されます。
* 入力したデータと全く同じデータが表示されます。
* これは、個人識別データ（PII）がデータベースのレコードにプレーンテキストで保存されていることを意味します。
* これを改善するにはどうしたらよいでしょうか？Vault のTransit engineを有効にして確認してみましょう。

---
name: encrypted-record
# A Database Record Encrypted by Vault
#### こちらが暗号化されたレコードです。
.center[![:scale 80%](images/database_view_with_encrypted_record.png)]
* 生年月日と社会保障番号は暗号化されています。

???
* Show a record from the database encrypted by Vault's Transit engine.
* Point out that the birth_date and social_security_number field are encrypted as indicated by their starting with "vault:v1".
* Point out that the "v1" indicates that the first version of the encryption key was used.

---
name: encryption-key-rotation
# Rotating Transit Engine Encryption Keys
* Vaults Transit Engineの暗号化キーはRotationさせることができます。
* 新しいデータの暗号化には最新バージョンのキーが使用されます。
* 古いバージョンの暗号化キーは古いデータを復号化することはできますが、新しいデータを復号化することはできません。
* 暗号化キーをRotationさせても、Transitエンジンを使用するアプリは変更を気にする必要はありません。	
* データは `rewrap` エンドポイントを使って再暗号化することもできます。


---
name: lab-transit-challenge-1
# Challenge 8.1: Enable the Transit Engine
* In this lab challenge, you'll enable the Transit engine.
* You'll do this in the [Vault Encryption as a Service](https://play.instruqt.com/hashicorp/invite/qleasfx1dszc) Instruqt track.
* Instructions:
  * Click the "Enable the Transit Secrets Engine" challenge of the "Vault Encryption as a Service" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* Instruct the students to do the "Enable the Transit Secrets Engine" challenge of the "Vault Encryption as a Service" track.
* This challenge has them enable the Transit secrets engine on the path "lob_a/workshop/transit".

---
name: lab-database-challenge-2
# Challenge 8.2: Create an Encryption Key
* In this lab, you'll create an encryption key for use with the Transit engine you enabled in the previous challenge.
* Instructions:
  * Click the "Create a Key for the Transit Secrets Engine" challenge of the "Vault Encryption as a Service" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* Instruct the students to do the "Create a Key for the Transit Secrets Engine" challenge of the "Vault Encryption as a Service" track.
* This challenge has them create an encryption key for use with the Transit engine they enabled in the previous challenge.

---
name: lab-database-challenge-3
# Challenge 8.3: Use the Web App Without Vault
* In this lab, you'll use the web application without Vault.
* Instructions:
  * Click the "Use the Web App Without Vault" challenge of the "Vault Encryption as a Service" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* Instruct the students to do the "Use the Web App Without Vault" challenge of the "Vault Encryption as a Service" track.
* This challenge has them use the web application without Vault.
* Point out that the new record they add during this challenge will nto be encrypted.

---
name: lab-database-challenge-4
# Challenge 8.4: Use the Web App With Vault
* In this lab, you'll use the web application with Vault.
* You'll also rotate the encryption key.
* Instructions:
  * Click the "Use the Web App With Vault" challenge of the "Vault Encryption as a Service" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* Instruct the students to do the "Use the Web App With Vault" challenge of the "Vault Encryption as a Service" track.
* This challenge has them use the web application with Vault.
* Point out that the new record they add will have sensitive fields encrypted by Vault's Transit engine.

---
name: chapter-8-review-questions
#  Chapter 8 Review
* VaultのTransit engineを使用する主な利点は何ですか？
* VaultのTransit engineは、暗号化されたデータをどこに保存していますか？
* 暗号化キーをRotationさせても、アプリケーションは古い暗号化されたレコードを復号化することができましたか？
* 暗号化キーのどのバージョンが使用されたかを知ることはできますか？

???
* Let's review what we learned in this chapter.

---
name: chapter-8-review-answers
# Chapter 8 Review
* VaultのTransit engineを使用する主な利点は何ですか？
  * 開発者は、暗号技術の専門家でなくてもデータを暗号化することができます。
* VaultのTransit engineは、暗号化されたデータをどこに保存していますか？
  * 開発者が望む場所であればどこでも、Vaultの外であればどこでも。
* 暗号化キーをRotationさせても、アプリケーションは古い暗号化されたレコードを復号化することができましたか？
  * 暗号化キーを回転させた後も、アプリケーションは古い暗号化されたレコードを復号化できましたか？
* どのバージョンの暗号化キーが使用されたかを知ることはできますか？
  * はい。バージョンは `v1`, `v2` などで表示されます。

???
* Here are the answers to the review questions.

---
name: conclusion
# Thank You for Participating!
.center[![:scale 40%](images/vault_logo.png)]

### For more information please refer to the following links:
* https://www.vaultproject.io/docs/
* https://www.vaultproject.io/api/
* https://learn.hashicorp.com/vault

???
* Thank the students for their participation
* Share some Vault links

---
name: Feedback-Survey
# Workshop Feedback Survey
* Your feedback is important to us!
* The survey is short, we promise:
  * http://bit.ly/hashiworkshopfeedback

???
* Ask them to fill out the online survey
