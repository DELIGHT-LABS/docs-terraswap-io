---
bookFlatSection: true
---

# Trading Fees

Each liquidity pool for allowlisted asset pair has **LP Commission** rates paid as a trading fee outside of the spread algorithmic price-making determines. In Terraswap, LP Commission is fixed at *0.3%* and are deducted from the trader's received asset of the transaction.

{{< katex display >}}\text{received} = B_{\text{out}}(1- \text{fee}_{\text{LP}}) - \text{tax}{{< /katex >}}

LP Commission paid in each trade goes back to the corresponding liquidity pool as a fee for liquidity providers. They can withdraw LP Commission that is accumulated to the pool by burning LP tokens generated from liquidity provision.
