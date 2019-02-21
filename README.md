# MorpheusToken_Review
On 2018-June-22

On short notice, Quantstamp performed an informal audit of Morpheus Network’s updated ERC-20 smart contract. The audit was performed by Senior Research Engineers Kacper Bak and Martin Derka. The smart contract was also reviewed by CTO Steven Stewart. 

Quantstamp's security engineers performed an independent manual review of the source code. The engineers also ran automated checks using static analysis tools, including Oyente and Mythril. No known vulnerabilities were reported. For detailed findings, please review the below report.

## ERC20 race condition
The contract features the "standard" ERC20 race condition vulnerability between approve/transferFrom (which is a flaw in ERC20 design). While not an immediate vulnerability, it could be mitigated by implementing and recommending users to call increase/decreaseApproval() functions. For more information, see https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729.

## Centralization of power
While not a vulnerability, if the contract owner's private key is compromised, then the following issues may arise:

* The contract owner can perform unlimited minting, and, consequently, arbitrarily increase the supply.  It is unclear to us whether the token is intended to have a fixed supply or capped.
* The owner can also freeze and unfreeze transfers at will.

## Best practices and testing
We recommend that the contract developer adopt best practices and use unit tests for better comprehensive coverage. 

## Recommendation
We recommend Morpheus.Network to incorporate the aforementioned changes. 

---

## Methodology

Quantstamp's security engineers performed an independent manual review of the source code. The engineers also ran automated checks using static analysis tools, including Oyente and Mythril. No known vulnerabilities were reported.

Detailed findings

* The “pragma solidity” statement is set to ^0.4.20. We recommend: (1) setting it to the most recent Solidity version; (2) not using the caret symbol, but, instead, specify a set Solidity version.
* We recommend refactoring the code to follow the new syntax (e.g., use constructor() for constructor names)
* We recommend using well-tested OpenZeppelin contracts instead of having custom code for the token.
* We recommend using OpenZeppelin's SafeMath, Ownable and Burnable contracts.
* Conceptually, it is irregular to state that AbstractToken is SafeMath. We recommend to avoid using inheritance in this manner and to follow the pattern of using OpenZeppelin's SafeMath contract.
* There are if/else statements for argument validation. This includes validation of balances and global freeze state. We recommend using require() for this purpose, as well as introducing a function modifier for “frozen” status.
* The function decimals() returns 4. We recommend verifying that this is the correct number of decimals.
* Morpheus.Network should use unit tests.The contract lacks tests. Given the importance of the contract and the previous security breach, we recommend writing a comprehensive test suite.




©2018 Quantstamp, Inc.  All rights reserved. This content shall not be used, copied, modified, redistributed or otherwise disseminated except to the extent expressly permitted by Quantstamp.

DISCLAIMER:  
This report is based on the scope of materials and documentation provided for a limited review at the time provided. Results may not be complete nor inclusive of all vulnerabilities.  The review and this report are provided on an as-is, where-is, and as-available basis. You agree that your access and/or use, including but not limited to any associated services, products, protocols, platforms, content, and materials, will be at your sole risk. Cryptographic tokens are emergent technologies and carry with them high levels of technical risk and uncertainty. The Solidity language itself and other smart contract languages remain under development and are subject to unknown risks and flaws. The review does not extend to the compiler layer, or any other areas beyond Solidity or the smart contract programming language, or other programming aspects that could present security risks.  You may risk loss of tokens, Ether, and/or other loss.  A report is not an endorsement (or other opinion) of any particular project or team, and the report does not guarantee the security of any particular project. A report does not consider, and should not be interpreted as considering or having any bearing on, the potential economics of a token, token sale or any other product, service or other asset.  No third party should rely on the reports in any way, including for the purpose of making any decisions to buy or sell any token, product, service or other asset.  To the fullest extent permitted by law, we disclaim all warranties, express or implied, in connection with this report, its content, and the related services and products and your use thereof, including, without limitation, the implied warranties of merchantability, fitness for a particular purpose, and non-infringement. We do not warrant, endorse, guarantee, or assume responsibility for any product or service referenced, advertised, or offered by a third party through the report, its content, and the related services and products, any open source or third party software, code, libraries, materials, or information linked to, called by, referenced by or accessible through the product, any hyperlinked website, or any website or mobile application featured in any banner or other advertising, and we will not be a party to or in any way be responsible for monitoring any transaction between you and any third-party providers of products or services. As with the purchase or use of a product or service through any medium or in any environment, you should use your best judgment and exercise caution where appropriate.   FOR AVOIDANCE OF DOUBT, THE REPORT, ITS CONTENT, ACCESS, AND/OR USAGE THEREOF, INCLUDING ANY ASSOCIATED SERVICES OR MATERIALS, SHALL NOT BE CONSIDERED OR RELIED UPON AS ANY FORM OF FINANCIAL, INVESTMENT, TAX, LEGAL, REGULATORY, OR OTHER ADVICE.

