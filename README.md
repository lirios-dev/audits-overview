# audits-overview
Overview of participated audits and findings
Some can also be seen on [Code4rena profile page](https://code4rena.com/@Lirios)

| date | Project | platform | finding | classification | type | link |
| --- | --- | --- | --- | --- | --- | --- |
| apr 2022 | Nasdex | Immunefi | Unpriviledged config change | critical | access control | [contract](https://polygonscan.com/address/0xC01bd61922702D06fA0EA91D2672AEba4Cd7E6d3) |
| sep 2022 | NFTuLoan | Immunefi | Loan without collateral | critical | Incomplete Hash for signature verification | [Imunefi](https://bugs.immunefi.com/dashboard/submission/11692) |
| jan 2023 | GoGoPool | Code4rena | DOS, lock node operator funds  | High | DOS, access control | [Github Issue](https://github.com/code-423n4/2022-12-gogopool-findings/issues/792) |
| jan 2023 | NFTfi | Immunefi | Withreaw other user's NFT | critical | Incorrect ownership check | [contract](https://etherscan.io/address/0x18faa7748Bfd533638Aab95c2E26F4df00614aeb) |
| jan 2023 | Biconomy  | Code4rena | EIP1271 _signer validation | High | EIP1271 _signer validation | [Github issue](https://github.com/code-423n4/2023-01-biconomy-findings/issues/334) |
| jan 2023 | Biconomy | Code4rena | Frontrun wallet deployment | High | frontrun risk | [Github issue](https://github.com/code-423n4/2023-01-biconomy-findings/issues/364) |
| jan 2023 | Astaria | Code4rena | Lock NFTs in contract on liquidation | High | calldata validation | [Github issue](https://github.com/code-423n4/2023-01-astaria-findings/issues/521) |
| feb 2023 | Popcorn | Code4rena | Any user can set Fee to 0 | Medium | input validation | [Github issue](https://github.com/code-423n4/2023-01-popcorn-findings/issues/464) |
| feb 2023 | Popcorn | Code4rena | vault.changeAdapter can be misused to drain fees | Medium | access control | [Github issue](https://github.com/code-423n4/2023-01-popcorn-findings/issues/515) |
| mar 2023 | Wenwin | Code4rena | logic issues | Invalid | logic | [Github issue](https://github.com/code-423n4/2023-03-wenwin-findings/issues/221) |
| mar 2023 | Neo Tokio | Code4rena | LP stake points manipulation | High | precision loss | [Github issue](https://github.com/code-423n4/2023-03-neotokyo-findings/issues/16) |
| mar 2023 | Polynomial | Code4rena | DOS Liquiditypool QueuedWithdrawals | High | logic | [Github issue](https://github.com/code-423n4/2023-03-polynomial-findings/issues/103) |
| mar 2023 | Polynomial | Code4rena | DOS KangarooVault QueuedWithdraw | High | logic | [Github issue](https://github.com/code-423n4/2023-03-polynomial-findings/issues/105) |
| mar 2023 | Asymmetry | Code4rena | Bad derivative can lock all funds | Medium | validation | [Github issue](https://github.com/code-423n4/2023-03-asymmetry-findings/issues/545) |
| mar 2023 | Asymmetry | Code4rena | rEth Uniswap price manipulation | High | uniswap spot price | [Github issue](https://github.com/code-423n4/2023-03-asymmetry-findings/issues/555) |
| apr 2023 | Contest 225 | Code4rena | borrowToBuy execution parameters not checked | High | calldata validation | []() |
| apr 2023 | Contest 225 | Code4rena | borrowToBuy wrong collection in transferFrom | High | logic | []() |
| apr 2023 | Contest 225 | Code4rena | borrowToBuy incorrect token approval | Medium | logic | []() |
| apr 2023 | Contest 225 | Code4rena | buyToBorrow incorrect approve | Medium | logic | []() |
| apr 2023 | Contest 225 | Code4rena | buyToBorrow execution data is not validated | High | calldata validation | []() |
| apr 2023 | Rubicon  | Code4rena | batchOffer uses external calls | High | bad implementation | [Github issue](https://github.com/code-423n4/2023-04-rubicon-findings/issues/624) |
| apr 2023 | Rubicon  | Code4rena | batchRequote uses external offer calls | High | bad implementation  | [Github issue](https://github.com/code-423n4/2023-04-rubicon-findings/issues/775) |
| apr 2023 | Rubicon  | Code4rena | incorrect reward calculation | High | logic | [Github issue](https://github.com/code-423n4/2023-04-rubicon-findings/issues/781) |
| apr 2023 | Frankencoin | Code4rena | Challenger reward used to drain reserves | High | logic | [Github issue](https://github.com/code-423n4/2023-04-frankencoin-findings/issues/458) |
| apr 2023 | Frankencoin | Code4rena | Missing revoke minter option | Medium | logic | [Github issue](https://github.com/code-423n4/2023-04-frankencoin-findings/issues/464) |
| jun 2023 | Chainlink CCIP | Code4rena | USDC / offchainTokenData can be manipulated | High | input validation | [Github issue](https://github.com/code-423n4/2023-05-chainlink-findings/issues/671) |
| jun 2023 | Canto | Code4rena | Incorrect maxSwapAmount checked | High | decimal/precision | [Github issue](https://github.com/code-423n4/2023-06-canto-findings/issues/11) |
| jan 2024 | Curves | Code4rena | unprotected setCurves() in FeeSplitter | High | access control | [Github issue](https://github.com/code-423n4/2024-01-curves-findings/issues/514) |
| jan 2024 | reNFT | Code4rena | Gnosos safe Guard can be bypassed | High | Gnosis safe | [Github issue](https://github.com/code-423n4/2024-01-renft-findings/issues/178) |
| feb 2024 | blast.io | Cantina | Missing initialisation data for predeploy tokens | Medium(?) | geth | - |




