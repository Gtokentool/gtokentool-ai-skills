# Buy and sell procedure

## Buy through FairMint

The input amount is the exact wallet payment. Never append a platform fee.

For a native payment:

```text
quote = quoteBuy(buyer, amount, agentId)
simulate buyWithNative(minTokensOut, agentId, referrerAgentId)
  value = amount
```

For an ERC-20 payment:

1. Confirm exact `approve(FairMint, amount)`.
2. Wait for approval visibility.
3. Refresh the quote.
4. Simulate and confirm `buyWithToken`.

Set `minTokensOut` only from the user's stated slippage tolerance or a
conservative floor derived from the fresh quote. A quote below that floor stops
the order.

After receipt, verify the `Purchase` event has the expected buyer, Agent ID,
amount, and token amount, then compare actual token balance before and after.

## Sell on Pancake V2

The launch token applies sell tax in addition to the Router's `amountIn`.
For the deployed profile:

```text
sell tax = 2%
burn = 1%
treasury = 1%
```

The pair receives the full Router `amountIn`, while the seller is also debited
the 2% tax. Therefore:

```text
maxRouterAmount = walletTokenBalance * 10000 / 10200
```

Never use the entire wallet balance as Router `amountIn`.

Flow:

1. Read token balance and sell-tax fields.
2. Calculate `maxRouterAmount`.
3. Quote from live pair reserves.
4. Approve only the selected Router amount.
5. Simulate `swapExactTokensForETH`.
6. Summarize expected BNB, minimum BNB, tax, burn, and gas.
7. Require a fresh standalone confirmation.
8. Send and verify the receipt plus BNB and token balance changes.

On a sell, report the user-specified Router amount separately from the extra tax
debit so the final wallet decrease is not misleading.
