##Realize information on the Bytom                    


Many people who know about Bytom know that Bytom is a public chain project that focuses on the interaction and circulation of information and digital assets in the chain. So how to achieve information on the chain? I used postman to request a demo, and then used golang to write a demo of the interface. Before implementing it with golang code, we need to do some preparatory work. First make sure that you have built a node that is older than the original. If you haven't built a node yet, please refer to the development documentation：<https://github.com/Bytom/bytom/blob/master/README.md> 


Step1：Make sure your account has enough BTM. If you can't get the BTM than the original tap, pick up the address：
<http://test.blockmeta.com/faucet.php>

Step2:Issue your own assets, reference：<http://8btc.com/forum.php?mod=viewthread&tid=242940&extra=>

Step3: The essence of information chaining is actually creating and sending a transaction. We all know that there are three main steps in starting a transaction through api，first build → sign → submit，the corresponding api is：build-transaction、sign-transaction、submit-transaction。Use the postman request process as follows：

Request build-transaction interface: 

![avatar](https://raw.githubusercontent.com/huangxinglong/picture/master/2018%3A11%3A12/036ED528-2D0D-4F9A-A941-15EB3EE9C485.png)
     
Request params：

    {
	"base_transaction": null,
	"actions": [{
		"account_id": "0KTCS3R5G0A02",
		"amount": 10000000,
		"asset_id": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
		"type": "spend_account"
	}, {
		"account_id": "0KTCS3R5G0A02",
		"amount": 100,
		"asset_id": "608037f96e8d1613d900c67a0730cc90e2a03311fb7d091588f7eb551a6103cd",
		"type": "spend_account"
	}, {
		"account_id": "0KTCS3R5G0A02",
		"amount": 100,
		"asset_id": "608037f96e8d1613d900c67a0730cc90e2a03311fb7d091588f7eb551a6103cd",
		"arbitrary": "77656c636f6d65efbc8ce6aca2e8bf8ee69da5e588b0e58e9fe5ad90e4b896e7958c",
		"type": "retire"
	}],
	"ttl": 0,
	"time_range": 1521625823
    }
 
Request sign-transaction interface：

![avatar](https://raw.githubusercontent.com/huangxinglong/picture/master/2018%3A11%3A12/ECB67AB4-A4EE-4DD6-A5C8-4AB51E4224DC.png)

Request params：

    {
	"password": "huangxinglong123",
	"transaction": {
		"allow_additional_actions": false,
		"local": true,
		"raw_transaction": "0701dfd5c8d505020160015e560352e415b41be7648b2241ffdabf56259bc618525f62ac123dce32002110f0ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffc0989fe3020001160014adb6632c5b10c6d5b6f97b8d1250f6e409e11c0101000161015f560352e415b41be7648b2241ffdabf56259bc618525f62ac123dce32002110f0608037f96e8d1613d900c67a0730cc90e2a03311fb7d091588f7eb551a6103cd9cc5b191f3190101160014dcfd9b78c24260823e318153665d511d6c4ecb1b010003013dffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffc0ebbcde02011600147a9baebd37dba3f14960624ed8e6ca3cc9d5f73800013e608037f96e8d1613d900c67a0730cc90e2a03311fb7d091588f7eb551a6103cdb8c4b191f31901160014f0370fdf7a7bec7b34cc62fd5291071a3dc3d9b0000147608037f96e8d1613d900c67a0730cc90e2a03311fb7d091588f7eb551a6103cd6401246a2277656c636f6d65efbc8ce6aca2e8bf8ee69da5e588b0e58e9fe5ad90e4b896e7958c00",
		"signing_instructions": [{
			"position": 0,
			"witness_components": [{
				"keys": [{
					"derivation_path": [
						"0000002c",
						"00000099",
						"0100000000000000",
						"0100000000000000",
						"4600000000000000"
					],
					"xpub": "1c03161a08a4dbb7df153815a28f733fec1ac7579f954c4834e5ce9f0ad8deb260ecb2066a8623b69aa936f5798f4dcb9572bc476f2c8171953ce054d58a759f"
				}],
				"quorum": 1,
				"signatures": null,
				"type": "raw_tx_signature"
			}, {
				"type": "data",
				"value": "4f089176a5bca95ec9227b8a87dfec947c59453805bf46d3f5a18f8032255b5a"
			}]
		}, {
			"position": 1,
			"witness_components": [{
				"keys": [{
					"derivation_path": [
						"0000002c",
						"00000099",
						"0100000000000000",
						"0100000000000000",
						"4700000000000000"
					],
					"xpub": "1c03161a08a4dbb7df153815a28f733fec1ac7579f954c4834e5ce9f0ad8deb260ecb2066a8623b69aa936f5798f4dcb9572bc476f2c8171953ce054d58a759f"
				}],
				"quorum": 1,
				"signatures": null,
				"type": "raw_tx_signature"
			}, {
				"type": "data",
				"value": "67512f9250f559699e32c72c8af29096b1556af145f6ecc0c306e6acc88bbfaa"
			}]
		}]
	}
    }



Request submit-transaction interface：

![avatar](https://raw.githubusercontent.com/huangxinglong/picture/master/2018%3A11%3A12/C99613A2-3B9D-4098-83A0-BFE7E5122FE0.png)


Request params：
     
     {
	 "raw_transaction": "0701dfd5c8d505020160015e560352e415b41be7648b2241ffdabf56259bc618525f62ac123dce32002110f0ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffc0989fe3020001160014adb6632c5b10c6d5b6f97b8d1250f6e409e11c01630240c7004022db674ff2961b540d4edab846d550429ae9a92311ba375a4f452331422961fdcde3bf79631755dd12df409e24a849158d4aeab919cab81520fb7d1e02204f089176a5bca95ec9227b8a87dfec947c59453805bf46d3f5a18f8032255b5a0161015f560352e415b41be7648b2241ffdabf56259bc618525f62ac123dce32002110f0608037f96e8d1613d900c67a0730cc90e2a03311fb7d091588f7eb551a6103cd9cc5b191f3190101160014dcfd9b78c24260823e318153665d511d6c4ecb1b6302406b75ef5a9decfa31d4f5ae06e0fb14ca507ba4a03715874d1d831516945121573b9b858e4d7527d209c1f89f74e0aa4c4e38afd098cbadaff31b9107167099012067512f9250f559699e32c72c8af29096b1556af145f6ecc0c306e6acc88bbfaa03013dffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffc0ebbcde02011600147a9baebd37dba3f14960624ed8e6ca3cc9d5f73800013e608037f96e8d1613d900c67a0730cc90e2a03311fb7d091588f7eb551a6103cdb8c4b191f31901160014f0370fdf7a7bec7b34cc62fd5291071a3dc3d9b0000147608037f96e8d1613d900c67a0730cc90e2a03311fb7d091588f7eb551a6103cd6401246a2277656c636f6d65efbc8ce6aca2e8bf8ee69da5e588b0e58e9fe5ad90e4b896e7958c00"
      }
      
 Response params：
    
     {
    "status": "success",
    "data": {
        "tx_id": "5ef27b930646d468bbb436d3406972ff201aa63702518f777e31dd6a2147dddc"
      }
    }
    
    
Use the tx_id returned above to view the transaction details in the blockchain browser, you can view the data we uploaded.

![avatar](https://raw.githubusercontent.com/huangxinglong/picture/master/2018%3A11%3A12/742EFEDB-5537-45B0-B12D-ABFAD652B826.png)


Reference Code：


    package main

    import (
	"bytes"
	"encoding/json"
	"fmt"
	"io/ioutil"
	"net/http"
    )

    //build-transaction params
    //https://bytom.github.io/mydoc_RPC_call.cn.html#build-transaction
    type BytomAccount struct {
	AccountId string `json:"account_id"`
	Amount    int    `json:"amount"`
	AssetId   string `json:"asset_id"`
	//Arbitrary string `json:"arbitrary"`
	Type string `json:"type"`
    }
    type BytomAccount1 struct {
	AccountId string `json:"account_id"`
	Amount    int    `json:"amount"`
	AssetId   string `json:"asset_id"`
	Arbitrary string `json:"arbitrary"`
	Type      string `json:"type"`
    }

    type BaseTransaction struct{}

    type TransactionParams struct {
	BaseTransaction *BaseTransaction `json:"base_transaction"`
	Actions         []interface{}    `json:"actions"`
	Ttl             int              `json:"ttl"`
	TimeRange       int              `json:"time_range"`
    }

    //sign-transaction params
    //https://bytom.github.io/mydoc_RPC_call.cn.html#build-transaction
    type Transaction struct {
    }

    type SignParams struct {
	Password    string      `json:"password"`
	Transaction Transaction `json:"transaction"`
    }

    //submit-transaction
    //https://bytom.github.io/mydoc_RPC_call.cn.html#build-transaction
    type SubmitParams struct {
	RawTransaction string `json:"raw_transaction"`
    }
    type SubmitResponse struct {
	TxId string `json:"tx_id"`
    }

    func main() {

	account1, account2, account3 := BytomAccount{}, BytomAccount{}, BytomAccount1{}
	account1.AccountId = "0KTCS3R5G0A02"
	account1.Amount = 10000000
	account1.AssetId = "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
	account1.Type = "spend_account"

	account2.AccountId = "0KTCS3R5G0A02"
	account2.Amount = 100
	account2.AssetId = "608037f96e8d1613d900c67a0730cc90e2a03311fb7d091588f7eb551a6103cd"
	account2.Type = "spend_account"

	account3.AccountId = "0KTCS3R5G0A02"
	account3.Amount = 100
	account3.AssetId = "608037f96e8d1613d900c67a0730cc90e2a03311fb7d091588f7eb551a6103cd"
	account3.Arbitrary = "77656c636f6d65efbc8ce6aca2e8bf8ee69da5e588b0e58e9fe5ad90e4b896e7958c"
	account3.Type = "retire"

	//var array
	var actions []interface{}
	//append three params
	array_actions := append(actions, account1, account2, account3)
	transaction_params := &TransactionParams{}
	transaction_params.Actions = array_actions
	transaction_params.Ttl = 0
	transaction_params.TimeRange = 1521625823

	//本地测试网节点
	//build-transaction
	port := "http://127.0.0.1:9888/build-transaction"
	value, err := SendTransactionRetire(transaction_params, port)
	if err != nil {
		fmt.Println("err:", err)
	}

	fmt.Println("build-transaction接口返回的参数:", value)

	//sign-transaction
	//...........

	//submit-transaction
	//...........

    }

    //send post request
    func SendTransactionRetire(params *TransactionParams, port   string) (v interface{}, err error) {
	//以本地测试网节点连接
	ParamsStr, err := json.Marshal(params)
	if err != nil {
		return nil, err
	}

	jsonStr := bytes.NewBuffer(ParamsStr)
	fmt.Println(jsonStr)

	req, err := http.NewRequest("POST", port, jsonStr)
	req.Header.Set("Content-Type", "application/json")
	req.Header.Add("Accept", "application/json")

	client := &http.Client{}
	resp, err := client.Do(req)
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	var bodyBytes []byte
	if resp.StatusCode == 200 {
		bodyBytes, err = ioutil.ReadAll(resp.Body)
		if err != nil {
			return nil, err
		}
	}

	return string(bodyBytes), nil
    }
 
The above code is just a step of build-transaction, and the sign-transaction and submit-transaction requests need to organize the parameters themselves to request. Request the submit-transaction to get the returned transaction hash, go to the blockchain browser to view your own chain information, blockchain browser address：<http://52.82.46.157:8082/>。

Well, through the above four steps, we can use the Bytom to achieve information chaining. If you have any questions or don't understand, please contact us in our community.
