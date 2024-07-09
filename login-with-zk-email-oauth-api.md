# Login with ZK Email: OAuth API

## ZK Email Oauth SDK Documentation

### Overview

The [ZK Email Oauth](https://github.com/zkemail/email-wallet/tree/feat/oauth-mvp/packages/ts-sdk) SDK provides a set of functionalities to interact with an OAuth-based authentication system. It allows users to perform OAuth sign-in and sign-up operations for natural usernames via an email and broadcast transactions on-chain via an ephemeral key for that browser session. You can see our [demo frontend code here](https://github.com/zkemail/oauth-demo-ui) for more specific implementation details.

### Installation

To install the SDK, use the following command:

```bash
npm install @zk-email/oauth-sdk
```

### Importing the SDK

To use our SDK, import `OauthClient` and related libraries into your project as follows:

```typescript
import { OauthClient } from "@zk-email/oauth-sdk";
import { PublicClient, createPublicClient, Address } from 'viem';
```

### Creating an Instance of OauthClient

To create an instance of `OauthClient`, you need to provide the following parameters:

* `client`: An instance of `PublicClient`.
* `coreAddress`: The address of the core contract.
* `oauthAddress`: The address of the OAuth contract.
* `relayerHost`: The host URL of the relayer service.

#### Example

```typescript
const publicClient = new PublicClient(/* configuration */);
/* For Base Sepolia,
   const publicClient = createPublicClient({
        chain: baseSepolia,
        transport: http("https://sepolia.base.org"),
    });
*/
const coreAddress: Address = '0xYourCoreContractAddress';
const oauthAddress: Address = '0xYourOauthCoreContractAddress';
const relayerHost: string = 'https://your-relayer-host.com';

const oauthClient = new OauthClient(publicClient, coreAddress, oauthAddress, relayerHost);
```

**Note**

When an instance of `OauthClient` is created, a new ephemeral ECDSA key is internally generated.

### Functions

#### Step 1. `setup`

Signs up a new user or signs in an existing user with the given username and configurations of the ephemeral key. It requests the relayer service to send an email to the given user's email address, allowing the user to sign up or sign in the account by replying to the sent email. This reply email will simultaneously creates a new Email Wallet account if the user has not created it.

**Parameters**

* `userEmailAddr`: The email address of the user.
* `username`: The desired username for the new user. This is mandatory for the sign up and optional for the sign in. If the username taken by the existing user is different from the given one, the relayer service automatically sends an email to sign in the taken  username (not the given username).
* `expiryTime`: The expiry time for the ephemeral key (optional). If you set no time limitation, you can set `expiryTime` to `null`.
* `tokenAllowance`: Array of token allowances that the ephemeral key can consume (optional). Each element in the array is defined as  `[number, string]`, where the first and second ones, respectively, denote token amount and names to be displayed in the email subject field. You can set at most three token allowances.

**Returns**

* `Promise<number>`: A number of request ID that the relayer service issues for each sign-up/sign-in request.

**Example**

```typescript
// Case 1: the user of 'user@example.com' sign-ups/sign-ins 'newUserName' and activates the ephemeral key forever with no token allowance. 
const requestId = await oauthClient.setup('user@example.com', 'newUserName', null, null);
// Case 2: the user of 'user@example.com' sign-ups/sign-ins 'newUserName' and activates the ephemeral key until timestamp '1720489011' with token allowances up to 3 ETH and 100 USDC. 
const requestId = await oauthClient.setup('user@example.com', 'newUserName', 1720489011, [[3, "ETH"], [100, "USDC"]]);
// Case 3: the user of 'user@example.com' sign-ins the username that the user already took and activates the ephemeral key until timestamp '1720489011' with token allowances up to 3 ETH and 100 USDC.
const requestId = await oauthClient.setup('user@example.com', null, 1720489011, [[3, "ETH"], [100, "USDC"]]);
```

#### Step 2. `waitEpheAddrActivated`

Waits until the ephemeral key corresponding to the given request ID is activated on-chain, i.e., the proof of the user's email is processed on-chain. **Before calling this function, please ensure that the `setup` function is already called.**

**Parameters**

* `requestId`: The ID number that the relayer service issued for each sign-up/sign-in request.

**Returns**

* `Promise<boolean>`: A `true` value is returned when the corresponding ephemeral key is activated.

**Example**

```typescript
const isActivated = await oauthClient.waitEpheAddrActivated('your-request-id');
```

#### Step 3. `oauthExecuteTx`

Executes a transaction on behalf of the authenticated user. **Before calling this function, please ensure that the** `waitEpheAddrActivated` **function is already called.**

**Parameters**

* `target`: The target address to be called by the Email Wallet contract .
* `data`: The data payload when the Email Wallet contract calls `target`.
* `ethValue`: The amount of Ether to send (optional).
* `token_amount`: The amount of ERC20 tokens to send (optional). This value must be equal to that of the amount argument when the Email Wallet contract calls any of `transfer`, `transferFrom`, and `approve` functions in an ERC20 token as `target; otherwise, it can be omitted.`

**Returns**

* `Promise<string>` an executed transaction hash.

**Example**

```typescript
const txHash = await oauthClient.oauthExecuteTx('0xTargetAddress', '0xDataPayload', null, null);
console.log(`Transaction Hash: ${txHash}`);
```

**Note**

When the `token_amount` is larger than zero, the Email Wallet contract deducts its value from the allowance of `target`. If the deducted value is less than zero, it returns an error. Similarly, when the `ethValue` is larger than zero, that contract deducts its value from the allowance of the Wrapped ETH (WETH) address because the contract converts all incoming native ETH into WETH.

### Error Handling

Ensure to handle errors appropriately when calling the SDK functions. For example:

```typescript
try {
  await oauthClient.oauthExecuteTx('0xTargetAddress', '0xDataPayload', null, null);
} catch (error) {
  console.error('Error requesting email authentication:', error);
}
```

### Conclusion

The `OauthCore` SDK provides a comprehensive set of functions to manage ZK Email based OAuth-style authentication and transactions. By following the examples provided, you can easily integrate these functionalities into your application.

For more detailed information, refer to the source code or ask questions in the [ZK Email telegram](https://t.me/zkemail).

***

This documentation should help users understand how to create an instance of the `OauthClient` SDK and use its various functions effectively.
