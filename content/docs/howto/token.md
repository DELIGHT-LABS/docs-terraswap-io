---
weight: 1
bookFlatSection: true
---

# Mint Your CW20 Token

## Introduction

This token contract is implemented under the CW20 standard and it fully supports terraswap feature.
Except any function of your token itself contains more than asset, we recommend to mint your own token by **instantiating this binary**, rather than developing your own.

---
> **NOTE**
>
> We strongly recommend to create by the pre-stored binary. (hereinafter, referred to as `CODE ID : 4` of Terra 2.0 & `CODE ID : 3` of Terra classic) \
> There are some advantages below:
>
> * These token, pair contract codes are well audited by foundation. Don't have to audit for yours additionally.
> * Now, the foundation lists your token only Token factory-created tokens & pairs.
> * You don't have to migrate your contract whenever Terra network perform an upgrade. Foundation helps you migrating and you don't have to do any action.
>
---

## How to mint

### 1. Using deployed token factory (recommended)

The standard CW20 token is already stored in Terra 2.0 network as code ID `4`, and Terra classic as `3`.\
The code is wrapped based on cw20 [v0.11.0](https://docs.rs/cw20/0.11.0/cw20/index.html) \
Please check [here]({{< relref "/docs/contract_resources/contract_addresses" >}}) for the more addresses.

You may instantiate your own token using the JSON as follows:

```json
{
    "name": "yout_token_name",
    "symbol": "SYMBOL",
    "decimals": 6,
    "initial_balances": [
        {
            "address": "terraaddress0001asdfsdfbqwer...",
            "amount": "10000"
        },
        {
            "address": "terraaddress0002asdfsdfbqwer...",
            "amount": "10000"
        },
        ...
    ]
}
```

Then, the CLI reads:

```bash
terrad tx wasm instantiate <token_bin_code> '{"name": "yout_token_name", "symbol": "SYMBOL", "decimals": 3, ... }' --from your_key
```

After confirmantion, your token is minted on terra network!

With your tx hash, you may query the tx:
```bash
terrad query tx EF2REFAWE234A2EFV....
```

Then, you may find the address of your contract from:
```json
{
    "key": "contract_address",
    "value": "terra18vd8fpwxzck93qlwghaj6arh4p7c5n896xzem5"
}
```

---
> **NOTE**
>
> You can easily do on [Terra station web application](https://station.terra.money/contract), too!
> Search by the contract address, input the organized JSON, and execute!
>
---

### 2. Implement yourself

First, you should implement at least these messages.

```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum Cw20ExecuteMsg {
    /// Transfer is a base message to move tokens to another account without triggering actions
    Transfer { recipient: String, amount: Uint128 },
    /// Burn is a base message to destroy tokens forever
    Burn { amount: Uint128 },
    /// Send is a base message to transfer tokens to a contract and trigger an action
    /// on the receiving contract.
    Send {
        contract: String,
        amount: Uint128,
        msg: Binary,
    },
    /// Only with "approval" extension. Allows spender to access an additional amount tokens
    /// from the owner's (env.sender) account. If expires is Some(), overwrites current allowance
    /// expiration with this one.
    IncreaseAllowance {
        spender: String,
        amount: Uint128,
        expires: Option<Expiration>,
    },
    /// Only with "approval" extension. Lowers the spender's access of tokens
    /// from the owner's (env.sender) account by amount. If expires is Some(), overwrites current
    /// allowance expiration with this one.
    DecreaseAllowance {
        spender: String,
        amount: Uint128,
        expires: Option<Expiration>,
    },
    /// Only with "approval" extension. Transfers amount tokens from owner -> recipient
    /// if `env.sender` has sufficient pre-approval.
    TransferFrom {
        owner: String,
        recipient: String,
        amount: Uint128,
    },
    /// Only with "approval" extension. Sends amount tokens from owner -> contract
    /// if `env.sender` has sufficient pre-approval.
    SendFrom {
        owner: String,
        contract: String,
        amount: Uint128,
        msg: Binary,
    },
    /// Only with "approval" extension. Destroys tokens forever
    BurnFrom { owner: String, amount: Uint128 },
    /// Only with the "mintable" extension. If authorized, creates amount new tokens
    /// and adds to the recipient balance.
    Mint { recipient: String, amount: Uint128 },
    /// Only with the "marketing" extension. If authorized, updates marketing metadata.
    /// Setting None/null for any of these will leave it unchanged.
    /// Setting Some("") will clear this field on the contract storage
    UpdateMarketing {
        /// A URL pointing to the project behind this token.
        project: Option<String>,
        /// A longer description of the token and it's utility. Designed for tooltips or such
        description: Option<String>,
        /// The address (if any) who can update this data structure
        marketing: Option<String>,
    },
    /// If set as the "marketing" role on the contract, upload a new URL, SVG, or PNG for the token
    UploadLogo(Logo),
}

#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum Cw20QueryMsg {
    /// Returns the current balance of the given address, 0 if unset.
    /// Return type: BalanceResponse.
    Balance { address: String },
    /// Returns metadata on the contract - name, decimals, supply, etc.
    /// Return type: TokenInfoResponse.
    TokenInfo {},
    /// Only with "allowance" extension.
    /// Returns how much spender can use from owner account, 0 if unset.
    /// Return type: AllowanceResponse.
    Allowance { owner: String, spender: String },
    /// Only with "mintable" extension.
    /// Returns who can mint and the hard cap on maximum tokens after minting.
    /// Return type: MinterResponse.
    Minter {},
    /// Only with "marketing" extension
    /// Returns more metadata on the contract to display in the client:
    /// - description, logo, project url, etc.
    /// Return type: MarketingInfoResponse.
    MarketingInfo {},
    /// Only with "marketing" extension
    /// Downloads the embedded logo data (if stored on chain). Errors if no logo data stored for
    /// this contract.
    /// Return type: DownloadLogoResponse.
    DownloadLogo {},
    /// Only with "enumerable" extension (and "allowances")
    /// Returns all allowances this owner has approved. Supports pagination.
    /// Return type: AllAllowancesResponse.
    AllAllowances {
        owner: String,
        start_after: Option<String>,
        limit: Option<u32>,
    },
    /// Only with "enumerable" extension
    /// Returns all accounts that have balances. Supports pagination.
    /// Return type: AllAccountsResponse.
    AllAccounts {
        start_after: Option<String>,
        limit: Option<u32>,
    },
}

/// We currently take no arguments for migrations
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct MigrateMsg {}
```

Then, compile your contract using it. 

```bash
docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/rust-optimizer:0.12.4

```

Because the size of WASM file consumes over 1 MiB and this contract cannot be deployed due to binary size restriction. (512KiB)
