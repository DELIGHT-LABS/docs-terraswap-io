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
> We strongly recommend to create by the pre-stored binary. (hereinafter, referred to as `Token factory`) \
> There are some advantages below:
>
> * These token, pair contract codes are well audited by foundation. Don't have to audit for yours additionally.
> * Now, the foundation lists your token only Token factory-created tokens & pairs.
> * You don't have to migrate your contract whenever Terra network perform an upgrade. Foundation helps you migrating and you don't have to do any action.
>
---

## How to mint

### 1. Using Token factory (recommended)

The standard CW20 token is already stored in Terra network as code ID `3`.\
Please check [here]({{< relref "/docs/contract_resources/contract_addresses" >}}) for the more addresses.

You may instantiate your own token using the JSON as follows:

```json
{
    "name": "yout_token_name",
    "symbol": "SYMBOL",
    "decimals": 3,
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

### 2. Implement yourself

First, you should implement at least these messages.

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    /// Transfer is a base message to move tokens to another account without triggering actions
    Transfer {
        recipient: HumanAddr,
        amount: Uint128,
    },
    /// Burn is a base message to destroy tokens forever
    Burn { amount: Uint128 },
    /// Send is a base message to transfer tokens to a contract and trigger an action
    /// on the receiving contract.
    Send {
        contract: HumanAddr,
        amount: Uint128,
        msg: Option<Binary>,
    },
    /// Only with the "mintable" extension. If authorized, creates amount new tokens
    /// and adds to the recipient balance.
    Mint {
        recipient: HumanAddr,
        amount: Uint128,
    },
    /// Only with "approval" extension. Allows spender to access an additional amount tokens
    /// from the owner's (env.sender) account. If expires is Some(), overwrites current allowance
    /// expiration with this one.
    IncreaseAllowance {
        spender: HumanAddr,
        amount: Uint128,
        expires: Option<Expiration>,
    },
    /// Only with "approval" extension. Lowers the spender's access of tokens
    /// from the owner's (env.sender) account by amount. If expires is Some(), overwrites current
    /// allowance expiration with this one.
    DecreaseAllowance {
        spender: HumanAddr,
        amount: Uint128,
        expires: Option<Expiration>,
    },
    /// Only with "approval" extension. Transfers amount tokens from owner -> recipient
    /// if `env.sender` has sufficient pre-approval.
    TransferFrom {
        owner: HumanAddr,
        recipient: HumanAddr,
        amount: Uint128,
    },
    /// Only with "approval" extension. Sends amount tokens from owner -> contract
    /// if `env.sender` has sufficient pre-approval.
    SendFrom {
        owner: HumanAddr,
        contract: HumanAddr,
        amount: Uint128,
        msg: Option<Binary>,
    },
    /// Only with "approval" extension. Destroys tokens forever
    BurnFrom { owner: HumanAddr, amount: Uint128 },
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    /// Returns the current balance of the given address, 0 if unset.
    /// Return type: BalanceResponse.
    Balance { address: HumanAddr },
    /// Returns metadata on the contract - name, decimals, supply, etc.
    /// Return type: TokenInfoResponse.
    TokenInfo {},
    /// Only with "mintable" extension.
    /// Returns who can mint and how much.
    /// Return type: MinterResponse.
    Minter {},
    /// Only with "allowance" extension.
    /// Returns how much spender can use from owner account, 0 if unset.
    /// Return type: AllowanceResponse.
    Allowance {
        owner: HumanAddr,
        spender: HumanAddr,
    },
    /// Only with "enumerable" extension (and "allowances")
    /// Returns all allowances this owner has approved. Supports pagination.
    /// Return type: AllAllowancesResponse.
    AllAllowances {
        owner: HumanAddr,
        start_after: Option<HumanAddr>,
        limit: Option<u32>,
    },
    /// Only with "enumerable" extension
    /// Returns all accounts that have balances. Supports pagination.
    /// Return type: AllAccountsResponse.
    AllAccounts {
        start_after: Option<HumanAddr>,
        limit: Option<u32>,
    },
}

/// We currently take no arguments for migrations
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct MigrateMsg {}
```

Then, compile your contract using it. 

```bash
RUSTFLAGS='-C link-arg=-s' cargo wasm
```

If you miss `RUSTFLAGS='-C link-arg=-s'`, the size of WASM file consumes over 1 MiB and this contract cannot be deployed due to binary size restriction. (512KiB)
