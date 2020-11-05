---
weight: 3
bookFlatSection: true
---

# Query

## How to send query request

- Command line
```bash
terracli query wasm contract-store <contract_address> '<JSON_formed_message>'
```
ex)

```bash
terracli query wasm contract-store terra18vd8fpwxzck93qlwghaj6arh4p7c5n896xzem5 '{"balance":{"address": "terra1wxe503thjmapngtnyqarxrc4jy80vf800vf0cy"}}'
```

- RESTFul API
```URL
<light_clinet_address>/wasm/contracts/<contract_address>/store?query_msg=<JSON_formed_message>
```

ex)

```URL
https://tequila-lcd.terra.dev/wasm/contracts/terra1wxe503thjmapngtnyqarxrc4jy80vf800vf0cy/store?query_msg={%22pairs%22:{}}
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
