# memo-did-spec
## Abstract

MEMO DID是基于区块链的去中心化，开放的跨链账户系统。MEMO DID为不同区块链的用户提供自主且全球唯一的账户，可用于管理数字资产，身份认证等场景。

本文档描述了一种全新的DID方法——MEMO DID，并描述了如何对MEMO DID文档进行CRUD操作。

## DID Format

MEMO DID的组成如下：

> **DID组成**
>
> ```http
> did:memo:<memo-specific-id>
> ```

一个DID例子如下：

> **DID用例**
>
> ```http
> did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e
> ```

其中<font color="red">`<memo-specific-id>`</font>=<font color="red">`hex(hash(address))`</font>

<font color="red">`<address>`</font>是以太坊地址。

## DID Url Format

MEMO DID URL组成如下：

>**DID URL组成**
>
>```http
>did:memo:<memo-specific-id> path [ ? <query> ] [ # <fragment> ]
>```

一些DID URL例子如下：

> **DID URL例子1**
>
> ```http
> did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e#masterKey
> ```

> **DID URL例子2**
>
> ```http
> did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e#key-1
> ```

支持以下path:

- 暂无

支持以下query：

- 暂无

支持以下fragment：

- <font color="red">`#masterKey`</font>：指定主公钥；
- <font color="red">`#key-<n>`</font>：指定其他的公钥；

## CRUD Operation

### Create

创建MEMO DID需要调用MEMO DID Server的接口，请查阅接口文档。此外，在创建MEMO DID之前，需要使用钱包创建公私钥并生成地址。创建MEMO DID的流程如下：

1. 调用MEMO DID Server的/did/create接口，传入区块链名词以及地址，接口会返回需要签名的消息；
2. 调用钱包的signMessage接口对消息进行签名；
3. 调用MEMO DID Server的/did/create/confirm接口，传入消息以及签名；

在完成上述三个步骤后，MEMO DID Server会生成交易并为您创建一个独属于您的MEMO DID。

### Read

MEMO DID Server提供了MEMO DID Resolver服务，可以通过MEMO DID Resolver解析得到MEMO DID文档。

一些DID文档的例子如下：

> **DID文档例子1**
>
> ```http
> {
> 	"@context": "https://www.w3.org/ns/did/v1",
> 	"id": "did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96",
> 	"verifycationMethod": [{
> 		"id": "did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#masterKey",
> 		"controller": "did:memo:0000000000000000000000000000000000000000000000000000000000000000",
> 		"type": "EcdsaSecp256k1VerificationKey2019",
> 		"publicKeyHex": "0x03d21e6c4843fa3f5d019e551131106e2075925b01da2a83dc177879a512eb608f"
>  }],
>  "authentication": [
>  	"did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#masterKey"
>  ],
>  "assertionMethod": [
>  	"did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#masterKey"
>  ],
>  "capabilityDelegation": [
>  	"did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#masterKey"
>  ],
>  "recovery": [
>  	"did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#masterKey"
>  ]
> }
> ```

> **DID文档例子2**
>
> ```http
> {
> 	"@context": "https://www.w3.org/ns/did/v1",
> 	"id": "did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96",
> 	"verifycationMethod": [{
> 		"id": "did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#masterKey",
> 		"controller": "did:memo:0000000000000000000000000000000000000000000000000000000000000000",
> 		"type": "EcdsaSecp256k1VerificationKey2019",
> 		"publicKeyHex": "0x03d21e6c4843fa3f5d019e551131106e2075925b01da2a83dc177879a512eb608f"
>  },
>  {
>  	"id": "did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#key-1",
>  	"controller": "did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96",
>  	"type": "EcdsaSecp256k1RecoveryMethod2020",
>  	"blockchainAccountId": "0x4Ca0Da3bF629F194e1A8c06A406442B6ac2169A5"
>  }],
>  "authentication": [
>  	"did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#masterKey"
>  ],
>  "assertionMethod": [
>  	"did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#masterKey"
>  ],
>  "capabilityDelegation": [
>  	"did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#key-1"
>  ],
>  "recovery": [
>  	"did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#masterKey"
>  ]
> }
> ```

### Update

修改MEMO DID文档信息同样需要调用MEMO DID Server的接口。其流程如下：

1. 调用MEMO DID Server的/did/update接口，传入需要修改的信息，接口会返回需要签名的消息；
2. 调用钱包的signMessage接口对消息进行签名，需要使用masterKey对应的私钥进行签名；
3. 调用MEMO DID Server的/did/update/confirm接口，传入消息以及签名；

在完成上述三个步骤后，MEMO DID Server会生成交易并为您修改MEMO DID文档的信息。

### Delete

删除MEMO DID同样需要调用MEMO DID Server的接口。其流程如下：

1. 调用MEMO DID Server的/did/delete接口，传入需要删除的DID，接口会返回需要签名的消息；
2. 调用钱包的signMessage接口对消息进行签名，需要使用masterKey对应的私钥进行签名；
3. 调用MEMO DID Server的/did/delete/confirm接口，传入消息以及签名；

在完成上述三个步骤后，MEMO DID Server会生成交易并为您修改MEMO DID文档的信息。

