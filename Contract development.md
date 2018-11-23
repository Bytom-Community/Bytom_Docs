# Contract development guide



The previous article introduced the method for facilitating the dynamic compilation of contracts. Today I will lead you to the actual development contract. For intuitive and easy understanding, just use the postman request. For contract requests, please refer to the contract document on github:<https://github.com/Bytom-Community/Bytom_Docs/blob/master/contract.md>

Guide: For the first time, please contact the partner of the Bytom smart contract. [Getting started with Bytom Smart Contract](https://bbs.bbug.org.cn/thread-26.htm)和[Getting started with Equity language](https://bbs.bbug.org.cn/thread-23.htm)study，Convenient and better understanding of smart contracts and this article. Quickly get started with contract development.

###The reference contract template is as follows

    contract TradeOffer(assetRequested: Asset,
                    amountRequested: Amount,
                    seller: Program,
                    cancelKey: PublicKey) locks      
                    valueAmount of valueAsset {
    clause trade() {
    lock amountRequested of assetRequested with seller
    unlock valueAmount of valueAsset
    }
    clause cancel(sellerSig: Signature) {
    verify checkTxSig(cancelKey, sellerSig)
    unlock valueAmount of valueAsset
    }
    }

###lock contract
####Step1: request create-account-receiver interface
obtain control_program params，As a parameter when compiling the contract in the third step。

![avatar](https://raw.githubusercontent.com/huangxinglong/picture/master/201811/15/create-account.png)


####Step2: request list-pubkeys interface
obtain pubkeys,In the third step, when compiling the contract, it will be used as a parameter.

![avatar](https://raw.githubusercontent.com/huangxinglong/picture/master/201811/15/list-pubkeys.png)


####Step3: request compile interface
The parameters of the interface are the parameters that are required to be transmitted in the contract template. Program and PublicKey are obtained in the first step and the second step.

![avatar](https://github.com/huangxinglong/picture/blob/master/201811/15/contact.png?raw=true)


In addition to using Postman to compile contracts, you can also use contract code to execute contracts through the command line:<https://github.com/huangxinglong/picture/blob/master/20181113/contract.md>

For example, the TradeOffer contract file we created last time, first open the contract file compilation contract with vim, and then execute the following command to determine whether the contract is correct

      ./equity/equity TradeOffer --bin

 Compile contract：

      ./equity/equity TradeOffer --instance 84fe51a7739e8e2fe28e7042bb114fd6d6abd09cd22af867729ea001c87cd550 1000 0014d6598ab7dce6b04d43f31ad6eed76b18da553e94 7975f3f71ca7f55ecdef53ccf44224d514bc584bc065770bba8dcdb9d7f9ae6c


​      
​      
As shown below：

 ![avatar](https://github.com/huangxinglong/picture/blob/master/201811/15/build_contract_termal.png?raw=true)



####Step4: request build-transaction interface

The program returned by the development and compilation contract of the previous step is used as a parameter, and other parameters are referenced[Bytom development documentation](https://docs.bytom.io/mydoc_rpc_call.cn)

![avatar](https://raw.githubusercontent.com/huangxinglong/picture/master/201811/15/build-transaction.png)

####step5: request sign-transaction interface
The data returned in the previous step is passed as an argument and then gets raw_transaction.

![](https://raw.githubusercontent.com/huangxinglong/picture/master/201811/15/sign-transaction.png)

####Step6: request submit-transaction interface
The raw_transaction obtained in the previous step is used as the incoming parameter. After the request, the tx_id is obtained, and the contract transaction has been submitted.

![](https://raw.githubusercontent.com/huangxinglong/picture/master/201811/15/submit-transaction.png)

###Unlock/cancel contract
####Decode the parameters when the contract is generated
request list-unspent-outputs get the generated contract information to get the program

![](https://raw.githubusercontent.com/huangxinglong/picture/master/201811/15/list-unspent-outputs.png)

####decode-program incoming get generated contract parameter information
![](https://raw.githubusercontent.com/huangxinglong/picture/master/201811/15/decode-program.png)

It should be noted that the value of the code is reversed (there will be a detailed description of the article).

####Unlock/cancel is to remove the third step in the step of generating the contract, and replace the parameter of the fourth step of the call generation contract.

#####The construction parameters for canceling the contract are as follows： 
![](https://github.com/huangxinglong/picture/raw/master/201811/15/cancel-contract-params.png)


#####The parameters of the execution contract are constructed as follows： 
![](https://raw.githubusercontent.com/huangxinglong/picture/master/201811/15/carry%20out%20contract.png)

The build operation is actually the process of specifying input and output. Please check the details [build documents](https://github.com/Bytom/bytom/wiki/Smart-Contract-Build)  和  [api documents](https://github.com/Bytom/bytom/wiki/API-Reference)。

If you have problems during the operation, please contact us in our community: <https://github.com/Bytom/>
