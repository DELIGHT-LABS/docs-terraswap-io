---
weight: 2
bookFlatSection: true
---

# Factory

This contract registers the relation between your token and others. <br />
It uses the pre-stored pair contract binary and instantiate it. So, you don't have to execute pair contract additionally.

## Transaction

### Create pair

Instantiate pair from uploaded WASM binary. Please check [this document]({{< relref "/docs/howto/create_your_own_pair" >}}) in detail usage.

```json
{
  "pair_code_id": "1",
  "token_code_id": "2",
  "init_hook": {
    "msg": "<base64_encoded_json_string>",
    "contract_addr": "<HumanAddr>"
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
                    "contract_addr": "<HumanAddr>"
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
