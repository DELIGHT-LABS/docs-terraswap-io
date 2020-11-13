---
weight: 3
bookFlatSection: true
---

# CW20 Token

## Transaction

### Transfer

Transfer token from the sender user to the recipient.

```json
{
    "transfer": {
        "recipient": "<HumanAddr>",
        "amount": 123123,
    }
}
```

### Burn

Burn tokens from total supply.

```json
{
    "burn": {
        "amount": 123123
    }
}
```

### Send

Work same as transfer but it sends token to contract, not user address. <br />
And, it triggers given message in `msg`. The given message should be executable on recipient contract.<br />
`msg` is base64-endcoded JSON string.

```json
{
    "send": {
        "contract": "<HumanAddr>",
        "amount": 123123,
        "msg": "1234erwfaffaesfaef="
    }
}
```

### Mint

It produces token and transfer to given recipient.

```json
{
    "mint": {
        "recipient": "<HumanAddr>",
        "amount": 123123
    }
}
```

### Increase/Decrease allowance

It increases / decreases allowance of withdrawable token from the executor.<br />
After execution, user token can be transferred by spender's contract method execution, not only `transfer`.<br />
And it expires on the given time point.

```json
{
    "increase_allowance": {
        "spender": "<HumanAddr>",
        "amount": 123123,
        "expires": {
            "at_height": 123123,
            // or
            "at_time": 123123,
            // or
            "never": {}
        }
    }
}
```

```json
{
    "decrease_allowance": {
        "spender": "<HumanAddr>",
        "amount": 123123,
        "expires": {
            "at_height": 123123,
            // or
            "at_time": 123123,
            // or
            "never": {}
        }
    }
}
```

### Trasnfer from / send from

It transfers token from owner to recipient, which not owner nor recipeint are not the contract executor.<br />
Before execution, the allowance should be increased.

```json
{
    "transfer_from": {
        "owner": "<HumanAddr>",
        "recipient": "<HumanAddr>",
        "amount": 123123,
    }
}
```

```json
{
    "send_from": {
        "owner": "<HumanAddr>",
        "recipient": "<HumanAddr>",
        "amount": 123123,
        "msg": "<base64_encoded_json_message>"
    }
}
```

### Burn from

Burns token from token supply, whose owner is not same as the contract executor.

```json
{
    "burn_from": {
        "owner": "<HumanAddr>",
        "amount": 123123
    }
}
```

## Query

### Get balance

Check the balance of the given address

```json
{
    "balance": {
        "address": "<HumanAddr>"
    }
}
```

### Token info

Check metadata of the contract.<br />
It check:
- Name
- Decimals
- Total supply
- Other useful information

```json
{
    "token_info": {}
}
```

### Get minter

```json
{
    "minter": {}
}
```

### Get allowance

Get allowance list of the given owner & spender.

```json
{
    "allowance": {
        "owner": "<HumanAddr>",
        "spender": "<HumanAddr>"
    }
}
```

### Get all allowance

Get list of all allowance of the token.

```json
{
    "all_allowances": {
        "owner": "<HumanAddr>",
        "start_after": "<HumanAddr>", // optional
        "limit": 1 // optional
    }
}
```

### Accounts list that have the token

Get all accounts list of the token holder.

```json
{
    "all_accounts": {
        "start_after": "<HumanAddr>", // optional
        "limit": 1 // optional
    }
}
```
