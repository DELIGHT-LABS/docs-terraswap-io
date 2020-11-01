---
bookFlatSection: true
---

# Trading Fees

Each liquidity pool for mAssets/MIR has 2 commission rates (adjustable by governance) that make up the total trading fee outside of spread determined by algorithmic price-making. Trading fees are received as mAssets/MIR or UST, depending on the direction of the individual trade.

> Additionally, if the traded asset is a native token such as UST, the Terra network will incur a tax on the transfer (not controlled by TerraSwap contract).

{{< katex display >}}\text{received} = B_{\text{out}}(1- \text{fee}_{\text{LP}} - \text{fee}_{\text{owner}}) - \text{tax}{{< /katex >}}

## LP Commission

The **LP Commission** is the portion of each trade that goes back into to the pool as commissions for liquidity providers. It can be withdrawn by burning LP tokens and reclaiming a portion of the pool. By default, this is set to 0.25%.

The LP Commission serves mainly as an incentive to provide liquidity from TerraSwap's LP Token mechanism.

## Owner Commission

The **Owner Commission** is the portion of each trade that gets sent to the TerraSwap pair's contract owner (it does not stay in the pool). By default, this is set to 0.05%.
