---
name: trade-gtokentool-fair-launch
description: Buy or sell a GTokenTool Fair Launch token with Binance Agentic Wallet on BSC. Use when an order names a GTokenTool FairMint project or asks to quote, buy, check quota, or sell a fair-launch token. This skill wraps the official binance-agentic-wallet skill and never handles private keys.
metadata:
  author: GTokenTool
  version: '2.0.0'
  requiredCliVersion: '1.12.0'
  openclaw:
    requires:
      bins:
        - baw
---

# Trade a GTokenTool Fair Launch token

This skill prepares GTokenTool-specific quotes and calldata. All wallet access,
preview, signing, and execution must go through the official
`binance-agentic-wallet` skill and its `baw` CLI.

Read [references/trading.md](references/trading.md) for the buy and sell flows and
[references/interface.md](references/interface.md) for the contract ABI.

## Network gate

- Binance Agentic Wallet execution is supported on BSC mainnet, Binance chain ID
  `56`.
- Do not route BSC testnet (`97`) through Agentic Wallet. The official CLI does
  not expose testnet as an execution network.
- If a project URL points to a different chain, stop and report that Agentic
  Wallet execution is unavailable for that deployment.

## Required order details

Stop and ask if any of these are missing:

- A GTokenTool fair-launch project URL or the full FairMint address.
- Buy or sell.
- Amount with a unit.
- For a sell: the full token contract address.

If the user supplies a project URL, extract the address from its
`/fairlaunch/<address>` path. Verify that the configured FairLaunchFactory
recognizes that address. Never infer a token from a ticker or marketing text.

## Mandatory preflight

Use the official `binance-agentic-wallet` instructions. Do not rely on this file
for generic wallet syntax.

1. Run `baw wallet status --json`.
2. Run `baw wallet address --json` and select the BSC address.
3. Run `baw wallet agent-id --json`. Stop if `claimed` is not true.
4. Run `baw wallet settings --json`.
5. A custom contract buy requires Developer Mode. If `devMode.enabled` is false
   or `devMode.expiresAt` has passed, stop and tell the user to enable Developer
   Mode in Binance App → Agentic Wallet → Settings. Re-run the settings command
   after they confirm it is on.
6. Read the FairMint registry, token, payment token, open state, current price,
   quota, and project treasury from BSC mainnet.

Never ask for an Agent ID token number. `baw wallet agent-id` is the supported
eligibility check. Never request a private key, seed phrase, or session token.

## Buy

Follow [references/trading.md](references/trading.md).

1. Read the quoted output with `quoteBuy(buyer, amount)`.
2. Build `minTokensOut` from a fresh quote and the user's explicit slippage.
   If the user gives no slippage, use 1% and disclose it.
3. Build calldata for `buyWithNative(minTokensOut, referrer)` with
   `value = amount`, or `buyWithToken(amount, minTokensOut, referrer)`.
4. Use the zero address for `referrer` unless the user supplied a valid
   GTokenTool referral address.
5. Preview through the official skill:

   ```bash
   baw contract-call preview \
     --binanceChainId 56 \
     --from <agentWalletAddress> \
     --to <fairMint> \
     --value <amountInWei> \
     --inputData <calldata> \
     --json
   ```

6. Show the user the quote, full contract addresses, exact payment, expected and
   minimum tokens, platform fee, pool and treasury portions, parsed transaction,
   simulated balance changes, risks, authority changes, and gas.
7. Require a standalone explicit confirmation after the successful preview.
8. Execute only with the `requestId` from that preview:

   ```bash
   baw contract-call execute --requestId <requestId> --json
   ```

9. If execution is `PENDING_CONFIRMATION`, tell the user to approve it in the
   Binance App and track the order. Do not claim success before the transaction
   receipt is successful.

For an ERC-20 payment, a separate `approve` is another external contract call.
Preview it, show the exact allowance and spender, get a separate confirmation,
execute it, and only then preview the buy.

## Sell

Selling before the pool opens is unavailable. After the official Pancake pair is
open, use the official `binance-agentic-wallet` market-order skill:

```bash
baw market-order quote \
  --fromTokenQty <tokenAmount> \
  --fromToken <launchToken> \
  --toToken 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE \
  --binanceChainId 56 \
  --json
```

Read the token's sell tax and never try to sell the entire displayed balance
without leaving tax headroom. Show the quote, taxes, price impact, minimum
received, and risks. Wait for explicit confirmation before `market-order swap`.

## Forbidden execution paths

- Never use `cast send`, `forge script --broadcast`, viem `writeContract`, or any
  private-key signer for a Binance Agentic Wallet order.
- Never ask the user to import an Agentic Wallet private key.
- Never skip `contract-call preview`.
- Never execute from an intercepted or failed preview.
- Never silently replace an unavailable custom-contract buy with an immediate
  market order.

## Install

Install the official skill first:

```bash
npx skills add binance/binance-skills-hub/skills/binance-web3/binance-agentic-wallet
```

Install this skill:

```bash
npx skills add https://github.com/Gtokentool/gtokentool-ai-skills
```

After installation, use natural language. The user does not need to provide an
Agent ID number, API key, or private key.
