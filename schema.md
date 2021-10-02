# Schema

Below is the the entire schema of the Shoyu GraphQL server.

```graphql
    type Query {
        accounts: Accounts!
        user(address: Address!): User
        me: User
        nftContracts(owner: Address): [NFTContract!]!
        nfts(contract: Address, tokenId: Uint256, parked: Boolean, owner: Address, limit: Int, offset: Int): [NFT!]!
    }

    type Mutation {
        sync: SyncContext
        signUp(input: UserInput!): User
        updateMe(input: UserInput!): User
        deployNFTContract(input: DeployNFTContractInput!): NFTContract
        updateNFTs(input: UpdateNFTsInput!): [NFT!]
        ask(inputs: [AskInput!]!): [Ask!]
        bid(inputs: [BidInput!]!): [Bid!]
    }

    type SyncContext {
        chainId: Int!
    }

    type Accounts {
        admin: Address!
        proxy: Address!
        staff: Address!
    }

    type User {
        id: ID! # chainId:address
        chainId: Int!
        address: Address!
        username: String
        name: String
        email: String
        bio: String
        profilePicture: String
        website: String
        discord: String
        telegram: String
        twitter: String
        facebook: String
        instagram: String
        tiktok: String
        youtube: String
        twitch: String
    }

    enum NFTStandard {
        ERC721
        ERC1155
    }

    type NFTContract {
        id: ID! # chainId:address
        chainId: Int!
        address: Address!
        standard: NFTStandard!
        owner: Address!
        name: String
        symbol: String
        royaltyFeeRecipient: Address!
        royaltyFee: Int!
        txHash: String
        confirmed: Boolean!
    }

    type NFT {
        id: ID! # contract.chainId:contract.address:tokenId
        contract: NFTContract!
        tokenId: Uint256!
        parked: Boolean
        name: String
        description: String
        image: String
        owners: [NFTOwner!]
    }

    type NFTOwner {
        owner: Address!
        balance: Int!
    }

    type SocialToken {
        id: ID! # chainId:address
        chainId: Int!
        address: Address!
        owner: Address!
        name: String
        symbol: String
        dividendToken: Address!
    }

    type SocialTokenWithdrawRecord {
        id: ID! # txHash:index
        token: SocialToken!
        to: Address!
        amount: Uint256!
    }

    type BurnRecord {
        id: ID! # txHash:index
        contract: Address!
        tokenId: Uint256!
        amount: Uint256!
        label: Uint256!
        data: Bytes!
    }

    type Ask {
        id: ID!
        chainId: Int!
        data: AskData!
        hash: Bytes!
        status: AskStatus!
        exchange: Address!
        signer: User!
        currentBid: Bid
    }

    enum AskStatus {
        Submitted
        Cancelled
        Executed
    }

    type AskData {
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
        r: String!
        s: String!
    }

    type Bid {
        id: ID!
        chainId: Int!
        data: BidData!
        hash: Bytes!
        ask: Ask!
        signer: User!
        status: BidStatus!
        txHash: String
        confirmed: Boolean!
    }

    enum BidStatus {
        Submitted
        Executed
    }

    type BidData {
        askHash: Bytes!
        signer: Address!
        amount: Uint256!
        price: Uint256!
        recipient: Address!
        referrer: Address!
        v: Int!
        r: String!
        s: String!
    }

    input DeployNFTContractInput {
        standard: NFTStandard!
        owner: Address!
        name: String
        symbol: String
        toTokenId: Int
        tokenIds: [Int!]
        amounts: [Int!]
        royaltyFeeRecipient: Address!
        royaltyFee: Int! # out of 250(25%)
    }

    input UpdateNFTsInput {
        contract: Address!
        tokenIds: [Int!]!
        names: [String!]!
        descriptions: [String!]!
        images: [String!]!
    }

    input UserInput {
        username: String
        name: String
        email: String
        bio: String
        profilePicture: String
        website: String
        discord: String
        telegram: String
        twitter: String
        facebook: String
        instagram: String
        tiktok: String
        youtube: String
        twitch: String
    }

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

