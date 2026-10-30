# Contract interface

Use read-only JSON-RPC, `cast call`, or viem for the reads and calldata below.
Never use those tools to sign or broadcast an Agentic Wallet order. Signing and
broadcasting goes through `baw contract-call preview` and
`baw contract-call execute`.

## FairMint

```text
token() -> address
paymentToken() -> address
officialPair() -> address
opened() -> bool
mintedShares() -> uint256
totalShares() -> uint256
remainingQuota(address wallet) -> uint256
mintedByWallet(address wallet) -> uint256
inviteBonusByWallet(address wallet) -> uint256
referrerOfWallet(address wallet) -> address
quoteBuy(address wallet,uint256 paymentAmount)
  -> (sharesOut,tokensOut,pricePerShare,platformFee,liquidityAmount,treasuryAmount)
buyWithNative(uint256 minTokensOut,address referrer) payable
buyWithToken(uint256 amount,uint256 minTokensOut,address referrer)
```

`paymentToken == address(0)` means native BNB. Otherwise the order must use
`buyWithToken` and approve the exact FairMint address first.

## Execution

```bash
baw contract-call preview \
  --binanceChainId 56 \
  --from <agentWalletAddress> \
  --to <fairMint> \
  --value <amountInWei> \
  --inputData <calldata> \
  --json

baw contract-call execute --requestId <requestId> --json
```

Developer Mode must be enabled in the Binance App before preview. The user sees
the parsed transaction, simulation, risks, and authority changes, then gives a
fresh confirmation before execute.

## Binance AI wallet eligibility

For buys:

```text
balanceOf(address wallet) -> uint256
```

The buyer wallet must hold at least one token from the configured Binance Agent
ID collection. Quota and referral rewards are tracked by wallet address. Never
ask the user for an Agent ID token number.

## Pancake V2 sale

```text
swapExactETHForTokens(uint256,address[],address,uint256) payable
swapExactTokensForETH(uint256,uint256,address[],address,uint256)
getReserves() -> (uint112,uint112,uint32)
token0() -> address
```

Sell path is `[launch token, WBNB]`. Buy path is `[WBNB, launch token]`.
Read current reserves immediately before quote and simulation.
