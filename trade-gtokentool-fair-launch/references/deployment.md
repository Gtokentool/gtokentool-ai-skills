# Supported deployments

## BSC testnet

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
credential. Do not describe it as a production Binance identity check.

No BSC mainnet FairLaunchFactory is registered yet. Stop if an order requests
mainnet and no audited mainnet deployment is configured.
