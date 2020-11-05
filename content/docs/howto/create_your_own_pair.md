---
weight: 1
bookFlatSection: true
---

# Create Your Own Pair

## How to create your own pair

In this procedure, it uses pre-stored pair. Token contract should be instantiated in advance.

The JSON parameter of instantiation is as like below:

```json
{
  "pair_code_id": "1",
  "token_code_id": "2",
  "init_hook": {
    "msg": "base64_encoded_json_data",
    "contract_addr": "terra..."
  }
}
```

This factory contract links between your token and others, and prepares for terraswap running. <br />
You may put appropriate number to `pair_code_id` and `token_code_id`. (will be announced) <br />
Your token address is used at `init_hook.contract_addr`.

The JSON itself is very simple. But the problem is `init_hook.msg`. <br />

First, you should organize another JSON as like below:

```json
{
    "asset_infos": [
        {
            "token": {
                "contract_addr": "terratokenaddr0001xxxxxxxx"
            }
        },
        {
            "native_token": {
                "denom": "uluna"
            }
        }
    ],
    "commission_collector": "terracommissioncollector0001xxxxxxxx",
    "lp_commission": "0.3",
    "owner": "terratokencontractowner0001xxxxxxxxxx",
    "owner_commission": "0.2",
    "token_code_id": 1
}
```

This is a JSON constructor of pair contract.

- The pair can be between contract-based token, or terra-native token
  - `asset_infos[x].token.contract_addr`: if one side of the pair is contract-contructed token, you may input your tokena address here.
  - `asset_infos[x].native_token.denom`: if one side of the pair is terra-native token, you may input the denominator here.
- Owner and LP provider takes the commission of each trade of the pair.
  - `commission_collector`, `lp_commission`: Before swap trade, initial assets for pair should be transferred. The designated LP provider can send it, and got commission for each swap trade as a reward.
  - `owner`, `owner_commission`: The owner means the authority to change the config of the pair.
  - `token_code_id`: the ID of pre-stored token WASM binary

After complete the JSON, you may convert the JSON into Base64 encoding.

```json
{
    "asset_infos": [
        {
            "token": {
                "contract_addr": "terratokenaddr0001xxxxxxxx"
            }
        },
        {
            "native_token": {
                "denom": "uluna"
            }
        }
    ],
    "commission_collector": "terracommissioncollector0001xxxxxxxx",
    "lp_commission": "0.3",
    "owner": "terratokencontractowner0001xxxxxxxxxx",
    "owner_commission": "0.2",
    "token_code_id": 1
}
```
to

```plain
ew0KICAgICJhc3NldF9pbmZvcyI6IFsNCiAgICAgICAgew0KICAgICAgICAgICAgInRva2VuIjogew0KICAgICAgICAgICAgICAgICJjb250cmFjdF9hZGRyIjogInRlcnJhdG9rZW5hZGRyMDAwMXh4eHh4eHh4Ig0KICAgICAgICAgICAgfQ0KICAgICAgICB9LA0KICAgICAgICB7DQogICAgICAgICAgICAibmF0aXZlX3Rva2VuIjogew0KICAgICAgICAgICAgICAgICJkZW5vbSI6ICJ1bHVuYSINCiAgICAgICAgICAgIH0NCiAgICAgICAgfQ0KICAgIF0sDQogICAgImNvbW1pc3Npb25fY29sbGVjdG9yIjogInRlcnJhY29tbWlzc2lvbmNvbGxlY3RvcjAwMDF4eHh4eHh4eCIsDQogICAgImxwX2NvbW1pc3Npb24iOiAiMC4zIiwNCiAgICAib3duZXIiOiAidGVycmF0b2tlbmNvbnRyYWN0b3duZXIwMDAxeHh4eHh4eHh4eCIsDQogICAgIm93bmVyX2NvbW1pc3Npb24iOiAiMC4yIiwNCiAgICAidG9rZW5fY29kZV9pZCI6IDENCn0=
```

This encoded string goes to `init_hook.msg`.

```json
{
  "pair_code_id": "1",
  "token_code_id": "2",
  "init_hook": {
    "msg": "ew0KICAgICJhc3NldF9pbmZvcyI6IFsNCiAgICAgICAgew0KICAgICAgICAgICAgInRva2VuIjogew0KICAgICAgICAgICAgICAgICJjb250cmFjdF9hZGRyIjogInRlcnJhdG9rZW5hZGRyMDAwMXh4eHh4eHh4Ig0KICAgICAgICAgICAgfQ0KICAgICAgICB9LA0KICAgICAgICB7DQogICAgICAgICAgICAibmF0aXZlX3Rva2VuIjogew0KICAgICAgICAgICAgICAgICJkZW5vbSI6ICJ1bHVuYSINCiAgICAgICAgICAgIH0NCiAgICAgICAgfQ0KICAgIF0sDQogICAgImNvbW1pc3Npb25fY29sbGVjdG9yIjogInRlcnJhY29tbWlzc2lvbmNvbGxlY3RvcjAwMDF4eHh4eHh4eCIsDQogICAgImxwX2NvbW1pc3Npb24iOiAiMC4zIiwNCiAgICAib3duZXIiOiAidGVycmF0b2tlbmNvbnRyYWN0b3duZXIwMDAxeHh4eHh4eHh4eCIsDQogICAgIm93bmVyX2NvbW1pc3Npb24iOiAiMC4yIiwNCiAgICAidG9rZW5fY29kZV9pZCI6IDENCn0=",
    "contract_addr": "terra..."
  }
}
```

Instantiate the factory contract and start your own pair!
