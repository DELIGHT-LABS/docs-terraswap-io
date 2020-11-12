---
bookFlatSection: true
---

# Trading Fees

Each liquidity pool for mAssets/MIR has LP commission rates (adjustable by governance) that make up the total trading fee outside of spread determined by algorithmic price-making. Trading fees are received as mAssets/MIR or UST. The fee will be paid in receiving token.

> Additionally, if the traded asset is a native token such as UST, the Terra network will incur a tax on the transfer (not controlled by Terraswap contract).

{{< katex display >}}\text{received} = B_{\text{out}}(1- \text{fee}_{\text{LP}} - \text{fee}_{\text{owner}}) - \text{tax}{{< /katex >}}

## LP Commission

The **LP Commission** is the portion of each trade that goes back into to the pool as commissions for liquidity providers. It can be withdrawn by burning LP tokens and reclaiming a portion of the pool.

This is set to 0.3%.
