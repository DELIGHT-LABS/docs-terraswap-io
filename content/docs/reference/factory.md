---
weight: 2
bookFlatSection: true
---

# Factory

The factory contract registers a pair between two tokens.
It uses the pre-stored pair contract binary and instantiates it so that users do not need to execute a pair contract by themselves.

## Transaction

### Create pair

Instantiate a pair from uploaded WASM binary. Please check [this document]({{< relref "/docs/howto/create_your_own_pair" >}}) in detail usage.

```json
{
  "create_pair": {
    "asset_infos": [
      {
        "token": {
          "contract_addr": "terra..."
        }
      },
      {
        "native_token": {
          "denom": "uluna"
        }
      }
    ]
  }
}
```

## Query

### Config

```json
{
    "config": {}
}
```

### Pair

```json
{
    "pair": {
        "asset_infos": [
            {
                "token": {
                    "contract_addr": "<Addr>"
                }
            },
            {
                "native_token": {
                    "denom": "uluna"
                }
            }
        ]
    }
}
```

### Pairs

```json
{
    "pairs": {
        "start_after": [ // optional
            {
                "token": {
                    "contract_addr": "<Addr>"
                }
            },
            {
                "native_token": {
                    "denom": "uluna"
                }
            }
        ],
        "limit": 10 // optional, default=10, max=30
    }
}
```
