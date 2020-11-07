---
weight: 2
bookFlatSection: true
---

# Pair

## Transaction

### Update config

Update config of:
- Owner transfer
- Commission rate update (in the rate of percent. e.g.) 0.5 -> 0.5% )

```json
{
    "update_config": {
        "owner": "<HumanAddr>", // or do not insert this key if not essential
        "lp_commission": 0.3, // or do not insert this key if not essential
        "owner_commission": 0.3, // or do not insert this key if not essential
    }
}
```

### Provide liquidity

Send user's asset to the terraswap contract and provide liquidity.<br />
**NOTE: You should [allow your allowance]({{< relref "/docs/reference/token" >}}) of the token before providing liquidity!!**

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
    "slippage_tolerance": 0.1 // or do not insert this key if not essential
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
        "belief_price": 0.1,  // or do not insert this key if not essential
        "max_spread": 0.1, // or do not insert this key if not essential
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
        "belief_price": 0.1,  // or do not insert this key if not essential
        "max_spread": 0.1, // or do not insert this key if not essential
        "to": "<HumanAddr>",
    }
}
```

## Query

### General config

```json
{
    "config_general" :{}
}
```

### Asset config

```json
{
    "config_asset": {}
}
```

### Swap config

```json
{
    "config_swap": {}
}
```

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
