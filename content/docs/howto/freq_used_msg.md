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

If you provide cw20, first increase your allowance. [Execute `IncreaseAllowance`]({{< relref "/docs/reference/token#increasedecrease-allowance" >}})

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
  "query_result": {
    "assets": [
      {
        "info": {
          "native_token": {
            "denom": "ukrw"
          }
        },
        "amount": "2559780349122011"
      },
      {
        "info": {
          "native_token": {
          "denom": "uluna"
          }
        },
        "amount": "31364620251"
      }
    ],
    "total_share": "7114570186107"
  }
}
```
- ex) Luna <> KRT https://lcd.terra.dev/terra/wasm/v1beta1/contracts/terra1zw0kfxrxgrs5l087mjm79hcmj3y8z6tljuhpmc/store?query_msg=eyJwb29sIjp7fX0=

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

#### Simulation response

```json
{
  "query_result": {
    "return_amount": "813428493367",
    "spread_amount": "260126256",
    "commission_amount": "2447628365"
  }
}
```
- ex) Luna <> KRT https://lcd.terra.dev/terra/wasm/v1beta1/contracts/terra1zw0kfxrxgrs5l087mjm79hcmj3y8z6tljuhpmc/store?query_msg=ewogICJzaW11bGF0aW9uIjogewogICAgIm9mZmVyX2Fzc2V0IjogewogICAgICAiYW1vdW50IjoiMTAwMDAwMDAiLAogICAgICAiaW5mbyI6IHsKICAgICAgICAibmF0aXZlX3Rva2VuIjogewogICAgICAgICAgImRlbm9tIjoidWx1bmEiCiAgICAgICAgfQogICAgICB9CiAgICB9CiAgfQp9

#### Reverse simulation request

```json
{
  "reverse_simulation":{
    "ask_asset": {
      "amount":"500000000000",
      "info": {
        "native_token": {
          "denom": "ukrw"
        }
      }
    }
  }
}
```
#### Reverse simulation response
```json
{
  "query_result": {
    "offer_amount": "6146066",
    "spread_amount": "98210972",
    "commission_amount": "1504513540"
  }
}
```
- ex) Luna <> KRT https://lcd.terra.dev/terra/wasm/v1beta1/contracts/terra1zw0kfxrxgrs5l087mjm79hcmj3y8z6tljuhpmc/store?query_msg=ewogICJyZXZlcnNlX3NpbXVsYXRpb24iOnsKICAgICJhc2tfYXNzZXQiOiB7CiAgICAgICJhbW91bnQiOiI1MDAwMDAwMDAwMDAiLAogICAgICAiaW5mbyI6IHsKICAgICAgICAibmF0aXZlX3Rva2VuIjogewogICAgICAgICAgImRlbm9tIjogInVrcnciCiAgICAgICAgfQogICAgICB9CiAgICB9CiAgfQp9