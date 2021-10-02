# Mutations

## **sync**

This can only be called by the staff account, derived from `STAFF_PRIVATE_KEY` in `.env` and authenticated by `Auth-Signature`. It starts syncing all the relevant events from blockchains, with 4 blocks of delay \(to avoid reorg issues\).

If you call `deployNFTContract()` mutation, wait for 4 blocks and then hit this mutation, you'll see that the corresponding contract information is stored in the database.

## **signUp\(input\)**

It creates a new user record in the database with the id of `{Chain-Id}:{auth.account}` authenticated by `Auth-Signature`.

## **updateMe\(input\)**

It updates the information of a user identified the id `{Chain-Id}:{auth.account}` authenticated by `Auth-Signature`.

## **deployNFTContract\(input\)**

It creates a new contract record in the database with the id of `{Chain-Id}:{address}` and the `address` is automatically calculated.

If you specify `input.toTokenId`, it'll deploy the contract and mint all the NFTs with `tokenId`s upto `input.toTokenId`. Otherwise you can set `input.tokenIds` \(and `input.amounts` for ERC1155\) so that an NFT for each `tokenId` is minted.

Like mentioned above, it takes some time to sync. After calling this mutation, wait for 4 blocks and call `sync` and you'll be able to query the contract deployed and NFTs minted.

## **updateNFTs\(input\)**

This updates information of multiple NFTs at once identified the id of `{Chain-Id}:{input.contract}:{input.tokenIds}`. It includes metadata such as `name`, `description` and `image`.

## ask\(inputs\)

You can submit multiple ask orders \(for selling NFTs\). Each input field consists like:

```graphql
input AskInput {
    exchange: Address!
    signer: Address!
    token: Address!
    tokenId: Uint256!
    amount: Uint256!
    strategy: Address!
    currency: Address!
    recipient: Address!
    deadline: Int!
    params: Bytes!
    v: Int!
    r: Bytes!
    s: Bytes!
}
```

* **exchange**: The exchange address to execute the trade. _\(For NFTs that were deployed natively in shoyu, you can use the NFT contract's address because themselves are exchanges.\)_
* **signer**: seller address
* **token**: NFT contract address
* **tokenId**: unique id of the NFT
* **amount**: for NFT721 it must be 1 and for NFT1155 it can be &gt;= 1
* **strategy**: one of the strategy contract address that represent selling strategies \(Refer to 

  [contracts](https://github.com/sushiswap/shoyu/tree/master/contracts/strategies)\)

* **currency**: any ERC20 token \(including WETH\)
* **recipient**: if you leave this blank\(0x000000000000000000000000000000\) then the `signer` gets the funds
* **deadline**: unix timestamp that the auction will end
* **params**: encoded params using [@shoyunft/sdk.js](https://www.npmjs.com/package/@shoyunft/sdk.js)
* **v, r, s**: the signature you can get using [@shoyunft/sdk.js](https://www.npmjs.com/package/@shoyunft/sdk.js) signed by `signer`

{% hint style="info" %}
When signing the ask order, you must use `proxy` address that you can fetch with [`accounts` query](queries.md#accounts).
{% endhint %}

## bid\(inputs\)

You can submit multiple bids \(for buying NFTs\) with existing `askHash`es. Each input field consists like:

```graphql
input BidInput {
    askHash: Bytes!
    signer: Address!
    amount: Uint256!
    price: Uint256!
    recipient: Address!
    referrer: Address!
    v: Int!
    r: Bytes!
    s: Bytes!
}
```

* **askHash**: The hash of the ask order you can get using [@shoyunft/sdk.js](https://www.npmjs.com/package/@shoyunft/sdk.js)
* **signer**: buyer address
* **amount**: for NFT721 it must be 1 and for NFT1155 it can be &gt;= 1
* **price**: the amount of tokens \(identical to `ask.currency`\) to bid in wei
* **recipient**: if you leave this blank\(0x000000000000000000000000000000\) then the `signer` gets the NFTs
* **referrer**: to be used later..
* **v, r, s**: the signature you can get using [@shoyunft/sdk.js](https://www.npmjs.com/package/@shoyunft/sdk.js) signed by `signer`

