name: chapter-5
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Chapter 5      
## Vault Authentication Methods

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???

* Chapter 5 introduces Vault authentication methods
* It focuses on the Userpass method.

---
layout: true

.footer[
- Copyright 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: vault-auth-methods
# Vault Authentication Methods
.center[![:scale 45%](images/vault_auth_methods.png)]
.center[Vaultは多くのAuth methodをサポートしています。]

???
* Auth methods are how your apps and users verify their identity.
* In the same way you might present some kind of valid ID at the hotel check-in desk, users and apps provide some kind of credential or token to authenticate.
* You can enable multiple auth methods and multiple instances of the same auth method.

---
name:vault-auth-methods-2
# いくつかのVault Auth Methods

<div style="float: left; width: 50%;">
<u>ユーザー用のMethods</u>
<ul>
<li>Userpass</li>
<li>GitHub</li>
<li>LDAP</li>
<li>JWT/OIDC</li>
<li>Okta</li>
</ul>
</div>
<div style="float: right; width: 50%;">
<u>アプリケーション用のMethods</u>
<ul>
<li>AppRole</li>
<li>AWS</li>
<li>Azure</li>
<li>Google Cloud</li>
<li>Kubernetes</li>
</ul>
</div>

???
* Userpass - Allows users to authenticate with username and password managed by Vault
* GitHub - Allows users to authenticate with their GitHub personal access tokens
* LDAP - Allows users to authenticate against an LDAP server with their username and password managed by that server.
* JWT/OIDC - Allows users to authenticate against an external OpenID Connect provider or with JSON Web Tokens (JWTs)
* Okta - Allows users to authenticate using Okta single sign-on.
* AppRole - Allows applications to authenticate in automated workflows using a role and a role ID.
* AWS - Allows applications on AWS EC2 instances and Lambda functions to authenticate with IAM credentials or EC2 metadata.
* Azure - Allows applications associated with Azure Managed Service Identities to authenticate using Azure Active Directory credentials.
* Google Cloud - Allows applications in GCP to authenticate using Google Cloud IAM service accounts or Google Compute Engine (GCE) metadata.
* Kubernetes - Allows Kubernetes pods to authenticate with JWT tokens.

---
name: enabling-auth-methods
# Authentication Methodsの有効化

* Vault auth methodsも明示的に有効化します。
	* `vault auth enable`で行います。
* 各auth methodはデフォルトのPathがあります。
* 別のPathも設定可能です。
	* `vault auth enable -path=aws-east aws`
* カスタムPathへCLIやAPIでアクセスします。
	* カスタムパス： `vault write aws-east/config/root`
	* デフォルト： `vault write aws/config/root`

???

* Talk about enabling auth methods.
* Talk about default and custom paths
* Explain the examples

---
name: userpass-0
# Userpass Auth Method
.center[![:scale 30%](images/userpass_login.png)]
* Userpass メソッドは、Vault が管理するユーザー名とパスワードでユーザーを認証します。

???
* The Userpass method allows users to authenticate with username and password managed by Vault.
* It is not recommended for production, but it's fine for development and lab environments.
* In the real world you'd probably have Vault use your Active Directory, LDAP, GitHub, or other system of record for authentication by users.

---
name: chapter-5-review-answers
# Chapter 5 Review

* Vault は、どのような種類のエンティティを認証できますか？
  * ユーザーおよびアプリケーション
* Userpass auth メソッドの認証情報を管理するシステムは何ですか?
  * Vault
* デフォルトポリシー以外のポリシーが割り当てられていないユーザーは、シークレットにアクセスできますか?
  * いいえ


???
* Here are the answers to the review questions.
