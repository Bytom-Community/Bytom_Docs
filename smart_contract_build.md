---
title: Smart Contract Build
tags: [publishing]
keywords: building, serving, serve, build
last_updated: July 3, 2016
sidebar: mydoc_sidebar
permalink: mydoc_smart_contract_build.html
folder: mydoc

---

## Flow of building a contract

### Parameters of contract


  The contract parameters mainly involve two aspects, one is to compile the parameters in the contract, and the other is to unlock the parameters in the contract clause. The types of contracts have been described briefly. There are only three types of parameter in the API interface for compiling contracts:

  - `boolean` - Boolean contract parameter, including: 是`Boolean`.
  - `integer` - the contract parameter of the integer type, including:`Integer`、`Amount`.
  - `string` - The contract parameter of the string type, including:`String`、`Asset`、`Hash`、`Program`、`PublicKey`.

Note：
  - Compile the contract API interface only rqeuires the parameters in the contract
  - Unlock the contract only requires parameters in the Clause
  - The Signature type can only appear in the parameter list of the Clause and cannot appear in the API of compilile contract.
  - All strings of string type must appear in the form of a string of hexadecimal bytes, otherwise an error will be reported when calling the compile contract API.

If there are “Publickey” and “Signature” in the contract parameters, then these parameters need to be obtained by calling the “list-pubkeys”. Signature can only appear in the Clause, indicating that the parameter will only be used when the contract is unlocked. Since the signature of the transaction must be obtained through the “sign-transaction” interface to obtain the signature result, therefore only the signature parameter “derivation_path”  and “root_xpub” is required for unlocking. It should be noted that these parameters need to match the verification signature “pubkey”, otherwise the contract will also fail.

The parameters of “list-pubkeys” are as follows:

String - account_id, account ID.
The json format of its request and response is as follows:
```js
// Request
{
  "account_id": "0G1JIR6400A02"
}

// Result
{
  "pubkey_infos": [
    {
      "derivation_path": [
        "010100000000000000",
        "0300000000000000"
      ],
      "pubkey": "c37d5531f393bc6a3568628c0c0e17801ea452e75d604deb01403c4b161659a3"
    },
    {
      "derivation_path": [
        "010100000000000000",
        "0200000000000000"
      ],
      "pubkey": "117d12e84bb19e956451e0b1eb2bffc662ecb7aac7e63d77e524ddd467eb3617"
    },
    {
      "derivation_path": [
        "010100000000000000",
        "0100000000000000"
      ],
      "pubkey": "e9108d3ca8049800727f6a3505b3a2710dc579405dde03c250f16d9a7e1e6e78"
    }
  ],
  "root_xpub": "5c6145b241b1147987565719657a0506ebb417a2e110a235a42cfb40951880f447432f930ce9fd1a6b7e51b3ddbfdc7adb57d33448f93c0defb4de630703a144"
}
```

----

### Compile contract
Call the API of compile to compile the contract. If there is a “contract parameter” on the contract statement, you can instantiate the relevant parameters when calling the API of compile contract, so that the program field returned by the compilation will limit the unlocking parameters of the contract, otherwise the returned program is only is the execution step of the contract. In the absence of contract parameters, the user needs to customize the relevant contract parameters, otherwise the contract-locked assets will be exposed to potential leakage.

Parameter：
String - contract, content of contract
Array of Object - args, contract parameter structure (array type).
Boolean - boolean, the boolean type  of contract parameter
Integer - integer, the integer type  of contract parameter.
String - string, the string type of contract parameter.
Take “LockWithPublicKey” as an example, the json format of the request and response is as follows:

```js
// Request
{
  "contract": "contract LockWithPublicKey(publicKey: PublicKey) locks locked { clause unlockWithSig(sig: Signature) { verify checkTxSig(publicKey, sig) unlock locked }}",
  "args": [
    {
      "string": "e9108d3ca8049800727f6a3505b3a2710dc579405dde03c250f16d9a7e1e6e78"
    }
  ]
}

// Result
{
  "name": "LockWithPublicKey",
  "source": "contract LockWithPublicKey(publicKey: PublicKey) locks locked { clause unlockWithSig(sig: Signature) { verify checkTxSig(publicKey, sig) unlock locked }}",
  "program": "20e9108d3ca8049800727f6a3505b3a2710dc579405dde03c250f16d9a7e1e6e787403ae7cac00c0",
  "params": [
    {
      "name": "publicKey",
      "type": "PublicKey"
    }
  ],
  "value": "locked",
  "clause_info": [
    {
      "name": "unlockWithSig",
      "args": [
        {
          "name": "sig",
          "type": "Signature"
        }
      ],
      "value_info": [
        {
          "name": "locked"
        }
      ],
      "block_heights": [],
      "hash_calls": null
    }
  ],
  "opcodes": "0xe9108d3ca8049800727f6a3505b3a2710dc579405dde03c250f16d9a7e1e6e78 DEPTH 0xae7cac FALSE CHECKPREDICATE",
  "error": ""
}
```

----

### Lock contract

