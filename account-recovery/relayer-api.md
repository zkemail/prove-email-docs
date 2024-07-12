# Relayer API

You can hit [https://auth.prove.email](https://auth.prove.email) as the base URL for this right now. To see an interactive example of this API in practice, you can see [demo frontend code here](https://github.com/zkemail/email-recovery-demo) and [try the flow yourself here](https://recovery.prove.email/).

## Account Recovery Relayer API Docs

### `/api/echo`

**Method:** GET\
**Path:** `/api/echo`\
**Request Payload:** None\
**Response:** A simple string response saying "Hello, world!"

### `/api/requestStatus`

**Method:** POST\
**Path:** `/api/requestStatus`\
**Request Payload:**

* `request_id`: u32 - The unique identifier for the request.

**Response:**

* Success: JSON serialized `RequestStatusResponse` object with HTTP status 200. Includes `request_id`, `status`, `is_success`, `email_nullifier`, and `account_salt`.
* Failure: Error message with HTTP status 500.

### `/api/acceptanceRequest`

**Method:** POST\
**Path:** `/api/acceptanceRequest`\
**Request Payload:**

* `controller_eth_addr`: String - The Ethereum address of the recovery manager.
* `guardian_email_addr`: String - The email address of the guardian.
* `account_code`: String - The account code.
* `template_idx`: u64 - The index of the template.
* `subject`: String - The subject of the request.

**Response:**

* Success: JSON serialized `AcceptanceResponse` object with HTTP status 200. Includes `request_id` and `subject_params`.
* Failure: Error message with HTTP status 500.

### `/api/recoveryRequest`

**Method:** POST\
**Path:** `/api/recoveryRequest`\
**Request Payload:**

* `controller_eth_addr`: String - The Ethereum address of the recovery manager.
* `guardian_email_addr`: String - The email address of the guardian.
* `template_idx`: u64 - The index of the template.
* `subject`: String - The subject of the request.

**Response:**

* Success: JSON serialized `RecoveryResponse` object with HTTP status 200. Includes `request_id` and `subject_params`.
* Failure: Error message with HTTP status 500.

### `/api/completeRequest`

**Method:** POST\
**Path:** `/api/completeRequest`\
**Request Payload:**

* `account_eth_addr`: String - The Ethereum address of the account.
* `controller_eth_addr`: String - The Ethereum address of the recovery manager.
* `complete_calldata`: String - The calldata to complete the recovery.

**Response:**

* Success: JSON serialized response with HTTP status 200 indicating "Recovery completed" or "Recovery failed".
* Failure: Error message with HTTP status 500.

### `/api/getAccountSalt`

**Method:** POST\
**Path:** `/api/getAccountSalt`\
**Request Payload:**

* `account_code`: String - The account code.
* `email_addr`: String - The email address associated with the account.

**Response:**

* Success: JSON serialized response with HTTP status 200 containing the account salt.
* Failure: Error message with HTTP status 500.
