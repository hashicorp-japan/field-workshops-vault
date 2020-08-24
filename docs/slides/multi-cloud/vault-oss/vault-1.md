name: chapter-1
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Chapter 1  
## HashiCorp Vault Overview

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???
Chapter 1 introduces Vault

---
name: hashiCorp-vault-overview
# HashiCorp Vault Overview
![:scale 10%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

  * HashiCorp Vaultは、API主導のクラウドに依存しない秘密管理システムです。
  * ハイブリッドクラウド環境でセンシティブなデータを安全に保存・管理することができます。
  * Vault を使用して、動的に短命のクレデンシャルを生成したり、アプリケーションのデータをその場で暗号化したりすることもできます。

???
This is meant as a high level overview.  For detailed descriptions or instructions please see the docs, API guide, or learning site:
* https://www.vaultproject.io/docs/
* https://www.vaultproject.io/api/
* https://learn.hashicorp.com/vault/

---
name: the-old-way
layout: false
# 従来のセキュリティモデル
.center[![:scale 70%](images/bodiam_castle.jpg)]

???
* This picture shows the traditional castle and moat security model.

---
layout: true

.footer[
- Copyright 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: traditional-security-models
# 従来のセキュリティモデル
* 従来のセキュリティモデルは、境界線に基づいたセキュリティの考えに基づいて構築されていました。
* ファイアウォールがあり、その中では安全だと思われていました。
* データベースなどのリソースは、ほとんどが静的なものでした。 そのようなルールは IP アドレスに基づいており、クレデンシャルはソースコードに組み込まれていたり、ディスク上の静的ファイルに保存されていました。

???
This slide discusses the traditional security model

---
name: problems-with-traditional-security-models
# 従来のセキュリティモデルの問題
* IPアドレスベースのルール
* 以下のような問題のあるハードコードされた資格情報。
  * アプリとユーザーの共有サービスアカウント
  * Rotationさせたり、破棄したり、誰がアクセスできるかを判断するのは難しい。
  * 危険なクレデンシャルを破棄する影響が大きい

???
* This slide describes some of the problems with the traditional security model.
---
name: the-new-way
layout: false
# モダンなシークレット管理
.center[![:scale 65%](images/nomadic_houses.jpg)]
.center[十分に定義された境界線がないため、アイデンティティによってセキュリティを強化します。

???
* These are Mongolian Yurts or "Ger" as they are called locally. Instead of a castle with walls and a drawbridge, a fixed fortress that has an inside and an outside, these people move from place to place, bringing their houses with them.

* And if you don't think the Nomadic way can be an effective security posture, think about this for a moment. The Mongol military tactics and organization enabled the Genghis Khan to conquer nearly all of continental Asia, the Middle East and parts of eastern Europe. Mongol warriors would typically bring three or four horses with them, so they could rotate through the horses and go farther. Mongol army units could move up to 100 miles a day, which was unheard of in the 13th century. They were faster, more adaptable, and more resilient than all their enemies.

---
name: identity-based-security-1
#Identity Based Security
.center[![:scale 75%](images/identity-triangle.png)]
.center[[Identity Based Security and Low Trust Networks](https://www.hashicorp.com/identity-based-security-and-low-trust-networks)
]

???
* Here we see that Vault has multiple means of authenticating users and applications with its Auth Methods.
* Vault can manage many types of secrets and excels at generating short-lived, dynmamic secrets.
* Vault's ACL policies are associated with tokens that users and applications use to access secrets after authenticating.
* Tokens can only read/write secrets that its policies allow.
* Click on the link to read a white paper about identity-based security in low trust networks.

---
layout: true

.footer[
- Copyright 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: identity-based-security-2
# Identity Based Security

Vaultは、現代のアプリケーションのセキュリティニーズに対処するために設計されました。 その使用方は、従来のアプローチとは異なります。

* アイデンティティベースのルールにより、ネットワークの境界を越えてセキュリティを拡張することができます。
* 頻繁にローテーションするダイナミックで短命なクレデンシャル
* 複数のアカウントでもエンティティによって単一ユーザーとして扱える
* 簡単に無効化できるクレデンシャルとエンティティ

???
* This slide discusses how Vault is designed for modern applications.

---
name: secrets-engines
layout: false
# Vault Secrets Engines
.center[![:scale 60%](images/vault-engines.png)]
.center[[Vault Secrets Engines](https://www.vaultproject.io/docs/secrets/)]

???
* Vault provides many out-of-the-box secrets engines.
* Additional custom secrets engines can be added by customers.
* Click on the link to learn more about Vault secrets engines.

---
name: vault-reference-architecture-1
# Vault Architecture Internals
.center[![:scale 75%](images/vault_arch.png)]
.center[[HashiCorp Vault Internals Architecture](https://www.vaultproject.io/docs/internals/architecture/)
]

???
* Click the link to learn more about the internal's of Vault's architecture.

---
name: vault-reference-architecture-2
# Vault Architecture - High Availability
.center[![:scale 60%](images/vault-ref-arch-lb.png)]
.center[[Vault High Availability](https://www.vaultproject.io/docs/concepts/ha/)
]

???
* Vault allows multiple servers to be combined in a highly available cluster within a single cloud region or physical data center.
* Click on the link to learn more about Vault's high availability in a single cluster.

---
name: vault-reference-architecture-3
# Vault Architecture - Multi-Region
.center[![:scale 70%](images/vault-ref-arch-replication.png)]
.center[[Vault Enterprise Replication](https://www.vaultproject.io/docs/enterprise/replication/)
]

???
* Vault Enterprise supports replication between clusters across regions and data centers.
* It supports Disaster Recovery and Performance replication.
* These can be used together.
* Click the link to learn more about Vault's replication.

---
layout: true

.footer[
- Copyright 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: chapter-1-review-answer
# Chapter 1 Review
* What is HashiCorp Vault?
 * Vaultはシークレット管理システムです。
  * API駆動で、クラウドに依存しません。
  * 信頼されていないネットワーク上での使用を想定して開発されています。
  * 多くのシステムに対応したユーザーやアプリケーションの認証が可能です。
  * 短命なシークレットの動的生成をサポートします。
  * Resionをまたいで複製可能な、高可用性のクラスタを構築します。

???
* Here are the answers to the review questions.