The lock contract actually refers the deployment of contract. The essence is to call the “build-transaction” to send the asset to the contract-specific program. Just set the receiver “control_program” to the specified contract. The template for constructing the lock contract transaction is as follows: (Note: contract transactions do not support transactions in which the recipient's assets are BTM at the moment.)
```js
// Request
{
  "base_transaction": null,
  "actions": [
    {
      "account_id": "0G1JIR6400A02",
      "amount": 20000000,
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "type": "spend_account"
    },
    {
      "account_id": "0G1JIR6400A02",
      "amount": 900000000,
      "asset_id": "1e074b22ed7ae8470c7ba5d8a7bc95e83431a753a17465e8673af68a82500c22",
      "type": "spend_account"
    },
    {
      "amount": 900000000,
      "asset_id": "1e074b22ed7ae8470c7ba5d8a7bc95e83431a753a17465e8673af68a82500c22",
      "control_program": "20e9108d3ca8049800727f6a3505b3a2710dc579405dde03c250f16d9a7e1e6e787403ae7cac00c0",
      "type": "control_program"
    }
  ],
  "ttl": 0,
  "time_range": 1521625823
}

// Result
{
  "raw_transaction": "0701dfd5c8d505020161015f150ec246dc739a8c4c3f7b4083ededcb2854ca221e437a49f23ec84c7c47ea80ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8099c4d599010001160014726e902e30525e01f0157f12be476c904060383b01000160015ed53c1f3388681f62ae778ac8a54c2b091bbdc91d68ec1e94b20aa2183484f8331e074b22ed7ae8470c7ba5d8a7bc95e83431a753a17465e8673af68a82500c2280c8afa02501011600145de3c504b41019d11698d572b1a37d9a4c9118c1010003013effffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff80bfffcb990101160014310c2265e8e3b7057a62caf09a9f907763f369ea00013d1e074b22ed7ae8470c7ba5d8a7bc95e83431a753a17465e8673af68a82500c2280f69bf32101160014d0d18752a276c94b25f920b02a8edff251b16b7600014f1e074b22ed7ae8470c7ba5d8a7bc95e83431a753a17465e8673af68a82500c2280d293ad03012820e9108d3ca8049800727f6a3505b3a2710dc579405dde03c250f16d9a7e1e6e787403ae7cac00c000",
  "signing_instructions": [
    {
      "position": 0,
      "witness_components": [
        {
          "type": "raw_tx_signature",
          "quorum": 1,
          "keys": [
            {
              "xpub": "5c6145b241b1147987565719657a0506ebb417a2e110a235a42cfb40951880f447432f930ce9fd1a6b7e51b3ddbfdc7adb57d33448f93c0defb4de630703a144",
              "derivation_path": [
                "010100000000000000",
                "0100000000000000"
              ]
            }
          ],
          "signatures": null
        },
        {
          "type": "data",
          "value": "e9108d3ca8049800727f6a3505b3a2710dc579405dde03c250f16d9a7e1e6e78"
        }
      ]
    },
    {
      "position": 1,
      "witness_components": [
        {
          "type": "raw_tx_signature",
          "quorum": 1,
          "keys": [
            {
              "xpub": "5c6145b241b1147987565719657a0506ebb417a2e110a235a42cfb40951880f447432f930ce9fd1a6b7e51b3ddbfdc7adb57d33448f93c0defb4de630703a144",
              "derivation_path": [
                "010100000000000000",
                "0700000000000000"
              ]
            }
          ],
          "signatures": null
        },
        {
          "type": "data",
          "value": "54df681ac3174a11d4456265641a204e04f64b8f860f37bf5584cf4187f54e99"
        }
      ]
    }
  ],
  "allow_additional_actions": false
}
```

After the deployment is successful, the transaction can be signed “sign-transaction” (the signs of successful signature is that all the signature fields in the “signing_instructions” containing the signature type have values and the number is equal to the value of quorum), and then submit the transaction “submit-transaction” to In the trading pool, waiting for the transaction to be confirmed. 

----

### Find contract utxo

After the deployment transaction is successfully sent, then you need to unlock the contract-locked assets, you need to find the contract UTXO before unlocking the contract.

You can find it by calling the API  “list-unspent-outputs”. In this case, you must set “smart_contract” to true, otherwise you will not find it. The parameters are as follows:

String - id, the outputID corresponding to UTXO.
Boolean - smart_contract, whether to display the contract UTXO, which is not displayed by default.
The corresponding input and output results are as follows:
```js
// Request
curl -X POST list-unspent-outputs -d '{"id": "413d941faf5a19501ab4c06747fe1eb38c5ae76b74d0f5af524fc40ee6bf7116", "smart_contract": true}'

// Result
{
  "account_alias": "",
  "account_id": "",
  "address": "",
  "amount": 900000000,
  "asset_alias": "GOLD",
  "asset_id": "1e074b22ed7ae8470c7ba5d8a7bc95e83431a753a17465e8673af68a82500c22",
  "change": false,
  "control_program_index": 0,
  "id": "413d941faf5a19501ab4c06747fe1eb38c5ae76b74d0f5af524fc40ee6bf7116",
  "program": "20e9108d3ca8049800727f6a3505b3a2710dc579405dde03c250f16d9a7e1e6e787403ae7cac00c0",
  "source_id": "c9680e6dd5e9ae7f825fe7edab9fa35c119eb7feab0ab4e426c84a579daf4ef9",
  "source_pos": 2,
  "valid_height": 0
}
```
After finding the corresponding contract UTXO, the parameter information of the contract can be parsed through the API interface “decode-program”, and the user can judge whether the contract can be unlocked according to the existing parameter information.

----

### Unlock contract

Unlock (unlock) contract, that is, call the contract, the essence is to add the corresponding contract parameters to the transaction so that the contract program can be successfully executed in the virtual machine. Currently, the contract-related parameters can be added through array parameter “arguments” in the “send_account_unspent_output “ of the action structure , in which there are two types of parameters:

- rawTxSigArgument: signs related parameter mainly including the main public key “xpub” and its corresponding derived path “derivation_path”, and the publickey to be verified is generated by the primary public key and the child public key generated through the derived path (these parameters can be accessed through the API interface “list- Pubkeys”)
	- Xpub master public key
	- derivation_path : path, in order to form a sub-private key and a sub-public key
- dataArgument : Other types of parameters whose value is the string format of the []byte type 

Taking the contract “LockWithPublicKey” as an example, the template for unlocking a contract transaction is as follows:
```js
// Request
{
  "base_transaction": null,
  "actions": [
    {
      "type": "spend_account_unspent_output",
      "output_id": "413d941faf5a19501ab4c06747fe1eb38c5ae76b74d0f5af524fc40ee6bf7116",
      "arguments": [
        {
          "type": "raw_tx_signature",
          "raw_data": {
            "xpub": "5c6145b241b1147987565719657a0506ebb417a2e110a235a42cfb40951880f447432f930ce9fd1a6b7e51b3ddbfdc7adb57d33448f93c0defb4de630703a144",
            "derivation_path": [
              "010100000000000000",
              "0100000000000000"
            ]
          }
        }
      ]
    },
    {
      "type": "control_program",
      "asset_id": "1e074b22ed7ae8470c7ba5d8a7bc95e83431a753a17465e8673af68a82500c22",
      "amount": 900000000,
      "control_program": "0014726e902e30525e01f0157f12be476c904060383b"
    },
    {
      "type": "spend_account",
      "asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "amount": 5000000,
      "account_id": "0G1JIR6400A02"
    }
  ],
  "ttl": 0,
  "time_range": 1521625823
}

// Result
{
  "raw_transaction": "0701dfd5c8d5050201720170c9680e6dd5e9ae7f825fe7edab9fa35c119eb7feab0ab4e426c84a579daf4ef91e074b22ed7ae8470c7ba5d8a7bc95e83431a753a17465e8673af68a82500c2280d293ad0302012820e9108d3ca8049800727f6a3505b3a2710dc579405dde03c250f16d9a7e1e6e787403ae7cac00c001000160015e8412e8e8c359683f1f5f3a7308b084022f1f149dab176e6e6e8daada895d0e29ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffe0c6c6944f00011600141313e974d19f3d37db29a212d75b4c763e42f433010002013d1e074b22ed7ae8470c7ba5d8a7bc95e83431a753a17465e8673af68a82500c2280d293ad0301160014726e902e30525e01f0157f12be476c904060383b00013dffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffa0b095924f011600147076c737d92621e0033899a54d02fa79f362922700",
  "signing_instructions": [
    {
      "position": 0,
      "witness_components": [
        {
          "type": "raw_tx_signature",
          "quorum": 1,
          "keys": [
            {
              "xpub": "5c6145b241b1147987565719657a0506ebb417a2e110a235a42cfb40951880f447432f930ce9fd1a6b7e51b3ddbfdc7adb57d33448f93c0defb4de630703a144",
              "derivation_path": [
                "010100000000000000",
                "0100000000000000"
              ]
            }
          ],
          "signatures": null
        }
      ]
    },
    {
      "position": 1,
      "witness_components": [
        {
          "type": "raw_tx_signature",
          "quorum": 1,
          "keys": [
            {
              "xpub": "5c6145b241b1147987565719657a0506ebb417a2e110a235a42cfb40951880f447432f930ce9fd1a6b7e51b3ddbfdc7adb57d33448f93c0defb4de630703a144",
              "derivation_path": [
                "010100000000000000",
                "0300000000000000"
              ]
            }
          ],
          "signatures": null
        },
        {
          "type": "data",
          "value": "c37d5531f393bc6a3568628c0c0e17801ea452e75d604deb01403c4b161659a3"
        }
      ]
    }
  ],
  "allow_additional_actions": false
}
```

After the deployment transaction is successful, the transaction can be signed “sign-transaction” (the signs of successful signature is that all the signature fields in the “signing_instructions” containing the signature type have values and the number is equal to the value of quorum), and then submit the transaction “submit-transaction” to the trading pool, waiting for the transaction to be broadcast. 

