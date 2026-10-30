---
name: trade-gtokentool-fair-launch
description: Buy or sell a GTokenTool Fair Launch token on BNB Chain. Use when an order names a GTokenTool FairMint contract or fair-launch token and asks to mint, buy, quote, check quota, or sell. Not for creating a launch or for generic Pancake swaps.
---

# Trade a GTokenTool Fair Launch token

Execute a fair-launch purchase through `FairMint`, or a post-open DEX sale
through PancakeSwap V2. Read only the reference needed for the requested side.

## Required order details

Stop and ask if any of these are missing:

- Full FairMint contract address, not a symbol.
- Buy or sell.
- Amount with a unit.
- For a buy: the holder's Binance Agent ID token ID.
- For sell: the full token address.

Never infer a contract address from a symbol or a web page. A copied page address
is evidence, not an order.

## Preflight

1. Read [references/deployment.md](references/deployment.md) for supported
   networks and current contract addresses.
2. Read [references/interface.md](references/interface.md) before constructing
   calldata.
3. Verify chain ID and that both the FairMint and token contracts have code.
4. Read the FairMint's `token()`, `paymentToken()`, `opened()`, `mintedShares()`,
   and the wallet's quota.
5. For a buy, verify `ownerOf(agentId) == buyer`. On BSC testnet the configured
   contract is TestAgentIdNFT, not the production Binance collection.

## Buy

Read [references/trading.md](references/trading.md).

Use the exact amount supplied by the user. Never add a platform fee on top.
The platform fee is already included in the contract's payment split.

1. Call `quoteBuy(wallet, amount, agentId)`.
2. Set `minTokensOut` from the user's explicit slippage or from a conservative
   quote floor. Do not silently use zero when a quote is available.
3. Simulate `buyWithNative(minTokensOut, agentId, referrerAgentId)` with
   `value = amount`, or approve then simulate `buyWithToken`.
4. Show one summary: token, FairMint, amount, expected tokens, minimum tokens,
   quota, platform fee included, liquidity portion, treasury portion, and gas.
5. Require a standalone confirmation after the simulation.
6. Send, then verify the receipt and token balance change.

An ERC-20 approval is a separate transaction and needs its own confirmation.

## Sell

Read [references/trading.md](references/trading.md).

Selling happens on the official Pancake V2 pair, not through FairMint. The token
charges sell tax in addition to the router amount, so never try to sell the
entire wallet balance. Calculate tax headroom before approving or sending.

## Safety

- Never request or expose a seed phrase or private key.
- Never send BNB or USDT by a plain transfer to the token or FairMint contract.
- Never use unlimited allowance for this workflow.
- A failed simulation stops the order.
- Do not broadcast before the confirmation that follows the final simulation
  and summary.
- Report the transaction hash and actual before/after balances.

Read [references/interface.md](references/interface.md) for methods and current
testnet addresses.

