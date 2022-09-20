---
weight: 1
bookFlatSection: true
---

# Create Your Own Pair

PLEASE CHECK [HERE](#important---only-for-terra-20) for additional action on Terra 2.0

## Instantiation by Contract Address

You can use the Terraswap token factory contract.
- The address on Terra 2.0: `terra1466nf3zuxpya8q9emxukd7vftaf6h4psr0a07srl5zw74zh84yjqxl5qul`
- The address on Terra Classic: `terra1jkndu9w5attpz09ut02sgey5dd3e8sq5watzm0`

The JSON message format is as follows:

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

This is a JSON constructor of pair contract.

- A token pair can be either, contract-based token, or terra-native token
  - `asset_infos[x].token.contract_addr`: Contract-basd token **address** is entered here.
  - `asset_infos[x].native_token.denom`: Terra native token **denominator** is entered here.

Then, you may execute the contract with the organized JSON above.

## IMPORTANT - Only for Terra 2.0

PLEASE BE AWARE that native tokens must be registered in the factory first in order to create a pair including them. You can check registered native tokens through [this query]({{< ref "/docs/reference/factory#native-token-decimals" >}}). Contact us via [Terraswap Discord](https://discord.gg/XERFR8dCWv) if you need to add a new native token.