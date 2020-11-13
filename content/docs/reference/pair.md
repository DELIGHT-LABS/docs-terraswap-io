---
weight: 2
bookFlatSection: true
---

# Pair

## Transaction

### Provide liquidity

Send user's asset to a Terraswap contract and provide liquidity.<br />
**NOTE: You should [allow your allowance]({{< relref "/docs/reference/token" >}}) of the token before providing liquidity!**

Asset can be both of contract-based token and native token. It can be distinguished with the key under `info`: `token` or `native_token`.


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
    ],
    "slippage_tolerance": 0.1 // optinonal
  }
}
```

### Swap

Swap between the given two tokens. It can be thought of as trade.<br />
`offer_asset` is your source asset and `to` is your destination token contract.<br />

**NOTE: You should [allow your allowance]({{< relref "/docs/reference/token" >}}) of the token before swap!!**<br />
**NOTE: This method is only used to swap to contract-based token as a destination! In opposite case, please check [another document]({{< relref "/docs/howto/swap" >}})**

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
        "belief_price": 0.1,  // optional
        "max_spread": 0.1, // optional
        "to": "<HumanAddr>",
    }
}
```

```json
{
    "swap": {
        "offer_asset": {
            "info" : {
                "token": {
                    "contract_addr": "<HumanAddr>"
                }
            },
            "amount": "10"
        },
        "belief_price": 0.1,  // optional
        "max_spread": 0.1, // optional
        "to": "<HumanAddr>" // optional,
    }
}
```

## Query


### Pool

```json
{
    "pool": {}
}
```

### Simulation

```json
{
    "simulation": {
        "offer_asset": {
            "info" : {
                "token": {
                    "contract_addr": "<HumanAddr>"
                }
            },
            "amount": "10"
        }
    }
}
```

### Reverse simulation

```json
{
    "reverse_simulation": {
        "ask_asset": {
            "info" : {
                "token": {
                    "contract_addr": "<HumanAddr>"
                }
            },
            "amount": "10"
        }
    }
}
```
