# memo-did-spec

## Abstract

MEMO DID is a blockchain-based, decentralized, and open cross-chain account system. It provides users across different blockchains with autonomous and globally unique accounts, which can be used for managing digital assets, identity authentication, and other scenarios.

This document describes a novel DID method—MEMO DID—and outlines how to perform CRUD operations on MEMO DID documents.

## DID Format

The composition of a MEMO DID is as follows:

> **DID Composition**
>
> ```
> did:memo:<memo-specific-id>  
> ```

An example of a DID is:

> **DID Example**
>
> ```
> did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e  
> ```

Where `<memo-specific-id>` = `hex(hash(address))`.

`<address>` is an Ethereum address.

## DID URL Format

The composition of a MEMO DID URL is as follows:

> **DID URL Composition**
>
> ```
> did:memo:<memo-specific-id> path [ ? <query> ] [ # <fragment> ]  
> ```

Some examples of DID URLs are:

> **DID URL Example 1**
>
> ```
> did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e#masterKey  
> ```

> **DID URL Example 2**
>
> ```
> did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e#key-1  
> ```

Supported paths:

- None currently.

Supported queries:

- None currently.

Supported fragments:

- `#masterKey`: Specifies the master public key.
- `#key-<n>`: Specifies other public keys.

## CRUD Operation

### Create

Creating a MEMO DID requires calling the MEMO DID Server's API. Please refer to the API documentation. Additionally, before creating a MEMO DID, you need to use a wallet to generate a public-private key pair and derive an address. The process for creating a MEMO DID is as follows:

1. Call the `/did/create` endpoint of the MEMO DID Server, passing the blockchain name and address. The endpoint will return a message to be signed.
2. Call the wallet's `signMessage` function to sign the message.
3. Call the `/did/create/confirm` endpoint of the MEMO DID Server, passing the message and the signature.

After completing these three steps, the MEMO DID Server will generate a transaction and create a unique MEMO DID for you.

### Read

The MEMO DID Server provides a MEMO DID Resolver service, which can be used to resolve and retrieve MEMO DID documents.

Some examples of DID documents are:

> **DID Document Example 1**
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
> }],
> "authentication": [
> 	"did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#masterKey"
> ],
> "assertionMethod": [
> 	"did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#masterKey"
> ],
> "capabilityDelegation": [
> 	"did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#masterKey"
> ],
> "recovery": [
> 	"did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#masterKey"
> ]
> }
> ```

> **DID Document Example 2**
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
> },
> {
> 	"id": "did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#key-1",
> 	"controller": "did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96",
> 	"type": "EcdsaSecp256k1RecoveryMethod2020",
> 	"blockchainAccountId": "0x4Ca0Da3bF629F194e1A8c06A406442B6ac2169A5"
> }],
> "authentication": [
> 	"did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#masterKey"
> ],
> "assertionMethod": [
> 	"did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#masterKey"
> ],
> "capabilityDelegation": [
> 	"did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#key-1"
> ],
> "recovery": [
> 	"did:memo:d687daa192ffa26373395872191e8502cc41fbfbf27dc07d3da3a35de57c2d96#masterKey"
> ]
> }
> ```

### Update

Modifying a MEMO DID document also requires calling the MEMO DID Server's API. The process is as follows:

1. Call the `/did/update` endpoint of the MEMO DID Server, passing the information to be updated. The endpoint will return a message to be signed.
2. Call the wallet's `signMessage` function to sign the message, using the private key corresponding to the `masterKey`.
3. Call the `/did/update/confirm` endpoint of the MEMO DID Server, passing the message and the signature.

After completing these three steps, the MEMO DID Server will generate a transaction and update the MEMO DID document for you.

### Delete

Deleting a MEMO DID also requires calling the MEMO DID Server's API. The process is as follows:

1. Call the `/did/delete` endpoint of the MEMO DID Server, passing the DID to be deleted. The endpoint will return a message to be signed.
2. Call the wallet's `signMessage` function to sign the message, using the private key corresponding to the `masterKey`.
3. Call the `/did/delete/confirm` endpoint of the MEMO DID Server, passing the message and the signature.

After completing these three steps, the MEMO DID Server will generate a transaction and delete the MEMO DID document for you.
