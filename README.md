# ZK Email Documentation Hub

![Prove Email Logo](.gitbook/assets/ProveEmailLogo.png)

Welcome to the ZK Email Documentation Hub, the central repository for comprehensive documentation on the ZK Email project suite. This hub serves as your one-stop guide to understanding and implementing our email verification solutions powered by zero-knowledge proof technology.

## ZK Email Project Repos

We have several repos within the ZK Email ecosystem. Developers can use these to build their own custom email verification circuits.

[ZK Email SDK](zk-email-sdk/): Use this to build a new kind of ZK Email proofs out of the box with 8 lines of JSON, and automatically have proof infrastructure deployed for you. We recommend this for all developers new to ZK Email.

[ZK Email Verifier](https://github.com/zkemail/zk-email-verify): Build custom circuits, smart contracts, and frontend utils. This SDK will likely require ZK knowledge to be able to use, and gives more customizability than the [ZK Email SDK](zk-email-sdk/).

[Email Wallet](https://github.com/zkemail/email-wallet): A smart contract wallet controlled by your email. You can understand [how to hit the API](email-wallet/api-documentation.md) from a frontend easily, [run your own relayer](email-wallet/relayer-infrastructure.md) if you like, or [define your own extensions](email-wallet/email-wallet-extensions-sdk.md) to add functionality.

[Account Recovery](account-recovery/relayer-api.md): Explains the interfaces to call account recovery functions from a frontend API, to enable and trigger recovery on any Safe or 7579-compatible wallet.

[Oauth Login](login-with-zk-email-oauth-api.md):  Explains how to hit our APIs from a frontend to be able to login via emails with ZK Email, and optionally request scopes like temporary access to assets.

[ZK Regex](https://github.com/zkemail/zk-regex): A library to do regex verification in circom. The zk-regex CLI takes in a regex and outputs a ZK circuit.

## Projects that use ZK Email

There are many real world applications that currently use our sdks. Here's a list of a few notable ones

* [ZKP2P: ](https://zkp2p.xyz/)Decentralized Venmo <> USDC Bridge
* [Email Wallet:](https://emailwallet.org) Send transactions via email or act as a Safe multisig signer!
* [Email Account Recovery](https://prove.email/blog/recovery): Use emails as guardians for any smart wallet or multisig.&#x20;
* [Nozee:](https://nozee.xyz) Anonymous Proofs of Email Domain via JWTs
* [Proof of Twitter:](https://twitter.prove.email/) Verify your twitter username on chain using zk proofs

## Quick Start

To get started with ZK Email, follow these steps:

1. **Choose the Project Component**: Select the project component you're interested in from our list of SDKs, templates, and examples.
2. **Read the README Guide**: Each section comes with a "README" guide. Make sure to read it to understand how the technology works and to get started with the basic setup.
3. **Dive into Specific Documentation**: For detailed information and advanced configurations, delve into the specific documentation of the component you've chosen. Check out our [How to Set Up Proof of Twitter Example](https://prove.email/blog/twitter) blog post for a step-by-step tutorial showcasing a practical example of how our SDKs can be implemented for a custom ZK email circuit, and our [Github](https://github.com/zkemail) to see code for other demos.
4. **Regularly Check for Updates**: Since we frequently make changes to enhance functionality and security, it's crucial to keep your circuits package up-to-date.

Please read our [CONTRIBUTING.md](contributing.md) for details on our how you can contribute to Prove.email, and the process for submitting pull requests to us.

## License

This documentation is licensed under the [MIT License](https://github.com/zkemail/prove-email-docs/blob/docs/LICENSE.md).

## Support and Contact

If you need help or have questions, please join our community chat on [Telegram](https://t.me/zkemail).
