---
description: These headers are sent via HTTP requests.
---

# Headers

## **Chain-Id**

For all the queries and mutations, you need to specify `Chain-Id` header in number \(ex: 1-mainnet, 42-kovan\). Since Shoyu NFT supports multi-chain, this chain id is used for distinguishing accounts and contracts in different chains.

## **Auth-Signature**

To identify a user's account, it requires `Auth-Signature`, the result of signing a special message `I'd like to sign in to Shōyu` according to EIP-191 standard.

### Using [ethers.js](https://docs.ethers.io/v5/single-page/#/v5/api/signer/-%23-Signer--signing-methods)

{% hint style="success" %}
#### Signing

`signer.signMessage( message ) ⇒ Promise< string<` [`RawSignature`](https://docs.ethers.io/v5/single-page/#/v5/api/utils/bytes/-%23-signature-raw) `> >`

This returns a Promise which resolves to the [Raw Signature](https://docs.ethers.io/v5/single-page/#/v5/api/utils/bytes/-%23-signature-raw) of message.

A signed message is prefixd with `"\x19Ethereum signed message:\n"` and the length of the message, using the [hashMessage](https://docs.ethers.io/v5/single-page/#/v5/api/utils/hashing/-%23-utils-hashMessage) method, so that it is [EIP-191](https://eips.ethereum.org/EIPS/eip-191) compliant. If recovering the address in Solidity, this prefix will be required to create a matching hash.

Sub-classes **must** implement this, however they may throw if signing a message is not supported, such as in a Contract-based Wallet or Meta-Transaction-based Wallet.Note

If message is a string, it is **treated as a string** and converted to its representation in UTF8 bytes.

**If and only if** a message is a [Bytes](https://docs.ethers.io/v5/single-page/#/v5/api/utils/bytes/-%23-Bytes) will it be treated as binary data.

For example, the string `"0x1234"` is 6 characters long \(and in this case 6 bytes long\). This is **not** equivalent to the array `[ 0x12, 0x34 ]`, which is 2 bytes long.

A common case is to sign a hash. In this case, if the hash is a string, it **must** be converted to an array first, using the [arrayify](https://docs.ethers.io/v5/single-page/#/v5/api/utils/bytes/-%23-utils-arrayify) utility function.

```javascript
// To sign a simple string, which are used for
// logging into a service, such as CryptoKitties,
// pass the string in.
signature = await signer.signMessage("Hello World");
// '0x3077d1b961d146d5a956e67495cbfcc2b6971c787690382ff85ef4403d96fee1625bb24fe54c69b628b6cb34d0a2cb3bdff10a635b66d76585db0dc378363c3c1c'

//
// A common case is also signing a hash, which is 32
// bytes. It is important to note, that to sign binary
// data it MUST be an Array (or TypedArray)
//

// This string is 66 characters long
message = "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"

// This array representation is 32 bytes long
messageBytes = ethers.utils.arrayify(message);
// Uint8Array [ 221, 242, 82, 173, 27, 226, 200, 155, 105, 194, 176, 104, 252, 55, 141, 170, 149, 43, 167, 241, 99, 196, 161, 22, 40, 245, 90, 77, 245, 35, 179, 239 ]

// To sign a hash, you most often want to sign the bytes
signature = await signer.signMessage(messageBytes)
```
{% endhint %}

### Using [web3.js](https://web3js.readthedocs.io/en/v1.2.11/web3-eth-accounts.html#sign)

{% hint style="success" %}
### sign

```java
web3.eth.accounts.sign(data, privateKey);
```

Signs arbitrary data.

#### Parameters

1. `data` - `String`: The data to sign.
2. `privateKey` - `String`: The private key to sign with.

_Note_

The value passed as the data parameter will be UTF-8 HEX decoded and wrapped as follows: `"\x19Ethereum Signed Message:\n" + message.length + message`.

#### Returns

`Object`: The signature object

* `message` - `String`: The the given message.
* `messageHash` - `String`: The hash of the given message.
* `r` - `String`: First 32 bytes of the signature
* `s` - `String`: Next 32 bytes of the signature
* `v` - `String`: Recovery value + 27

#### Example

```javascript
web3.eth.accounts.sign('Some data', '0x4c0883a69102937d6231471b5dbb6204fe5129617082792ae468d01a3f362318');
> {
    message: 'Some data',
    messageHash: '0x1da44b586eb0729ff70a73c326926f6ed5a25f5b056e7f47fbc6e58d86871655',
    v: '0x1c',
    r: '0xb91467e570a6466aa9e9876cbcd013baba02900b8979d43fe208a4a4f339f5fd',
    s: '0x6007e74cd82e037b800186422fc2da167c747ef045e5d18a5f5d4300f8e1a029',
    signature: '0xb91467e570a6466aa9e9876cbcd013baba02900b8979d43fe208a4a4f339f5fd6007e74cd82e037b800186422fc2da167c747ef045e5d18a5f5d4300f8e1a0291c'
}
```
{% endhint %}

