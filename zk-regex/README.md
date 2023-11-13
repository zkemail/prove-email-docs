# Zk Regex
Zk Regex is a unique library that combines the power of Regular Expressions (Regex) and Zero-Knowledge Proofs (ZKPs). Regular Expressions are sequences of characters that form a search pattern, used for pattern matching within strings. This library is a JavaScript and Rust adaptation of the original Python regex-to-circom work by Sampriti and Yush_g, available at zkregex.com.

The Zk Regex library is designed for use with Circom circuits and allows users to construct proofs verifying:

- **Regex Compliance**: An input string adheres to specific regex patterns defined within circom.

- **Substring Extraction Accuracy**: Substrings are precisely extracted from the input string as per defined substring parameters.

This library enhances the foundational work with additional features and user-friendly functionalities.

We have two SDKs zk-regex-circom and zk-regex-compiler.

zk-regex-circom contains regex verification circuits for common regexes.

zk regex-compiler is a compiler to generate a regex verification circuit in circom.



[Quick Start >>](./quick_start.md)
