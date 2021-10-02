# Queries

## accounts

It returns the addresses of `admin`, `proxy` and `staff`

## me

It returns my information identified by `Chain-Id` and authenticated by `Auth-Signature`.

## **user\(address\)**

It returns a specific user's information identified by the id `{Chain-Id}:{address}`.

## **nftContracts\(owner\)**

It returns contracts that are being deployed or were already deployed that belong to `owner`.

## **nfts\(contract: Address, tokenId: Uint256, parked: Boolean, owner: Address, limit: Int, offset: Int\)**

It returns nfts that were deployed, filtered by `contract` address, `tokenId`, `parked`, `owner` and paginated by `limit` and `offset`.

