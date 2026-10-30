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
- Factory implementation: `0x672af015d29C0D63CEc2caA935cD4360d9C779D1`
- LaunchToken implementation: `0x4b44D885075a5D083F95a89426A4B91c1CC1d5ea`
- FairMint implementation: `0x5BADBd18c3fb749804Eb1C66d3cfFcF92c810e4C`
- Agent ID collection: `0x60B9262456dE011F7d15a54CdD8c48bAaa8B022F`
- Pancake V2 router: `0x10ED43C718714eb63d5aA57B78B54704E256024E`
- WBNB: `0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c`

Sample project:

- Token `GTAI`: `0x3818c37Eb0FDd29253A0d2AC21c25e51b92587c4`
- FairMint: `0xda8cdF09C9D3f791bd1DF557D7aD3cB120b67B4e`

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
