name: chapter-6
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Chapter 6      
## Vault Policies

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???

* Chapter 6 introduces Vault ポリシー

---
layout: true

.footer[
- Copyright 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: vault-policies
# Vault ポリシー
* Vault ポリシーは、ユーザーおよびアプリケーションがアクセスできるシークレットを制限します。
* デフォルトでアクセスを拒否し、最小特権の慣行に従っています。
* Vault 管理者は、ポリシーステートメントを使用して、ユーザーとアプリケーションに特定のパスへのアクセスを明示的に許可する必要があります。
* ポリシーは、パスを指定するだけでなく、そのパスの機能セットも指定します。
* ポリシーは、HashiCorp Configuration Language（HCL）で記述されます。

---
name: vault-policy-example
# ポリシーの例
* ポリシー
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
# ポリシーのPathとCapabilities
* ポリシーのPathは、Vault API Pathにマップされます。
* 最も一般的な機能は、`create`、`read`、`update`、`delete`、`list`です。POSTやGETなどのHTTP動詞に対応します。
* 特殊な権限：
  * `sudo` は root で保護されたパスへのアクセスを許可します。
  * `deny` はPathへのアクセスを拒否し、他の機能よりも優先されます。



???
* Explain policy paths and capabilities

---
name: policies-for-lobs
# 管理者向けのポリシー
* 多くの組織では、管理者とユーザーで異なるポリシーを使います。
* ここでは、業務A、部門1の管理者のポリシーの例を示します。

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
# Vault PolicyのCLIコマンド
* Vault のポリシーは、Vault の CLI、UI、または API を使用して、Vault サーバに追加できます。
* CLIでポリシーを追加するコマンドは、`vault policy write`です。
* 以下は、HCLファイル "lob-A-dept-1-policy.hcl"から "lob-A-dept-1 "というポリシーを作成するコマンドです。
	* `vault policy write lob-A-dept-1 lob-A-dept-1-policy.hcl`
* このポリシーをUserpass認証のユーザーに関連付けるコマンドは以下の通りです。
	* `vault write auth/userpass/users/joe/policies policies=lob-A-dept-1`

???
Describe the most important Vault CLI commands for policies.

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
