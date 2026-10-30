# Buy and sell procedure

## Buy through FairMint

The input amount is the exact wallet payment. Never append a platform fee.
All signing and broadcasting goes through Binance Agentic Wallet's
`contract-call preview` and `contract-call execute` commands.

For a native payment:

```text
quote = quoteBuy(buyer, amount)
calldata = buyWithNative(minTokensOut, referrer)
value = amount
```

For an ERC-20 payment:

1. Confirm exact `approve(FairMint, amount)`.
2. Preview the approval as a separate `baw contract-call preview` operation,
   show the exact spender and amount, get explicit confirmation, and execute it.
3. Wait for approval visibility.
4. Refresh the quote.
5. Preview `buyWithToken` through `baw contract-call preview`.
6. Show the parsed transaction, simulation result, risks, and authority changes.
7. Require a fresh standalone confirmation, then execute the requestId.

Set `minTokensOut` only from the user's stated slippage tolerance or a
conservative floor derived from the fresh quote. A quote below that floor stops
the order.

After receipt, verify the `Purchase` event has the expected buyer, amount, and
token amount, then compare actual token balance before and after. Eligibility is
checked from the buyer wallet's Agent ID NFT balance at purchase time. The user
never supplies an Agent ID token number.

## Sell on Pancake V2

Use the official `binance-agentic-wallet` market-order flow after the token is
on the official Pancake pair. The launch token applies sell tax in addition to
the Router's `amountIn`. For the deployed profile:

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
3. Run `baw market-order quote` against the official pair.
4. Summarize expected BNB, minimum BNB, tax, burn, price impact, and gas.
5. Require a fresh standalone confirmation.
6. Run `baw market-order swap`.
7. Poll `baw market-order list` until the order is `FINISHED` or `FAILED`.
8. Verify the receipt and BNB and token balance changes.

On a sell, report the user-specified Router amount separately from the extra tax
debit so the final wallet decrease is not misleading.
