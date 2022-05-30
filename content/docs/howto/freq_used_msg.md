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
  "data": {
    "assets": [
      {
        "info": {
          "token": {
            "contract_addr": "terra1cl0kw9axzpzkw58snj6cy0hfp0xp8xh9tudpw2exvzuupn3fafwqqhjc24"
          }
        },
        "amount": "81695"
      },
      {
        "info": {
          "native_token": {
            "denom": "uluna"
          }
        },
        "amount": "40774785"
      }
    ],
    "total_share": "1814863"
  }
}
```
- ex) Luna <> DELIGHT https://pisco-lcd.terra.dev/cosmwasm/wasm/v1/contract/terra12gyq86zdaef9hafs6a6y24xqxgjyzh4m5a224tenlchj2ekgf5tsv94gwj/smart/eyJwb29sIjp7fX0=

### Simulation / Reverse simulation

Simulation works for guessing how much you will be swapped with your token.

If you want to know how much the target token will be given from source token, use `simulation`. Or if you want to derive the number of source token from the number of target token, use this query message.

#### Simulation request

```json
{
  "simulation": {
    "offer_asset": {
      "amount":"1000000",
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
  "data": {
    "return_amount": "1950",
    "spread_amount": "48",
    "commission_amount": "5"
  }
}
```
- ex) Luna <> DELIGHT https://pisco-lcd.terra.dev/cosmwasm/wasm/v1/contract/terra12gyq86zdaef9hafs6a6y24xqxgjyzh4m5a224tenlchj2ekgf5tsv94gwj/smart/ewogICJzaW11bGF0aW9uIjogewogICAgIm9mZmVyX2Fzc2V0IjogewogICAgICAiYW1vdW50IjoiMTAwMDAwMCIsCiAgICAgICJpbmZvIjogewogICAgICAgICJuYXRpdmVfdG9rZW4iOiB7CiAgICAgICAgICAiZGVub20iOiJ1bHVuYSIKICAgICAgICB9CiAgICAgIH0KICAgIH0KICB9Cn0=

#### Reverse simulation request

```json
{
  "reverse_simulation":{
    "ask_asset": {
      "amount":"1000000",
      "info": {
        "native_token": {
          "denom": "uluna"
        }
      }
    }
  }
}
```
#### Reverse simulation response
```json
{
  "data": {
    "offer_amount": "2060",
    "spread_amount": "25157",
    "commission_amount": "3009"
  }
}
```
- ex) Luna <> DELIGHT https://pisco-lcd.terra.dev/cosmwasm/wasm/v1/contract/terra12gyq86zdaef9hafs6a6y24xqxgjyzh4m5a224tenlchj2ekgf5tsv94gwj/smart/ewogICJyZXZlcnNlX3NpbXVsYXRpb24iOnsKICAgICJhc2tfYXNzZXQiOiB7CiAgICAgICJhbW91bnQiOiIxMDAwMDAwIiwKICAgICAgImluZm8iOiB7CiAgICAgICAgIm5hdGl2ZV90b2tlbiI6IHsKICAgICAgICAgICJkZW5vbSI6ICJ1bHVuYSIKICAgICAgICB9CiAgICAgIH0KICAgIH0KICB9Cn0=