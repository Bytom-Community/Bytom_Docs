##Bytom contract build



Hello everyone in the technical community. Have you encountered any problems during the development of the contract? For example, in the process of compiling the contract, we can't check whether the compiled contract file is correct in real time, so I will teach you a very convenient method today. You can let your friends check if their contract is written correctly at any time during the writing process.

First of all, we must ensure that we have a go language development environment and the version is higher than 1.8. If you have not built the go language development environment, please Baidu. Make sure the version supported by go is installed correctly:


    $ go version
    $ go env GOROOT GOPATH
         
 Get the source code and compile, reference link：<https://github.com/Bytom/equity>
 
   After compiling, we can execute under equity:
 
     ./equity/equity --help
    
 Get the command help for the contract. The screenshots returned are as follows：
     
 ![avatar](https://raw.githubusercontent.com/huangxinglong/picture/master/20181113/帮助命令.png)
 
The symbols 1, 2, 3, and 4 in the figure indicate the meanings of the parameters carried out by the command. The instance parameter corresponding to 3 in the figure represents the instantiation contract, and the shift corresponding to 4 indicates the specific function in the specified execution contract. Then create a contract file under the project (the contract file is best without any suffix), as shown below：

 ![avatar](https://raw.githubusercontent.com/huangxinglong/picture/master/20181113/创建文件.png)
 

Then write the contract, I am using vim to compile the contract, you can choose to use vim or editor to write the contract. If there is a problem in the process of compiling the contract, please refer to the contract development document: <https://bytom.github.io/mydoc_RPC_call.cn.html> , The picture below is the contract I wrote in vim

 
  ![avatar](https://raw.githubusercontent.com/huangxinglong/picture/master/20181113/创建合约.png)
  
  
  
After the contract is written, if the contract is written incorrectly or there is a syntax error, the following situation will appear. Please check the contract you have written.
   
   
  ![avatar](https://raw.githubusercontent.com/huangxinglong/picture/master/20181113/合约错误.png)
 
After checking, the contract file is executed under the corresponding directory, and then the binary shown in the figure below can be output. Indicate that the contract was written successfully.
  
  
   ![avatar](https://github.com/huangxinglong/picture/raw/master/20181113/编译合约.png)
 
 
Have you found out that it is very simple? Practice it soon! If you have problems during the development process, please contact us in our community.：<https://github.com/Bytom/>
 
        
 


  