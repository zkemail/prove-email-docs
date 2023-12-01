# Package Overviews

This document provides an overview of the three main packages that make up ZK Email Verifier. Each package serves a specific purpose in the process of verifying DKIM signatures in emails using zero-knowledge proofs.

## zk-email/helpers

The @zk-email/helpers package offers a set of utility functions designed to assist in the creation of inputs for zk circuits.

Key considerations:

* This package is essential for generating the necessary inputs for the verification circuits.
* It includes functions for handling RSA signatures, public keys, email bodies, and hashes.
* Developers should familiarize themselves with the generateCircuitInputs function in the `input.helpers.ts` file, which is central to the operation of the SDK.

## zk-email/circuits

The zk-email/circuits package provides pre-built circuits for generating proofs and verifying DKIM signatures. These circuits are designed to be used with the zk-email/helpers package to generate the necessary inputs.

Key considerations:

* the `email-verifier.circom` file is a standard template that can be used for email verification and customized for specific applications
* It processes DKIM headers and employs Regex for pattern matching in emails.
* By default, inputs are kept private unless stated otherwise, while outputs are always made public.
* Upon obtaining the vkey and zkey, you can establish a `verifier.sol` contract, enabling on-chain proof verification!

## zk-email/contracts

The @zk-email/contracts package contains the main contract of the SDK, `DKIMRegistry.sol`. This Solidity contract serves as a registry for storing the hash of the DomainKeys Identified Mail (DKIM) public key for each domain.

Key considerations:

* The `DKIMRegistry.sol` contract maintains a record of the DKIM key hashes for public domains. The hash is calculated by taking the Poseidon hash of the DKIM key split into 9 chunks of 242 bits each.
* The contract provides functions for registering, revoking, and validating DKIM public key hashes.
* It emits events upon successful registration (`DKIMPublicKeyHashRegistered`) and revocation (`DKIMPublicKeyHashRevoked`) of DKIM public key hashes.
* The `DKIMRegistry` contract is used in conjunction with the `EmailVerifier` circuit to verify emails. The `EmailVerifier` circuit checks the DKIM signature of an email against the DKIM public key hash stored in the `DKIMRegistry` contract for the email's domain.

## **Circuit Helpers**

The `circuits` directory includes a `helpers` folder, which houses a variety of Circom helper templates. These templates are instrumental in constructing your primary circuit file.

### **base64.circom**:

The base64.circom file is a part of the zk-email/circuits package and provides functionality for decoding base64 encoded data within arithimetic circuits.

**Overview**

It includes two templates:

* Base64Lookup: Converts a base64 character into its 6-bit binary representation.
* Base64Decode: Decodes a base64 encoded string into binary data.

**Importing**

To use these templates in your Circom program, you need to import the base64.circom file. Here's how you can do it:

```bash
include "path/to/base64.circom"
```

Replace "path/to/base64.circom" with the actual path to the base64.circom file.

### **extract.circom**:

The extract.circom file is part of the zk-email/circuits package. It provides a set of utilities for manipulating signal arrays within arithmetic circuits.

**Overview**

The file includes several templates:

`PackBytes(max_in_signals, max_out_signals, pack_size)`

A template that packs a number of chunks (i.e., number of char signals that fit into a signal) from the input signals into the output signals.

Inputs:

* in: An array of signals to be packed.
* max\_in\_signals: The maximum number of input signals.
* max\_out\_signals: The maximum number of output signals.
* pack\_size: The number of chunks to be packed into a signal.

Outputs:

* out: An array of packed signals.

`VarShiftLeft(in_array_len, out_array_len)`

A template that shifts the input signals left by a variable size of bytes.

Inputs:

* in: An array of signals to be shifted.
* shift: The number of bytes to shift.
* in\_array\_len: The length of the input array.
* out\_array\_len: The length of the output array.

Outputs:

* out: An array of shifted signals.

`VarShiftMaskedStr(in_array_len, out_array_len)`

Similar to VarShiftLeft, but it assumes the input is the masked bytes and checks that shift is the first index of the non-masked bytes.

Inputs:

* in: An array of masked signals to be shifted.
* shift: The number of bytes to shift.
* in\_array\_len: The length of the input array.
* out\_array\_len: The length of the output array.

Outputs:

* out: An array of shifted signals.

`ClearSubarrayAfterEndIndex(n, nBits)`

A template that clears a subarray after a specified end index.

Inputs:

* in: An array of signals.
* end: The end index.

Outputs:

* out: An array of signals with the subarray after the end index cleared.

`ShiftAndPack(in_array_len, max_substr_len, pack_size)`

A template that shifts the input signals left by a variable size of bytes and packs the shifted bytes into fields under a specified pack size.

Inputs:

* in: An array of signals to be shifted and packed.
* shift: The number of bytes to shift.
* in\_array\_len: The length of the input array.
* max\_substr\_len: The maximum length of the substring.
* pack\_size: The number of chunks to be packed into a signal.

Outputs:

* out: An array of shifted and packed signals.

`ShiftAndPackMaskedStr(in_array_len, max_substr_len, pack_size)`

Similar to ShiftAndPack, but it assumes the input is the masked bytes and checks that shift is the first index of the non-masked bytes.

Inputs:

* in: An array of masked signals to be shifted and packed.
* shift: The number of bytes to shift.
* in\_array\_len: The length of the input array.
* max\_substr\_len: The maximum length of the substring.
* pack\_size: The number of chunks to be packed into a signal.

Outputs:

* out: An array of shifted and packed signals.

**Importing**

To use these templates in your Circom program, you need to import the extract.circom file. Here's how you can do it:

```bash
include "path/to/extract.circom"
```

Replace "path/to/extract.circom" with the actual path to the extract.circom file.

Usage Guide >
