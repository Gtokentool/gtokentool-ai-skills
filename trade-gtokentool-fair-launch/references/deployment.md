# Supported deployments

## Agentic Wallet network

Binance Agentic Wallet execution is currently available on BSC mainnet,
Binance chain ID `56`. The official `baw contract-call` command does not expose
BSC testnet as an execution network.

If the project is deployed on BSC testnet, it can be tested with a dedicated
test key from the development environment, but it must not be described as
buyable through Binance Agentic Wallet.

## BSC mainnet

- chainId: `56`
- FairLaunchFactory proxy: `0x4750e798F8FC9bD473338934e8FE499350FA1c58`
- Factory implementation: `0x230C10F0fB3152C7A0E7273a83E54A41ed2Fd780`
- LaunchToken implementation: `0x3509f53a769a2797e2268e3309DA8A781890F8DE`
- FairMint implementation: `0x73c5521F8077ef5Ee2F90f760E93e41c532239C1`
- Agent ID collection: `0x60B9262456dE011F7d15a54CdD8c48bAaa8B022F`
- Pancake V2 router: `0x10ED43C718714eb63d5aA57B78B54704E256024E`
- WBNB: `0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c`

Sample project:

- Token `GTAI`: `0xC2058a38773ee8075f46657b6a056b05C7A79a52`
- FairMint: `0x3f5a5E13C0336c71b4565Cf96b72002DD9c372fD`
- Pancake pair: `0xF4f194eA9Cb6542EeA06561Da1007B0995e36caa`

## BSC testnet (contract development only)

- chainId: `97`
- RPC: `https://data-seed-prebsc-1-s1.bnbchain.org:8545`
- FairLaunchFactory: `0x11A232680C80186E5306925948c527795f2cae0F`
- TestAgentIdNFT: `0x98F764d2Ee2A554Cf68013de3afEb2D55Ff1c656`
- LaunchToken implementation: `0xA0631714cCB7407e5fc7ffB5a175426fe791Ecfb`
- FairMint implementation: `0xcD5ecF69Ca5E5244fbb78F3E56710845275A047D`
- Pancake V2 router: `0xD99D1c33F9fC3444f8101754aBC46c52416550D1`
- WBNB: `0xae13d989daC2f0dEbFf460aC112a837C89BAa7cd`

Sample project:

- Token `GTF`: `0x7c8106f3317A980CB7181B5564020fD5c85eF4Ae`
- FairMint: `0x83d2fdDAc9456387352bAD81e511ebC1f8471Ba5`
- Official pair: `0x27a763FF633CF93e3C825aa5de5F7F42B77A8fC7`

Testnet uses `TestAgentIdNFT` because Binance Agent ID is a production
credential. Do not describe it as a production Binance identity check and do not
route it through `baw`.

The factory proxy is the canonical mainnet entry point. Verify the sale exists
in that factory before building any order.
