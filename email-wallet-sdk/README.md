# Email-Wallet SDK

The Email Wallet SDK is a set of smart contracts designed to provide functionalities for an Email Wallet. The main feature of this SDK is the handling of commands, which are messages or notes associated with transactions or other operations within the system. It acts as an extension to [Email Wallet](www.emailwallet.org).

For example with this SDK, developers will be able to build their own commands in the subject lines of emails such as:

- "Swap 1 ETH to USDC"
- "Transfer 2ETH to user@ethereum.org"

These subject lines will trigger functions in a smart contract from your email wallet. 



## Main Components

### ExtensionBase.sol
This is a contract that provides structure for defining and managing extensions. Extensions are used to call other smart contracts and trigger specific actions.

### ExtensionQuery.sol
This is an abstract contract that provides a structure for defining and managing query templates. These templates are used to parse the subject line of an email into a format understood by the smart contract.

THe defineQueryTemplate function is used to define the actual query templates and the query function is used to execute the query.

### StringUtils.sol
This library provides various utility functions for string manipulation.

### MemoExtension.sol
This is an example of an extension contract.



[Quick Start>>](./quick_start.md)