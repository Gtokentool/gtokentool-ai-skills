---
name: mint-gtokentool-ai-rune
description: Mint an Agent ID-gated GTokenTool rune launch with Binance Agentic Wallet. Use for a GTokenTool /fwlist project marked Binance AI Only; do not use for unrestricted rune launches or token swaps.
metadata:
  author: GTokenTool
  version: '1.0.0'
  requiredCliVersion: '1.12.0'
  openclaw:
    requires:
      bins:
        - baw
---

# Mint a GTokenTool AI Rune

This skill prepares and submits a native-coin mint for a GTokenTool rune
launch that is gated by a Binance Agent ID. Wallet access, preview, signing and
broadcast always go through the official `binance-agentic-wallet` skill and its
`baw` CLI.

Read [references/interface.md](references/interface.md) before constructing a
preview. Never use `cast send`, a private-key signer, or another wallet tool for
the mint.

## Network

- Execution is supported on BNB Smart Chain mainnet, chain ID `56`.
- A BSC testnet deployment may be inspected, but do not claim that Agentic
  Wallet execution is available there.
- If the project is on another chain, stop and explain that this AI mint flow
  is BSC-only.

## Required details

Stop and ask if either is missing:

- A `/fwlist/<contract>` project URL or the full rune contract address.
- A positive integer number of shares.

Extract the contract from the URL. Never infer a contract from a token name or
ticker.

## Mandatory preflight

1. Run `baw wallet status --json`.
2. Run `baw wallet address --json` and select the BSC address.
3. Run `baw wallet agent-id --json`. Stop if `claimed` is not true.
4. Run `baw wallet settings --json`.
5. If custom contract calls require Developer Mode, verify that
   `devMode.enabled` is true and `devMode.expiresAt` has not passed. If it is
   off, tell the user to enable it in Binance App → Agentic Wallet → Settings.
6. Read the rune contract using [references/interface.md](references/interface.md).
7. Stop unless `agentIdCollection` is non-zero and
   `isAgentEligible(agentWallet)` returns true.
8. Stop unless minting is open and the requested shares fit both `_max` and
   `_limitAddr`.

Never ask for a private key, seed phrase, session token, or Agent ID number.
The official wallet command is the eligibility check.

## Quote

Calculate from live contract state, not from values copied into the website:

```text
payment = _price * requestedShares
tokenAmount = _singleAmount * requestedShares
```

Minting is a plain native transfer to the rune contract. Its `receive()`
performs the final Agent ID check, transfers the project tokens, pays the
project recipient, and pays the platform sale fee.

Before preview, show:

- Rune contract and token contract.
- Agent ID eligibility result.
- Current minted, maximum, wallet used and remaining wallet limit.
- Requested shares, exact native payment, and expected token amount.
- The platform sale fee and project payment split disclosed by simulation.
- That the contract will reject the transfer if eligibility or limits changed.

## Preview and confirmation

Use the official contract-call preview with empty calldata:

```bash
baw contract-call preview \
  --binanceChainId 56 \
  --from <agentWalletAddress> \
  --to <runeContract> \
  --value <exactPaymentWei> \
  --inputData 0x \
  --json
```

Show the parsed transaction, simulation result, exact wallet debit, token
balance change, recipient transfers, and risks. Require a separate explicit
confirmation after a successful preview.

Then execute only the returned request:

```bash
baw contract-call execute --requestId <requestId> --json
```

If the result is `PENDING_CONFIRMATION`, tell the user to approve it in the
Binance App. Do not claim success until the transaction receipt is successful.

## Stop conditions

- The wallet does not hold a Binance Agent ID.
- `_enableMint` is false.
- The requested shares exceed the remaining total or wallet limit.
- The amount is not an exact multiple of `_price`.
- Preview reverts, fails, or reports an unexpected recipient or allowance.
- The user has not explicitly confirmed the final preview.

## Install

Install the official wallet skill first:

```bash
npx skills add binance/binance-skills-hub/skills/binance-web3/binance-agentic-wallet
```

Then install the GTokenTool skills repository:

```bash
npx skills add https://github.com/Gtokentool/gtokentool-ai-skills
```

The user can then ask naturally, for example:

> Mint 1 share of this Binance AI-only GTokenTool rune:
> https://www.gtokentool.com/fwlist/0x...
