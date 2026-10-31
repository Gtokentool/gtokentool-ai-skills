# AI Rune contract interface

Use read-only JSON-RPC, `cast call`, or viem for these reads. Never use those
tools to sign or broadcast the Agentic Wallet transaction.

```text
_enableMint() -> bool
_price() -> uint256
_singleAmount() -> uint256
_alreadyMint() -> uint256
_max() -> uint256
_limitAddr() -> uint256
_userLimit(address wallet) -> uint256
_token() -> address
owner() -> address
agentIdCollection() -> address
isAgentEligible(address wallet) -> bool
getDetail() -> bytes
```

`_max == 0` means no total cap. `_limitAddr == 0` means no per-wallet cap.
Minting remains unavailable when `_enableMint` is false or `_price` is zero.

The exact payment is:

```text
payment = _price * shares
```

The transfer must be an exact multiple of `_price`. The rune contract's
`receive()` is the mint entry point and requires no calldata.

## Eligibility

Use the official command:

```bash
baw wallet agent-id --json
```

Require `claimed: true`. Do not ask the user for an Agent ID token number or
attempt to derive one from a wallet address.

The contract performs the final check against `agentIdCollection`. A wallet
without at least one token from that collection reverts with `NotEligible()`.

## Management methods

These methods are creator-only and are not part of the mint flow:

```text
setEnableMint(bool)
transferOwnership(address)
renounceOwnership()
```

Do not call management methods from this skill.
