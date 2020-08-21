name: chapter-6
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Chapter 6      
## Vault Policies

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???

* Chapter 6 introduces Vault Policies

---
layout: true

.footer[
- Copyright 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: vault-policies
# Vault Policies
* Vault ポリシーは、ユーザーおよびアプリケーションがアクセスできるシークレットを制限します。
* デフォルトでアクセスを拒否し、最小特権の慣行に従っています。
* Vault 管理者は、ポリシーステートメントを使用して、ユーザーとアプリケーションに特定のパスへのアクセスを明示的に許可する必要があります。
* ポリシーは、パスを指定するだけでなく、そのパスの機能セットも指定します。
* ポリシーは、HashiCorp Configuration Language（HCL）で記述されます。

---
name: vault-policy-example
# A Vault Policy Example
* Example of a Vault policy:
```hcl
# Allow tokens to look up their own properties
path "auth/token/lookup-self" {
    capabilities = ["read"]
}
```
* このポリシーでは、トークンのプロパティを変更することはできません（Read only）。

???
* This policy allows tokens to look up their own properties

---
name: vault-policy-paths-capabilities
# Policy Paths and Capabilities
* ポリシーのパスは、Vault API パスにマップされます。
* 最も一般的な機能は、`create`、`read`、`update`、`delete`、`list`です。POSTやGETなどのHTTP動詞に対応します。
* 特殊な権限：
  * `sudo` は root で保護されたパスへのアクセスを許可します。
  * `deny` はパスへのアクセスを拒否し、他の機能よりも優先されます。



???
* Explain policy paths and capabilities

---
name: policies-for-lobs
# Configuring Policies for LOBs
* 多くの組織では、ビジネスライン（LOB）と部門別にVaultのシークレットを管理しています。
* ここでは、業務Aライン、部門1のポリシーの例を示します。

```hcl
path "lob_a/dept_1/*" {
    capabilities = ["read", "list", "create", "delete", "update"]
}
```

* このポリシーは、グロブ文字 (`*`) を使用して `lob_a/dept_1/` の下にマウントされたすべてのシークレットに、すべての標準機能を付与します。


???
* Talk about how many organizations organize Vault secrets by line of business and department.
* Explain the policy including the glob character and that it can only be used at the end of a path.

---
name: vault-policy-commands
# Vault Policy CLI Commands
* Vault のポリシーは、Vault の CLI、UI、または API を使用して、Vault サーバに追加できます。
* CLIでポリシーを追加するコマンドは、`vault policy write`です。
* 以下は、HCLファイル "lob-A-dept-1-policy.hcl"から "lob-A-dept-1 "というポリシーを作成するコマンドです。
	* `vault policy write lob-A-dept-1 lob-A-dept-1-policy.hcl`
* このポリシーをUserpassユーザーに関連付けるコマンドは以下の通りです。
	* `vault write auth/userpass/users/joe/policies policies=lob-A-dept-1` です。

???
Describe the most important Vault CLI commands for policies.

---
name: lab-vault-basics-challenge-7
# Lab Challenge 6.1: Vault Policies
* In this lab, you'll use Vault policies to grant different users access to different secrets.
* Instructions:
  * Click the "Use Vault Policies" challenge of the "Vault Basics" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* Instruct the students to do the "Use Vault Policies" challenge of the "Vault Basics" track.
* This challenge has them create a second user and create and associate policies with their 2 users.
* It then has them valdiate that each user can only access their own secrets.

---
name: chapter-6-review-questions
# Chapter 6 Review
* Vault は、デフォルトでシークレットへのアクセスを許可しますか？
* HTTP の動詞に対応するポリシー機能は何ですか？
* Vault にポリシーを追加するために使用できる CLI コマンドは何ですか？

???
* Let's review what we learned in this chapter.

---
name: chapter-6-review-answers
# Chapter 6 Review

* Vault は、デフォルトでシークレットへのアクセスを許可しますか？
  * いいえ
* HTTP の動詞に対応するポリシー機能は何か?
  * `create`, `read`, `update`, `delete`, `list` の4つの機能があります。
* Vaultにポリシーを追加するために使用できるCLIコマンドは何ですか?
  * `vault policy write` 

???
* Here are the answers to the review questions.
