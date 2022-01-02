---
weight: 1
bookFlatSection: true
---

# Whitelist your asset

Terraswap aims to be a pure DEX, that anyone can mint & list their own assets. On the contrary of that, many duplicated scams are also listed and it harms to many innocent users, especially crypto newbies. As this matter can harm to grow our ecosystem, we adopt the whitelisting system of Terra repository.

Therefore, for project developers, if you want to unify your asset on Terraswap with your logo, you need to whitelist your asset information on the Terra Asset repository.

---
> **NOTE**
>
> As Terraswap aims & is a pure DEX, Terraswap does not judge whether the asset is a genuine or scam. All assets should be treated equally & all users have responsible for their transactions on Terraswap. Terraswap team has been exempted from any loss from its app usage.
---

## Find Terra Asset repository

Please find it on [`assets` repository](https://github.com/terra-money/assets)

## Add your asset information

1. If your asset is Terra native
    - Modify `cw20/token.js`
    - Add protocol, symbol, name, token contract address, the path of the icon, and decimals
    - [Example](https://github.com/terra-money/assets/pull/99)

1. If your asset is IBC token
    - Modify `ibc/token.js`
    - Add denom, IBC path, base denom, symbol, name, and the path of the icon
    - [Example](https://github.com/terra-money/assets/blob/master/ibc/tokens.js)

## Add the pair information

- Modify `cw20/pairs.js`
- Add the pair contract address and its pair assets information
- [Example](https://github.com/terra-money/assets/pull/74/files)

## [OPTIONAL] Add your non-asset contract informtaion

If you have more contracts(airdrop, governance, for game playing, etc) but if it is not related your assets, you may put your information on `cw20/contracts.js`. This whitelisting is applied to [Terra finder](https://finder.terra.money/)
