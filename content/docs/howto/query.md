---
weight: 3
bookFlatSection: true
---

# Query

## How to send query request

- Command line
```bash
terrad query wasm contract-state smart <contract_address> '<JSON_formed_message>'
```
ex)

```bash
terrad query wasm contract-state smart terra1466nf3zuxpya8q9emxukd7vftaf6h4psr0a07srl5zw74zh84yjqxl5qul '{"pairs":{}}'
```

- RESTFul API
```URL
<light_clinet_address>/cosmwasm/wasm/v1/contract/<contract_address>/smart/<JSON_formed_message_with_base64>
```

ex)

```URL
https:://lcd.terra.dev/cosmwasm/wasm/v1/contract/terra1466nf3zuxpya8q9emxukd7vftaf6h4psr0a07srl5zw74zh84yjqxl5qul/smart/eyJwYWlycyI6e319
```

## How to oragnize query message

You may check `<contract>/src/msg.rs` of each contract. <br/>
Here is an example. Query messages are defined as like below:

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Balance { address: HumanAddr },
    ...
}
```

This is one of the message
```rust
Balance { address: HumanAddr }
```

You may wrap as `{}` and change it into snake case as like below:
```json
{"balance": {"address": "<HumanAddr>"}}
```

`HumanAddr` is a type, which is bech32-encoded address starting from `terra`.<br />
Here is an example:
```json
{"balance": {"address": "terra1wxe503thjmapngtnyqarxrc4jy80vf800vf0cy"}}
```

This rule can be applied to other messages. This overview can be helpful of contract self-study.
