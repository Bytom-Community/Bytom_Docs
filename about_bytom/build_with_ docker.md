#  docker搭建比原节点


微服务和容器目前比较流行，相信很多小伙伴都比较熟悉docker, 如果你不是太了解，可以查看文档[docker学习手册](http://www.runoob.com/docker/docker-tutorial.html)。那如何用docker搭建比原链(Bytom)的节点呢?

在操作之前，请自行安装docker。然后在你的终端输入(windows对应cmd)：
    
    docker
   
出现如下图说明你已经安装成功了docker：

![avatar](https://raw.githubusercontent.com/huangxinglong/picture/master/201812/1203/1.png)



获取bytom的docker镜像

    docker pull bytom/bytom:latest
 
用docker images 查看自己下载的bytom镜像  
     
    docker images
    
然后出现如下图说明已经获取到了镜像：

![avatar](https://raw.githubusercontent.com/huangxinglong/picture/master/201812/1203/4.png)



###初始化：
docker run -v < Bytom / data / directory / on / host / machine >：/ root /.bytom bytom：latest bytomd init --chain_id < chainId >

默认的Bytom数据目录（在主机上）是：

Mac： ~/Library/Bytom

Linux： ~/.bytom

windows： %APPDATA%\Bytom

chainId 有三种选择：

mainnet：连接到主网

testnet：连接到测试网

solonet：单节点

如下例（mac/testnet）：

    docker run -v ~/Library/Bytom:/root/.bytom bytom/bytom:latest bytomd init --chain_id testnet
  
###开启docker终端交互模式：
 
    docker run -it -p 9888:9888 -v ~/Library/Bytom:/root/.bytom bytom/bytom:latest

###开启守护进程模式：
 
    docker run -d -p 9888:9888 -v ~/Library/Bytom:/root/.bytom bytom/bytom:latest bytomd node --web.closed --auth.disable
    
 查看正在运行的容器：
 
    docker ps
  
 下图中我们可以看到我们在运行的容器：
 
 ![avatar](https://github.com/huangxinglong/picture/raw/master/201812/1203/2.png)
  
    
 最后在浏览器中请求：<http://0.0.0.0:9888>，可以就可以查看我们钱包。
 
 ![avatar](https://raw.githubusercontent.com/huangxinglong/picture/master/201812/1203/3.png)

 
  
