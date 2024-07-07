# Login with ZK Email: OAuth API

## ZK Email Oauth SDK Documentation

### Overview

The [OauthCore](https://github.com/zkemail/email-wallet/blob/feat/oauth-mvp/packages/ts-sdk/src/oauthClient.ts) SDK provides a set of functionalities to interact with an OAuth-based authentication system. It allows users to request email authentication, complete the authentication process, query OAuth usernames, and perform OAuth sign-in and sign-up operations via an ephemeral key for that browser session.

### Installation

To install the SDK, use the following command:

```bash
npm install ????? (coming soon)
```

### Importing the SDK

To use the `OauthCore` SDK, import it into your project as follows:

```typescript
import OauthCore from 'oauthcore-sdk';
import { PublicClient, Address } from 'viem';
```

### Creating an Instance of OauthCore

To create an instance of `OauthCore`, you need to provide the following parameters:

* `client`: An instance of `PublicClient`.
* `coreAddress`: The address of the core contract.
* `ioauthAddress`: The address of the OAuth contract.
* `relayerHost`: The host URL of the relayer service.

#### Example

```typescript
const publicClient = new PublicClient(/* configuration */);
const coreAddress: Address = '0xYourCoreContractAddress';
const ioauthAddress: Address = '0xYourOauthContractAddress';
const relayerHost: string = 'https://your-relayer-host.com';

const oauthCore = new OauthCore(publicClient, coreAddress, ioauthAddress, relayerHost);
```

### Functions

#### 1. `requestEmailAuthentication`

Requests email authentication for a user.

**Parameters**

* `userEmailAddr`: The email address of the user.

**Example**

```typescript
await oauthCore.requestEmailAuthentication('user@example.com');
```

#### 2. `completeEmailAuthentication`

Completes the email authentication process using the account code.

**Parameters**

* `accountCode`: The account code received via email.

**Example**

```typescript
await oauthCore.completeEmailAuthentication('your-account-code');
```

#### 3. `getOauthUsername`

Retrieves the OAuth username associated with the authenticated user.

**Returns**

* A `Promise` that resolves to the OAuth username.

**Example**

```typescript
const username = await oauthCore.getOauthUsername();
console.log(`OAuth Username: ${username}`);
```

#### 4. `oauthSignup`

Signs up a new user with the given username.

**Parameters**

* `username`: The desired username for the new user.

**Note**

Before calling this function, please ensure that `getOauthUsername` returns an **empty** string, i.e., the user's account has not signed up yet.

**Example**

```typescript
await oauthCore.oauthSignup('newUsername');
```

#### 5. `oauthSignin`

Signs in an existing user with optional parameters.

**Parameters**

* `expiry_time`: The expiry time for the session (optional).
* `is_sudo`: Boolean indicating if the user has sudo privileges (optional).
* `token_allowances`: Array of token allowances (optional).

**Note**

Before calling this function, please ensure that `getOauthUsername` returns a **non-empty** string, i.e., the user's account has already signed up.

**Example**

```typescript
await oauthCore.oauthSignin(null, false, null);
```

#### 6. `oauthExecuteTx`

Executes a transaction on behalf of the authenticated user.

**Parameters**

* `target`: The target address for the transaction.
* `data`: The data payload for the transaction.
* `ethValue`: The amount of Ether to send (optional).
* `token_amount`: The amount of tokens to send (optional).

**Returns**

* A `Promise` that resolves to the transaction hash.

**Example**

```typescript
const txHash = await oauthCore.oauthExecuteTx('0xTargetAddress', '0xDataPayload', null, null);
console.log(`Transaction Hash: ${txHash}`);
```

### Error Handling

Ensure to handle errors appropriately when calling the SDK functions. For example:

```typescript
try {
  await oauthCore.requestEmailAuthentication('user@example.com');
} catch (error) {
  console.error('Error requesting email authentication:', error);
}
```

### Conclusion

The `OauthCore` SDK provides a comprehensive set of functions to manage ZK Email based OAuth-style authentication and transactions. By following the examples provided, you can easily integrate these functionalities into your application.

For more detailed information, refer to the source code or ask questions in the [ZK Email telegram](https://t.me/zkemail).

***

This documentation should help users understand how to create an instance of the `OauthCore` SDK and use its various functions effectively.
