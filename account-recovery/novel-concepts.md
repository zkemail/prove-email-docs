# Novel Concepts

#### Account Code and Salt

An account code is a random integer in a finite scalar field of BN254 curve. It is a private randomness to derive a CREATE2 salt of the user’s Ethereum address from the email address, i.e., `userEtherAddr := CREATE2(hash(userEmailAddr, accountCode))`. That CREATE2 salt is called account salt, which is published on-chain. **As long as the account code is hidden, no adversary can learn the user's email address from on-chain data.**

#### Invitation Code

An invitation code is a hex string of the account code along with a prefix, contained in any field of the email header to be inherited by its reply, e.g., Subject. By confirming that a user sends an email with the invitation code, the contract can ensure that the account code is available to that user. It ensures the user’s liveness even when a malicious relayer or another user generates the user's account code because it prevents them from withholding the account code. **It suggests that the contract must check if the given email sent by the user contains the invitation code before confirming that user’s account for the first time.**

Notably, the email-auth message, which represents data in the user's email along with its ZK proof, has a boolean field `isCodeExist` such that the value is true if the invitation code is in the email. However, the Subject message in the email-auth message masks characters for the invitation code. **Consequently, no information beyond the existence of the invitation code is disclosed.**

#### Subject Template

A subject template defines the expected format of the message in the Subject for each application. **It allows developers to constrain that message to be in the application-specific format without new ZKP circuits.**

Specifically, the subject template is an array of strings, each of which has some fixed strings without space and the following variable parts:

* `"{string}"`: a string. Its Solidity type is `string`.
* `"{uint}"`: a decimal string of the unsigned integer. Its Solidity type is `uint256`.
* `"{int}"`: a decimal string of the signed integer. Its Solidity type is `int256`.
* `"{decimals}"`: a decimal string of the decimals. Its Solidity type is `uint256`. Its decimal size is fixed to 18. E.g., “2.7” ⇒ `abi.encode(2.7 * (10**18))`.
* `"{ethAddr}"`: a hex string of the Ethereum address. Its Solidity type is `address`. Its value MUST satisfy the checksum of the Ethereum address.

