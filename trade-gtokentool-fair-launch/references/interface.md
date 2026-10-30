# Contract interface

Use raw JSON-RPC, `cast`, or viem. All reads below are `eth_call`.

## FairMint

```text
token() -> address
paymentToken() -> address
officialPair() -> address
opened() -> bool
mintedShares() -> uint256
totalShares() -> uint256
remainingQuota(uint256 agentId) -> uint256
agentIdByWallet(address wallet) -> uint256
inviteBonusByAgentId(uint256 agentId) -> uint256
referrerOfAgentId(uint256 agentId) -> uint256
quoteBuy(address wallet,uint256 paymentAmount,uint256 agentId)
  -> (sharesOut,tokensOut,pricePerShare,platformFee,liquidityAmount,treasuryAmount)
buyWithNative(uint256 minTokensOut,uint256 agentId,uint256 referrerAgentId) payable
buyWithToken(uint256 amount,uint256 minTokensOut,uint256 agentId,uint256 referrerAgentId)
```

`paymentToken == address(0)` means native BNB. Otherwise the order must use
`buyWithToken` and approve the exact FairMint address first.

## Agent ID

For buys:

```text
ownerOf(uint256 agentId) -> address
```

The returned owner must equal the buyer wallet. One wallet is permanently
bound to the first Agent ID it uses, and one Agent ID is bound to the first
wallet that uses it.

## Pancake V2 sale

```text
swapExactETHForTokens(uint256,address[],address,uint256) payable
swapExactTokensForETH(uint256,uint256,address[],address,uint256)
getReserves() -> (uint112,uint112,uint32)
token0() -> address
```

Sell path is `[launch token, WBNB]`. Buy path is `[WBNB, launch token]`.
Read current reserves immediately before quote and simulation.
