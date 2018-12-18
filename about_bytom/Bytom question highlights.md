#   比原开发社区常见问题集锦


#### 一.基本信息指引
##### 1.比原链(Bytom)项目地址：[Github](https://github.com/Bytom/bytom) / [Gitee](https://gitee.com/BytomBlockchain/bytom)
##### 2.比原链(Bytom)开发文档：[Wiki](https://github.com/Bytom/wiki) / [Docs](https://docs.bytom.io/)
##### 3.比原链社区项目一览(持续更新)：[社区项目一览](https://my.oschina.net/u/3886279/blog/1932757)
##### 4.区块链浏览器：[区块链浏览器](https://blockmeta.com/)/[测试网浏览器]( http://52.82.26.101:8082/)
##### 5.水龙头(领取测试BTM):[水龙头](http://test.blockmeta.com/faucet.php)
##### 6.比原侧链资料：[比原链侧链源码](https://github.com/Bytom/vapor)
##### 7.Derek读比原：[Derek读比原连接](http://shanhuhai5739.github.io/)
##### 8.剥开比原看源码系列：[剥开比原看源码系列](https://github.com/freewind/unwrap-bytom)


#### 二.合约编译：
##### 1.合约预编译: [合约预编译文档](https://mp.weixin.qq.com/s/yKbPMIALBF4pWPDvoLBH-g)
##### 2.调用接口编译合约：[调用接口编译合约参考](https://docs.bytom.io/mydoc_smart_contract_build.html)



#### 三.常见问题：

##### 1.如何连接远程全节点服务器

远程服务需要本地生成的Access-token，可以通过以下两种方式： ./bytomcli create-access-token test 或者 curl -X POST create-access-token -d '{"id":"test"}' 然后获得access-token：

"created_at": "2018-05-18T16:00:25.284677605+08:00", "id": "test", "token":"test:fe50927ddaa5bcca77021e9f50fa5ef236a6140c012d1fe2eb9241f61a9228e4

test是账户，fe50927ddaa5bcca77021e9f50fa5ef236a6140c012d1fe2eb9241f61a9228e4是密码

postman的方式，设置Authorization为Basic Auth，然后填写账户名和密码

Java代码调用：

String auth = Username + ":" + Password;
byte[] encodedAuth = Base64.encodeBase64(auth.getBytes(Charset.forName("US-ASCII")));
String authHeader = "Basic " + new String(encodedAuth);
Map<String, String> header = new LinkedHashMap<String, String>();
header.put("Authorization", authHeader);

##### 2.错误：{"status":"fail","msg":"tx rejected: checking result 0: checking output source: checking value source: checking mux source 0: checking value source: checking issuance program: pushing initial argument 0: run limit exceeded"} ，

交易费gas给少了

##### 3.reservation found outputs already reserved **

表示该账户的utxo被暂时缓存，建议隔几分钟后再发该交易，一般是交易密码错误

##### 4.build里面那个ttl是干嘛用的？

ttl表示utxo的缓存时间， reservation found outputs already reserved, 这个错误对应的时间，time_range 是为了延迟交易上链的一个时间戳，ttl 为 0 的话会采用默认的时间，大概2两个块的时间（五分钟），超过ttl不能重新 build-transaction

##### 5.一笔交易最大可以支持多少上链数据？

上链数据和gas上限有关，现在比原链数据上限为175000字节 = 170 KB

##### 6.如何配置区块数据到指定位置

$ ./bytomd node --mining --home <config_and_data_path>


##### 7.build transaction 这个接口，可以离线构建吗？

##### 8.retie销毁后 币是去哪里了 打到一个黑洞地址吗？
retire是可以销毁BTM的，但是不能合约锁定BTM

##### 9.只要是自己控制的资产，无论 btm 还是 token，都可以使用 retire 销毁

##### 10.比原后期会增发吗？
不会增发
##### 11.有了 txId 不代表交易成功 还得有blockHeight 才说明 交易被打包成块

##### 12.utxo类型的链一般成功签发交易差不多意味着交易成功
不一定，有可能重复使用相同的utxo

##### 13.为什么交易长时间都不会被确认？
可能 utxo 双花了，等待24小时会自动删除的（双花：即双重支付，指的是在数字货币系统中，由于数据的可复制性，使得系统可能存在同一笔数字资产因不当操作被重复使用的情况）

##### 14.比原建议钱包采用多少confirmation数算安全的呢？
6个
##### 15.合约解锁时，传的raw_value值，如果传的值是中文，是什么转换为ascii的，并非源字符

##### 16.Available UTXOs of account have been reserved 请问一下这个是什么错误呢？
密码错误 或者之前交易没完成 utxo被锁定了

##### 17.交易回滚：回滚会把你的交易utxo退回给你，不会自动重新发起交易

##### 18.retire: 6a0568656c6c6f 转化为 hello 是什么转的？
6a05为前缀，6a是固定的，05表示5字节.
16进制转string有函数直接转的，68656c6c6f用函数直接转成hello,前缀截取掉就行了

##### 19.get transaction 的数据没有aribitrary字段
目前只有Coinbse的Input才有arbitray,普通交易是没有的。arbitrary在交易的retire action中可以使用

##### 20.retire里面有arbitray。 为什么资产上链，或普通交易的时候没有arbitray，只有销毁资产的时候才有？ 有什么作用吗 ？
理论上都可以有，但是考虑到比原是专注于资产管理领域的，自然不希望把比原链搞成乱七八糟的信息都可以随意上链，不然区块体积也会太大，所以选择在销毁资产时可以附加信息。同时将来可能也会控制资产的发布或者通过调节gas来控制附加信息的大小。

##### 21.比原链快速同步的方法： [比原链快速同步](https://mp.weixin.qq.com/s/TX2qK10JjOq4BvEBC4x1cw)

##### 22.我想搭个私网不想用solonet，但是我自己的创始节点怎么搭啊？
自己修改toml.go 添加一个netconfigtmpl

##### 23.怎么在本地构建一笔交易？
用钱包就可以搞定

##### 24.比原交易时必须要付手续费。0手续费的交易时不会被验证通过的

##### 25.github上bytom-gm 和 bytom 是什么区别呀？
gm是比原国密网源码(国密网是一条测试网，不能挖BTM)，算法不一样

##### 26.调用币源多签的 接口是哪个？（想实现，一个多签地址给另一个地址转账；  此多签地址的两个key的密码不同）
调2次sign-transaction， 第二次的raw_transaction填第一次的输出
 
##### 27.比原链高级交易: [比原链钱包高级交易 灵活的资产交互能力](https://mp.weixin.qq.com/s/yQvJ81AqL8Srx8hB3AffoQ)

##### 28.用php写bytom rpc接口的请参考？
[php实现bytom rpc接口参考](https://github.com/Bytom/faucet)

##### 29.“data stack underflow  ”有时候会出现这个错误，是什么原因造成的？
“data stack underflow” 一般是实际输入的合约参数小于合约原本需要输入的参数个数的情况下报错的，比如：你签名没有成功，直接submit交易，就会报这个错误

##### 30.区块重组的时候有必要回溯主链和侧链的所有区块吗？具体的issue   https://github.com/Bytom/bytom/issues/1497 」

存在这个问题。找到分叉点即可。无需遍历所有 

#### 四.notice:

##### 如果你对我们的问题有疑问，欢迎在社区提issues。一起做更多的交流，让我们做的更好。谢谢！
