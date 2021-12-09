---
weight: 1
bookFlatSection: true
---

# Frequently Used Messages

## Transaction

### Provide liquidity / Withdraw

As the swap ratio is stated as [here]({{< relref "/docs/introduction/mechanism" >}}), the size of the pool is related with the difference of swap ratio. The ratio goes stable if the size of the pool increases, and vice versa. Otherwise, LP provider needs more tokens for adjusting the price in bigger pool. It means that the market loses elastic. LP provider adjusts this market by providing & withdraw the liquidity within this trade-off.

#### Provide liquidity

Contribute to pool by sending sender's token pair. Not only increasing its size, but also it affects to the swap ratio.

Execute this message by the **Pair contract** address!

If you provide cw20, first increase your allowance. [Execute `IncreaseAllowance`]({{< relref "/docs/reference/token" >}})

```json
{
  "provide_liquidity": {
    "assets": [
      {
        "info" : {
            "token": {
                "contract_addr": "<HumanAddr>"
            }
        },
        "amount": "10"
      },
      {
        "info" : {
            "native_token": {
                "denom": "uluna"
            }
        },
        "amount": "10"
      }
    ]
  }
}
```

#### Withdraw

Withdraw your tokens and decrease the size of the pool.

Execute this message by the **Liquidity token contract** address! Not token contract, not pair contract neither!

```json
{
  "send": {
    "contract": "<PairContractAddress>",
    "amount": 123,
    "msg": "base64-encodedStringOfWithdrawMsg"
  },
}
```

In `send.msg`, you may decode this JSON string into base64 encoding.

```json
{
  "withdraw_liquidity": {}
}
```

Then, the liquidity token contract calculates the portion of your liquidity token comparing to the total supply, and withdraw the pairs.

## Query

### Pool

Pool query message returns the amount of the tokens in the pool from the given pair contract address.

```json
{
  "pool":{}
}
```

Response:

```json
{
  "height": "699536",
  "result": {
    "assets": [
      {
        "info": {
          "native_token": {
            "denom": "ukrw"
          }
        },
        "amount": "616618779506317"
      },
      {
        "info": {
          "native_token": {
            "denom": "uluna"
          }
        },
        "amount": "1407689375952"
      }
    ],
    "total_share": "29432943447776"
  }
}
```

### Simulation / Reverse simulation

Simulation works for guessing how much you will be swapped with your token.

If you want to know how much the target token will be given from source token, use `simulation`. Or if you want to derive the number of source token from the number of target token, use this query message.

#### Simulation request

```json
{
  "simulation": {
    "offer_asset": {
      "amount":"10000000",
      "info": {
        "native_token": {
          "denom":"uluna"
        }
      }
    }
  }
}
```

#### Simulation request

```json
{
  "height": "1274586",
  "result": {
    "return_amount": "2875706843",
    "spread_amount": "147593468",
    "commission_amount": "8653079"
  }
}
```

#### Reverse simulation response

```json
{
  "reverse_simulation":{
    "ask_asset": {
      "amount":"5000000000",
      "info": {
        "native_token": {
          "denom": "ukrw"
        }
      }
    }
  }
}
```

```json
{
  "height":"1274605",
  "result": {
    "offer_amount": "18070071",
    "spread_amount": "463716168",
    "commission_amount": "15045135"
  }
}
```