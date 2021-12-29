---
weight: 3
bookFlatSection: true
---

# Swap

Swap in Terraswap works the same as trading on any other exchange. Prices on centralized exchanges for both stock and cryptocurrency are decided by a bidding system, and supply & demand. These prices change in millisecond intervals, so real-time transactions are maintained in the market. But if anyone is trying to implement an exchange with the bidding-based pricing on blockchain, immediate execution of transaction becomes impossible due to block times. 

Therefore, a different pricing approach is used for Terraswap - algorithmic pricing by tracking the ratio of the paired asset within the liquidity pool. To learn more about this approach, please refer to [mechanism section]({{< relref "/docs/introduction/mechanism" >}}).

## Prerequisites
- [Terracore]({{< https://github.com/terra-money/core >}})
- Token & pair should be deployed
- You should know the addresses of token & pair.

Make sure you have terrad working - theres instructions to set it up in the terracore [Github]({{< https://github.com/terra-money/core >}})
If you can succesfully run "terrad version --long" you have installed terracore and can now use terrad - if "terrad version --long" fails with the error "-bash: terrad: command not found" then run these two commands to hopefully fix it 

```bash
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

After successfully running "terrad version --long" you will need to connect to a terra node and add a wallet to sign the txes

To add a wallet run "terrad keys add <customname> --recover" then input your mnemonic phrase, if you want to create a new wallet run "terrad keys add <customname>"
    
You can connect a node by first running "terrad" to find available commands and flags, then navigate to the directory shown in the --home flag description (it might look something like this "/Users/john/.terra") - after navigating to the .terra folder locate the config.toml and client.toml files in the config folder

To connect to a node you will have to edit those files by adding in a terra rpc link (find terra rpc's [here]({{< https://docs.terra.money/Reference/endpoints.html >}})), there are 4 places you need to add the link in the config file and 1 in the client file

After making the changes and saving the file you should be good to go!!


## Swap by using CLI

Examples and flag info at the very bottom

### When Offer_asset is native & IBC token

By command line, all transactions can be executed in this way:

```bash
terrad tx wasm execute <contract-address> <handle-msg> <flags>
```

- `contract-address`: In swap transaction, this should be the pair address.
- `handle-msg`: It represents the method and parameters of this execution. Will explain below.
- `flags`: -- flags for chain id, from address, and gas fee settings

Enter `contract-address`, `flags` and `handle-msg`. To learn more about the general rules for `handle-msg` please refer to this [link]({{< relref "/docs/howto/query" >}}).

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
        "to": "<HumanAddr>"
    }
}
```

`swap.offer_asset` represents your source asset. Please make sure that `swap.offer_asset.amount` is not same as your amount. It depends on the decimal of the token setting. In case of Luna, its decimal is 6. Then, if `swap.offer_asset.amount` reads `10`, the actual amount is `10 x 10^-6`. So, you should multiply with mathcing value, `10^(decimal)`.

`swap.to` is the destination token address. It is unnecessary to enter the amount to swap into, since Terraswap price is calculated algorithmically. 

After filling it out, you may choose to change it into an inline string (not necessary if you can make it with multiline):

```bash
'{"swap":{"offer_asset": {"info" : {"native_token": {"denom": "uluna"}},"amount": "10"},"to": "<HumanAddr>",}}'
```

This is your `handle-msg`. The `handle-msg` can be used to complete the CLI command to swap tokens. 

### When offer_asset is contract-minted token

Swapping contract-minted token to native token is executed with the same logic as above, but requires a differnet `handle-msg` due to difference in token system and its implementation. Message looks like:

```json
{
    "send": {
        "contract": "<HumanAddr>",
        "amount": "10",
        "msg": Binary({
            "swap": {}
        })
    }
}
```

The method is a little bit tricky. Please do not lose your concentration!

```bash
terrad tx wasm execute <contract-address> <handle-msg> <flags>
```

In the CLI, 
- `contract-address`: Enter **origin token address**

In the message:
- `send.contract`: Enter **swap pair address**
- `send.amount`: The amount of the origin token to swap from

`send.msg.swap` is an optional value, which is not required for basic swap executions.

Encode `{"swap":{}}` into base64 encoding. It should look something like: `eyJzd2FwIjp7fX0=`.

Enter the base64 encoded value for `msg` of the JSON:

```json
{
    "send": {
        "contract": "<HumanAddr>",
        "amount": "10",
        "msg": "eyJzd2FwIjp7fX0="
    }
}
```

After then, you may proceed as like the above.


### Flags
(All without the "" of course)

from address = "--from yourconnectedwalletaddress"
chain id = "--chain-id columbus-5" - subject to change
gas settings - "--gas-prices=0.5uusd --gas=auto --gas-adjustment=1.4" - [gas fee docs]({{< https://docs.terra.money/Reference/terrad/#setting-fees >}})

    
### Examples
    
When Offer_asset is native & IBC token
Luna/bluna example - swapping 0.1 luna to bluna
    
```bash
terrad tx wasm execute terra1sgu6yca6yjk0a34l86u6ju4apjcd6refwuhgzv {"swap": {"offer_asset": {"info": {"native_token": {"denom": "uluna"}},"amount": "100000"},"to": "terra1kc87mu460fwkqte29rquh4hc20m54fxwtsx7gp"}} --from yourconnectedwalletaddress --chain-id columbus-5 --gas-prices=0.5uusd --gas=auto --gas-adjustment=1.4
```

When offer_asset is contract-minted token
loop/loopr example - swapping 1 loop to loopr (using the loop dex's contract cause I know they have liquidity)
    
```bash
terrad tx wasm execute terra1nef5jf6c7js9x6gkntlehgywvjlpytm7pcgkn4 '{"send": {"contract": "terra154jt8ppucvvakvqa5fyfjdflsu6v83j4ckjfq3","amount": "1000000","msg": "eyJzd2FwIjp7fX0="}}' --from yourconnectedwalletaddress --chain-id columbus-5 --gas-prices=0.5uusd --gas=auto --gas-adjustment=1.4
```

