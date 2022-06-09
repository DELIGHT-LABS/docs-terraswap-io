---
weight: 3
bookFlatSection: true
---

# Swap

Swap in Terraswap works as same as trade in other exchanges. Price of centralized exchanges for both stock and cryptocurrency move by price bidding system, and supply & demand. These prices change in millisecond intervals, so real-time transaction is maintained in the market. But if anyone is trying to implement an exchange with the bidding-based pricing on blockchain, immediate execution of transaction becomes impossible due to block time. 

Therefore, a different pricing approach is used in Terraswap - algorithmic pricing by tracking the ratio of the paired asset within the liquidity pool. To learn more about this approach, please refer to [mechanism section]({{< relref "/docs/introduction/mechanism" >}}).

## Prerequisites

- Token & pair should be deployed
- You should know the addresses of token & pair.

## Swap by using CLI

### When Offer_asset is native & IBC token

By command line, all transactions can be executed in this way:

```bash
terrad tx wasm execute <contract-address> <handle-msg> <coins>
```

- `contract-address`: In swap transaction, this should be the pair address.
- `handle-msg`: It represents the method and parameters of this execution. Will explain below.
- `coins`: Fee to execute transaction

Enter `contract-address`, `coins` and `handle-msg`. To learn more about the general rules for `handle-msg` please refer to this [link]({{< relref "/docs/howto/query" >}}).

```json
{
    "swap": {
        "offer_asset": {
            "info" : {
                "native_token": {
                    "denom": "uluna"
                }
            },
            "amount": "10"
        },
        "to": "<HumanAddr>"
    }
}
```

`swap.offer_asset` represents your source asset. Please make sure that `swap.offer_asset.amount` is not same as your amount. It depends on the decimal of the token setting. In case of Luna, its decimal is 6. Then, if `swap.offer_asset.amount` reads `10`, the actual amount is `10 x 10^-6`. So, you should multiply with mathcing value, `10^(decimal)`.

`swap.to` is the destination token address. It is unnecessary to enter the amount to swap into, since Terraswap price is calculated algorithmically. 

After filling it out, you may choose to change it into an inline string (not necessary if you can make it with multiline):

```bash
'{"swap":{"offer_asset": {"info" : {"native_token": {"denom": "uluna"}},"amount": "10"},"to": "<HumanAddr>",}}'
```

This is your `handle-msg`. The `handle-msg` can be used to complete the CLI command to swap tokens. 

### When offer_asset is contract-minted token

Swapping contract-minted token to native token is executed with the same logic as above, but requires a differnet `handle-msg` due to difference in token system and its implementation. Message looks like:

```json
{
    "send": {
        "contract": "<HumanAddr>",
        "amount": "10",
        "msg": Base64({
            "swap": {}
        })
    }
}
```

The method is a little bit tricky. Please do not lose your concentration!

```bash
terrad tx wasm execute <contract-address> <handle-msg> <coins>
```

In the CLI, 
- `contract-address`: Enter **token address**

In the message:
- `send.contract`: Enter **pair address**
- `send.amount`: The amount of the origin token to swap from

`send.msg.swap` is an optional value, which is not required for basic swap executions.

Encode `{"swap":{}}` into base64 encoding. It should look something like: `eyJzd2FwIjp7fX0=`.

Enter the base64 encoded value for `msg` of the JSON:

```json
{
    "send": {
        "contract": "<HumanAddr>",
        "amount": "10",
        "msg": "eyJzd2FwIjp7fX0="
    }
}
```

After then, you may proceed as like the above.

