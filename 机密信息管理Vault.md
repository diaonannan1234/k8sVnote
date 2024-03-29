# 机密信息管理

##  机密信息问题

```
数据库凭证和API密钥就是我们需要以安全的方式存储和提供给我们的应用程序的机密信息。
一个简单的解决方案是将这些信息存储在配置文件中，并在启动时读取它们。但是这种方法的问题显而易见：有权访问此文件的人共享我们的应用程序具有的数据库权限 - 通常可以完全访问所有存储的数据。

我们可以尝试通过加密这些文件来使事情变得更加困难。但是，这种方法在整体安全性方面不会增加太多。主要是因为我们的应用程序必须能够访问主密钥。当以这种方式使用时，加密仅是一种“错误”的安全感。

现代应用程序和云环境往往会增加一些额外的复杂性：分布式服务，多个数据库，消息传递系统等等，所有敏感信息都在各处传播，从而增加了安全漏洞的风险。

```

## Hashicorp提供的Vault解决思路
```
Vault的架构非常简单。其主要组成部分是：

* 持久性后端 —— 存储所有机密
* 一种API服务器 —— 用于处理客户端请求并对机密执行操作
* 许多secret引擎 —— 支持不同的机密类型
通过将所有机密处理委派给Vault，我们可以缓解一些安全问题：
我们的应用程序不再需要存储它们 ，只需在需要时询问Vault并在使用后将其丢弃
我们可以短暂的使用机密数据，从而限制攻击者盗取秘密的“机会之窗”

Vault会在将所有数据写入存储之前使用加密密钥对所有数据进行加密。此加密密钥由另一个密钥加密 —— 主密钥，主密钥仅在启动时使用。

Vault实现的一个关键点是它不会将主密钥存储在服务器中。 这意味着即使Vault也无法在启动后访问其保存的数据。 此时，Vault实例被称为处于“密封”状态。

```
### Vault 认证
```
要访问Vault中的机密，客户端需要使用一种受支持的方法对自身进行身份验证。最简单的方法使用Tokens，它只是使用特殊HTTP头在每个API请求上发送的字符串。

最初安装时，Vault会自动生成“根令牌”。此令牌与Linux系统中的超级用户等效，因此应将其使用限制在最低限度。作为最佳实践，我们应该使用此根令牌来创建具有较少权限的其他令牌，然后撤消它。但这不是问题，因为我们以后可以使用unseal键生成另一个根令牌。

Vault还支持其他身份验证机制，如LDAP，JWT，TLS证书等。所有这些机制都建立在基本令牌机制之上：一旦Vault验证了我们的客户端，它将提供一个令牌，然后我们可以使用它来访问其他API。

```
### Vault Secret类型
```
Vault支持一系列不同的秘密类型，可以解决不同的用例：

Key-Value： 简单的静态键值对
动态生成的凭据：由Vault根据客户端请求生成
加密密钥：用于使用客户端数据执行加密功能
每种秘密类型由以下属性定义：

一个挂载点，定义了它的REST API前缀
通过相应的API公开的一组操作
一组配置参数

```

### Vault Key-Value Secrets
```
Vault支持三种Key-Value机密：

非版本化键值对，其中更新替换现有值
版本化键值对，可保持可配置数量的旧版本
Cubbyhole，一种特殊类型的非版本化密钥对，其值的范围限定为给定的访问令牌（稍后将详细介绍）。
密钥值秘密本质上是静态的，因此没有到期的概念与它们相关联。这种秘密的主要使用场景是存储凭证以访问外部系统，例如API密钥。

```
### Vault 动态生成的Secrets
```
当应用程序请求时，Vault会动态生成动态机密。Vault支持多种类型的动态机密，包括以下几种：

数据库凭证
SSH密钥对
X.509证书
AWS凭证
Google Cloud服务帐户
Active Directory帐户
所有这些都遵循相同的使用模式

```
### Vault 加密密钥
```
这个类型的秘密引擎处理加密，解密，签名等加密功能。所有这些操作都使用Vault内部生成和存储的加密密钥。除非明确告知，否则Vault将永远不会公开给定的加密密钥。
```