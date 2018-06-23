# MorpheusToken_Review
On 2018-June-22

On short notice, Quantstamp performed an informal audit of Morpheus Network’s updated ERC-20 smart contract. The audit was performed by Senior Research Engineers Kacper Bak and Martin Derka. The smart contract was also reviewed by CTO Steven Stewart. 

Quantstamp's security engineers performed an independent manual review of the source code. The engineers also ran automated checks using static analysis tools, including Oyente and Mythril. No known vulnerabilities were reported. For detailed findings, please review the below report.

## ERC20 race condition
The contract features the "standard" ERC20 race condition vulnerability between approve/transferFrom (which is a flaw in ERC20 design). While not an immediate vulnerability, it could be mitigated by implementing and recommending users to call increase/decreaseApproval() functions. For more information, see https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729.

## Centralization of power
While not a vulnerability, if owner's private key is compromised, then the following issues may arise:

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

* The “pragma solidity” statement is set to ^0.4.20.
.. We recommend setting it to the most recent Solidity version.
.. We recommend not using the caret symbol, but, instead, specify a set Solidity version.
* We recommend refactoring the code to follow the new syntax (e.g., use constructor() for constructor names)
* We recommend using well-tested OpenZeppelin contracts instead of having custom code for the token.
* We recommend using OpenZeppelin's SafeMath, Ownable and Burnable contracts.
* Conceptually, it is irregular to state that AbstractToken is SafeMath. We recommend to avoid using inheritance and follow the pattern of using OpenZeppelin's SafeMath contract.
* There are if/else statements for argument validation. This includes validation of balances and global freeze state. We recommend using require() for this purpose, as well as introducing a function modifier for “frozen” status.
* The function decimals() returns 4. We recommend verifying that this is the correct number of decimals.
* Morpheus.Network should use unit tests.The contract lacks tests. Given the importance of the contract and the previous security breach, we recommend writing a comprehensive test suite.






