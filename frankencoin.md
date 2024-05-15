## Introduction to Frankencoin

### What is Frankencoin?

The Frankencoin (ZCHF), an oracle-free, collateralized Swiss Franc stablecoin that is intended to track the value of the Swiss Franc. Our mission is to create a stablecoin that addresses the current issues faced by existing stablecoins. These issues include trusting centralized third parties, dependence on centralized oracles, and limitations in the types of collateral available to mint the stablecoin.

It's governance is decentralized, with anyone being able to propose new minting mechanisms and anyone who contributed more than 3% to the stability reserve being able to veto them. So far, the Frankencoin has two approved minting mechanism. Both are accessible through the [frontend](https://frankencoin.com). One is a simple swap contract to convert XCHF (CryptoFranc, fiat-collateralized stablecoin) into ZCHF and back. The other is a novel collateralized minting mechanism based on auctions.

Unlike the minting mechanisms of other collateralized stablecoins, Frankencoin's auction-based mechanism does not depend on external oracles. It is very flexible with regards to the used collateral. In principle, it supports any collateral with sufficient availability on the market. However, its liquidation mechanism is slower than that of other collateralized stablecoins, making it less suitable for highly volatile types of collateral. The name is inspired by the system's self-governing nature.

## Overview

The focus of this audit are the smart contracts that comprise key parts of the Frankecoin app, namely the minting of Frakencoin tokens (ZCHF), the purchase of reserve pool shares, and the stablecoin conversion between XCHF and ZCHF. It is vital that there are no possibilities to mint infinite Frankencoins and that no one's deposited collateral can be stolen. There is a [Gitbook documentation page](https://docs.frankencoin.com/) with much more additional information.

## Scope / Contracts

The following are the relevant source files. The list excludes interfaces and test files.

