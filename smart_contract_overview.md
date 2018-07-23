---
title: Smart Contract Overview
tags: [publishing]
keywords: building, serving, serve, build
last_updated: July 3, 2016
sidebar: mydoc_sidebar
permalink: mydoc_smart_contract_overview.html
folder: mydoc
---

## Overview of equity contract

Equity is a high-level language used by bytom to express contract programs. It is mainly used to describe specific assets on the bytom blockchain:

- All on-chain assets are locked in the contract program. Once the asset value (ie UTXO) is unlocked by a contract, it is only meant to be locked by one or more other contracts.
- The way that the contract protects the asset value (that is, UTXO) is that the transaction must meet the condition through the “verify” command executed through virtual machine.

### Components of contract

  contract ContractName ( parameters ) locks value { clauses }

  - `ContractName` contract name is defined by user
  - `parameters` list of contract parameters, which must conform to the basic type of contract language 
  - `value` The value of the asset (ie UTXO's asset type and corresponding value), which can be customized by the user.
  - `clauses` (ie, functions) (one or more)

### Component of Clause (function)
  clause ClauseName ( parameters ) { statements }

  or

  clause ClauseName ( parameters ) requires name : amount of asset { statements }

  - `ClauseName` name of clauses, which is defined by users
  - `parameters`  list of clause / function parameter
  - `payments` other restrictions that are required to spend UTXO. For example, when performing coin-to-coin transaction, it is necessary to verify that the counterparty of the transaction can provide a corresponding asset value. The major use case of such constraints are contract trading between different types of assets.            

### Component of Statement

  statements contract language，Only `verify`、`lock` and `unlock`
  - the `verify` statement: for example `verify expression`，in which the result of expression must be bool type and true to continue execution
  - the `unlock` statement: for example`unlock value`，in which `value` represents the value of corresponding asset
  - the `lock` statement: for example`lock value with program`，in which `value`represents the value of corresponding asset, and `program` must be the program basic type.

Categories of contract: (`Time` type is temporarily disabled)
    `Amount Asset Boolean Hash Integer Program PublicKey Signature String`

Built-in functions: (The built-in functions related to timing “before” and “after” are temporarily disabled, the alternative is to verify the block height functions “below” and “above”)
- abs(n) returns the absolute value of the value n.
- min(x, y) returns the smallest of the two values ​​x and y.
-  max(x, y) returns the largest of the two values ​​x and y.
- size(s) returns the size of any type of byte.
- concat(s1, s2) returns the string generated from connecting the two strings s1 and s2
- concatpush(s1, s2) concatenates two string-type virtual machine execution opcodes s1 and s2 (ie s2 is joined ​​behind s1) and pushes them onto the stack. The operation function is mainly used for nested contracts.
- below(height) Determines whether the current block height is lower than the parameter height, and returns true if it is, otherwise returns false.
- above(height) Determines whether the current block height is higher than the parameter height, and returns true if it is, otherwise returns false.
- sha3(s) returns the hashing result of byte type string parameter s via SHA3-256. 
- sha256(s) returns the hashing result of byte type string parameter s via SHA-256
- checkTxSig(key, sig) verifies whether the multi-signature of the transaction is correct based on one PublicKey and one Signature.
- checkTxMultiSig([key1, key2, ...], [sig1, sig2, ...]) Verify that the multi-signature of the transaction is correct based on multiple PublicKeys and multiple Signatures.
