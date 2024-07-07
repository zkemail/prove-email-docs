---
description: >-
  A library to do regex verification in circom. It will also generate lookup
  tables compatible with halo2-regex soon.
---

# ZK-Regex

To understand the theory behind the regex circuit compiler, read our [blog post](https://prove.email/blog/zkregex).

### Introduction

This library provides circom circuits that enables you to prove that

* the input string satisfies regular expressions (regexes) specified in the chip.
* the substrings are correctly extracted from the input string according to substring definitions.

This is a Rust adaptation of the V1 JS/Python regex-to-circom work first done by [sampriti](https://github.com/sampritipanda/) and [yush\_g](https://twitter.com/yush\_g), rewritten and majorly expanded to a V2 by [aditya](https://github.com/Bisht13) and [sorasue](https://github.com/SoraSuegami/) in 2024 to use a simpler decomposed specification. You can generate your own regexes via the V1 compiler with our no-code tool at [zkregex.com](https://www.zkregex.com).

In addition to the original work, this library also supports the following features:

* CLI to dynamically generate regex circuit based on regex argument
* Extended regex circuit template supporting:
  * group and negation regexes.
  * a decomposed regex definition, which is the easiest way to define your regex.

You can define a regex to be proved and its substring patterns to be revealed. Specifically, there are two ways to define them:

1. (manual way, v1 only) converting the regex into an equivalent determistic finite automaton (DFA), selecting state transitions for each substring pattern, and writing the transitions in a json file.
2. (automatic way, v2 only) writing a decomposed version of the regex in a json file, and specify which part of the regex is revealed and which is private.
3. (no code way, v1 only) put the regex into zkregex.com > tool, highlight your chosen part, and copy the generated circuit.&#x20;

### How to use

#### Install

Install yarn v1, or run `yarn set version classic` to set the version. Also make sure that [`circom`](https://docs.circom.io/getting-started/installation/) is installed.

Clone the repo and install the dependencies:

```
yarn install
```

#### Compiler CLI

[`zk-regex`](https://github.com/zkemail/zk-regex/) is a CLI to compile a user-defined regex to the corresponding regex circuit. It provides two commands: `raw` and `decomposed`

**`zk-regex decomposed -d <DECOMPOSED_REGEX_PATH> -c <CIRCOM_FILE_PATH> -t <TEMPLATE_NAME> -g <GEN_SUBSTRS (true/false)>`**

This command generates a regex circom from a decomposed regex definition. For example, if you want to verify the regex of `email was meant for @(a|b|c|d|e|f|g|h|i|j|k|l|m|n|o|p|q|r|s|t|u|v|w|x|y|z)+.` and reveal alphabets after @, you can define the decomposed regex as follows.

```
{
     "parts":[
         {
             "is_public": false,
             "regex_def": "email was meant for @"
         },
         {
             "is_public": true,
             "regex_def": "[a-z]+"
         },
         {
             "is_public": false,
             "regex_def": "."
         }
     ]
}
```

Note that the `is_public` field in the second part is true since it is a substring to be revealed. You can generate its regex circom as follows.

1. Make the above json file at `./simple_regex_decomposed.json`.
2. Run `zk-regex decomposed -d ./simple_regex_decomposed.json -c ./simple_regex.circom -t SimpleRegex -g true`. It outputs a circom file at `./simple_regex.circom` that has a `SimpleRegex` template.

**`zk-regex raw -r <RAW_REGEX> -s <SUBSTRS_JSON_PATH> -c <CIRCOM_FILE_PATH> -t <TEMPLATE_NAME> -g <GEN_SUBSTRS (true/false)>`**

This command generates a regex circom from a raw string of the regex definition and a json file that defines state transitions in DFA to be revealed. For example, to verify the regex `1=(a|b) (2=(b|c)+ )+d` and reveal its alphabets,

1. Visualize DFA of the regex using [this website](https://zkregex.com).
2. Find state transitions matching with the substrings to be revealed. In this case, they are `2->3` for the alphabets after `1=`, `6->7` and `7->7` for those after `2=`, and `8->9` for `d`.
3.  Make a json file at `./simple_regex_substrs.json` that defines the state transitions. For example,

    ```
    {
        "transitions": [
            [
                [
                    2,
                    3
                ]
            ],
            [
                [
                    6,
                    7
                ],
                [
                    7,
                    7
                ]
            ],
            [
                [
                    8,
                    9
                ]
            ]
        ]
    }
    ```
4. Run `zk-regex raw -r "1=(a|b) (2=(b|c)+ )+d" -s ./simple_regex_substrs.json -c ./simple_regex.circom -t SimpleRegex -g true`. It outputs a circom file at `./simple_regex.circom` that has a `SimpleRegex` template.

#### Circuit Usage

The generated circuit has

* 1 template arguments:
  * `msg_bytes`: the number of characters for the input string.
* 1 input signals:
  * `msg[msg_bytes]`: the input message to match against
* 1 + (the number of substring patterns) output signals:
  * `out`: a bit flag to identify whether the substring of the input string matches with the defined regex.
  * `reveal(i)[msg_bytes]`: The masked version of `msg[msg_bytes]`. Each character in `msg[msg_bytes]` is turned to zero in `reveal(i)[msg_bytes]` if it does not belong to the i-th substring pattern.

For more examples in action, please checkout the test cases in the `./packages/circom/circuits/common` folder.

#### Helper APIs

A package in `./packages/apis` provides nodejs/rust apis helpful to generate inputs of the regex circuits.

### Development

Welcome any questions, suggestions or PRs!

#### Testing

```bash
yarn test
```

### Cite this Work

Use this bibtex citation.

```
@misc{zk-regex,
  author = {Gupta, Aayush and Panda, Sampriti and Suegami, Sora},
  title = {zk-regex},
  year = {2022},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/zkemail/zk-regex/}},
}
```