| Contract | SLOC | Purpose | Mainnet Instance |
| ----------- | ----------- | ----------- | ----------- |
| [contracts/Position.sol](https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/Position.sol) | 227 | Holds the collateral of an individual user for minting fresh Frankencoins. | [0x66541A...](https://etherscan.io/address/0x66541A729046ece8A380C1Fd70462c7b657d9171) (one of potentially many) |
| [contracts/MintingHub.sol](https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/MintingHub.sol) | 190 | A minter that can create and challenge positions. | [0x0E5Dfe...](https://etherscan.io/address/0x0E5Dfe570E5637f7b6B43f515b30dD08FBFCb9ea) |
| [contracts/Equity.sol](https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/Equity.sol) | 142 | The Frankencoin Pool Shares ERC20 token with time-weighted vetoing and a built-in market maker for issuance and redemption. | [0x346206...](https://etherscan.io/address/0x34620625DFfCeBB199331B8599829426a46fd123) |
| [contracts/Frankencoin.sol](https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/Frankencoin.sol) | 137 | The Frankencoin ERC20 token, supporting external minters, keeping track of the minter reserve, and depending on the equity contract for governance. |  [0x7a7870...](https://etherscan.io/address/0x7a787023f6E18f979B143C79885323a24709B0d8) |
| [contracts/ERC20.sol](https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/ERC20.sol) | 78 | An ERC20 token with support for infinite allowances and ERC677 notifications. |
| [contracts/ERC20PermitLight.sol](https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/ERC20PermitLight.sol) | 51 | A lightweight ERC20 permit contract. |
| [contracts/StablecoinBridge.sol](https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/StablecoinBridge.sol) | 48 | A contract to swap other stablecoins with the same base currency 1:1. | [0x4125cD...](https://etherscan.io/address/0x4125cD1F826099A4DEaD6b7746F7F28B30d8402B) |
| [contracts/PositionFactory.sol](https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/PositionFactory.sol) | 29 | A factory for creating new or cloning existing positions. | [0x63cF7c...](https://etherscan.io/address/0x63cF7c82460C5d84d10be2219D80F746D8706b7E) |
| [contracts/MathUtil.sol](https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/MathUtil.sol) | 26 | Mathematical utility functions. |
| [contracts/Ownable.sol](https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/Ownable.sol) | 21 | The usual Ownable contract. |
| Total | 949 |  |  |

Lines of code have been counted with the command

```shell
.\cloc-1.96.exe contracts --exclude-dir=test --by-file --not-match-f='II*'
```

## Points of Interest

To function properly, the Frankencoin depends as much on economic incentives as it does on the security of the smart contracts themselves.

The following are the three cornerstones for making sure the system is robust.

- It should not be possible to mint Frankencoins in a way that the FPS holders disapprove.
- The FPS issuance and redemption mechanism should be path-independent, an attacker should not be possible to drain capital from the equity contract through repeated interaction with the issuance and redemption mechanism.
- Collateralized minting should be safe. I.e. it is impossible for an attacker to steal someone's collateral, it is impossible for a position owner to mint beyond the limit of a position, and it is impossible for a position owner to withdraw collateral without having repaid the corresponding amount of previously minted Frankencoins.

### Out of scope

All the files in the [contracts/test](https://github.com/code-423n4/2023-04-frankencoin/tree/main/contracts/test) folder are only used for testing and out of scope for this audit.

The findings of the previous audit that we decided not to fix should not be reported again.

## Additional Context

There are two mechanisms of particular interest, the issuance and redemption of pool shares, as well as the collateral auction mechanism.

### Auction Mechanism

The novel idea behind the auction mechanism is to two piles of collateral for sale, one from the owner of the position
and one from the challenger that questions the soundness of the position. The final price determines which of the two
the bidder gets. If the price is high enough, the bidder gets the collateral of the challenger. If the price is too low,
the bidder gets the collateral from the position owner. This ensures that the position owner has no incentive to overbid
in order to avert a challenge. Doing so would be costly for them as they would have to buy collateral above the market
price from the challenger as often as the challenger repeats it. The only pre-condition for this to work is that the
collateral asset is available on the market.

### Pool Shares

Anyone can acquire new pool shares at any time through an AMM-inspired mechanism. If the Frankencoin system was a
bank, the FPS tokens would be its shares. They automatically become more valuable as the system earns fees and liquidation
proceeds, but decline in value in case of losses. The bonding curve for issuance and redemption is set such that the
market cap of all pool shares is always three times the equity reserves of the system, with the supply being proportional
to the cubic root of the market cap and the price being proportional to the cubic root squared. The mathematical
implications of having such a bonding curve are beyond the scope of this audit, but it should nonetheless be made sure
that no one can manipulate this system to their advantage.

### Scoping Details

```
- Public code repo: [github.com/frankencoin-zchf](github.com/frankencoin-zchf)
- How many contracts are in scope?: 15
- Total SLoC for these contracts?: 949
- How many external imports are there?: 0  
- How many separate interfaces and struct definitions are there for the contracts within scope?: 6
- Does most of your code generally use composition or inheritance?: Composition
- How many external calls?: 0
- What is the overall line coverage percentage provided by your tests?: 98%
- Is there a need to understand a separate part of the codebase / get context in order to audit this part of the protocol?:  true 
- Please describe required context: There is a [research paper](https://www.snb.ch/n/mmr/reference/sem_2022_06_03_maire/source/sem_2022_06_03_maire.n.pdf) describing the game theory behind the auction mechanism.
- Does it use an oracle?: No
- Does the token conform to the ERC20 standard?: Yes, both tokens.
- Are there any novel or unique curve logic or mathematical models?: There are two novel logics in the system. The first one is an auction mechanism to liquidate collateral. The second one is a built-in bonding curve when creating or redeeming equity tokens.
- Does it use a timelock function?: Yes, for example FPS tokens can only be redeemed after 90 days.
- Is it an NFT?: No
- Does it have an AMM?: Kind of. It offers an AMM-like possibility to mint and redeem Frankencoin Pool Shares (FPS).
- Is it a fork of a popular project?: No  
- Does it use rollups?: No
- Is it multi-chain?: No
- Does it use a side-chain?: No
- Describe any specific areas you would like addressed. E.g. Please try to break XYZ.": It is vital that there are no possibilities to mint infinite Frankencoins and that no one's deposited collateral can be stolen. The bidding process has been carefully designed with game theory in mind such that it does not pay off to manipulate the auction.
- Do you have a preferred timezone for communication?: CET
```

## Tests

The project has been compiled and tested with hardhat, using the following commands:

```shell
npm install
npx hardhat compile
# Includes a gas usage report
npx hardhat test
npx hardhat coverage
```

Alternatively, there are some very basic tests using foundry. Given that foundry (and Rust and Visual Studio Build Tools) are installed, they can be run with the following command:

```shell
forge test
```

Furthermore, you can also interact with the latest set of contracts deployed on mainnet, either through the [frontend](https://frankencoin.com) or by interacting with the contracts directly. The contract addresses can be found in the 'Scope / Contracts' section.

## Links

[Frankencoin App](https://frankencoin.com/)

[Documentation](https://docs.frankencoin.com/)

[Blog](https://frankencoin.substack.com/)

[Forum](https://github.com/Frankencoin-ZCHF/FrankenCoin/discussions)

[Telegram](https://t.me/frankencoinzchf)
