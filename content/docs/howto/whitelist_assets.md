---
weight: 1
bookFlatSection: true
---

# Allowlist Your Asset

Terraswap aims to be a pure DEX that anyone can mint and list their own assets. However, this direction may accept many scams to be listed that harms many innocent users, especially crypto newbies. As this kind of concerns grows, which can disrupt our ecosystem at last, we decided to adopt the safelisting system of the Terra repository.

According to the above reason, project developers are required to allowlist their asset information on the Terra Asset repository so as to be a verified asset on Terraswap with its logo.

---
> **NOTE**
>
> As Terraswap zeros in on a pure DEX, Terraswap does not judge whether the asset is a genuine or scam by itself. All assets should be treated equally & all users have responsible for their transactions on Terraswap. Terraswap team has been exempted from any loss from its app usage. Please be sure that you are trading the correct tokens you want by checking their addresses in each transaction.
---

## Find Terra Asset Repository

Please find it on [`assets` repository](https://github.com/terra-money/assets)

## Add Your Asset Information

1. If your asset is Terra native
    - Modify `cw20/token.js`
    - Add protocol, symbol, name, token contract address, the path of the icon, and decimals
    - [Example](https://github.com/terra-money/assets/blob/master/cw20/tokens.js)

1. If your asset is IBC token
    - Modify `ibc/token.js`
    - Add denom, IBC path, base denom, symbol, name, and the path of the icon
    - [Example](https://github.com/terra-money/assets/blob/master/ibc/tokens.js)

## Add the Pair Information

- Modify `cw20/pairs.js`
- Add the pair contract address and its pair assets information
- [Example](https://github.com/terra-money/assets/pull/74/files)

## [OPTIONAL] Add Your Non-asset Contract Informtaion

If you have more contracts(airdrop, governance, for game playing, etc.) but they are not related to your assets, you may put your information on `cw20/contracts.js`. This safelisting is applied to [Terra finder](https://finder.terra.money/)
