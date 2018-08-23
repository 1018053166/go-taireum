## **前言** 
我们在以太坊的代码基础上，进行了若干代码模块的重构与开发。
- 重构了以太坊的 P2P 网络通信模块，使其需要进行安全验证得到联盟许可才能加入新节点进入当前 联盟链网络。
-  重构了以太坊的共识算法，只有经过联盟成员认证授权的节点才能打包区块，打包节点按序轮流打 包，无需算力证明。
 
- 开发了联盟共识控制台(Consortium Consensus Console)CCC，方便对联盟链进行运维管理，联盟 链用户只需要在 web console 上就可以安装部署联盟链节点，投票选举新的联盟成员和区块授权打包 节点。

## **编译和安装**
环境准备

golang 最新 https://www.golangtc.com/download

geth客户端编译

    git clone https://github.com/itisaid/go-ethereum.git
    cd go-ethereum
    build/env.sh go run build/ci.go install

CCC控制台编译启动

在编译之前需要安装相关node.

    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
    cd tai/api/;nvm install v8.9 && npm install && node bin/www &
    cd client;npm install && npm dev run

## **验证**
查看相关端口

    netstat -tunlp| egrep "8001|8421"

访问浏览器
    http://localhost:8001

## **Taireum工作流程**

Taireum的启动是一个比较复杂的过程。如果他是一个创始成员，他将执行如下过程
- 1.Taireum客户端编译
- 2.CCC控制台启动（即8001和8421端口启动）
- 3.Taireum依赖CCC后启动
- 4.请求相关路由进行访问 即/api/enode 和/api/mine


## **Taireum启动过程**
我们已经在相关工作流中已经封装了接口。
初始化账号

    geth --datadir datadir account new

创始区块初始化

    geth --datadir datadir init genesis.json

启动进程，启动都时候需要指定一系列相关参数
    geth --mine --nodiscover --datadir datadir--mine --unlock account --password ./passwd --etherbase account --rpc --rpcaddr 127.0.0.1 --rpcport 8545 --rpcapi "web3,eth" --rpccorsdomain "*" --networkid ID

相关参数解析    
| 参数    | 描述 |
|:----------:|-------------|
| **`--mine`** | 开启挖矿|
| `--unlock` | 解锁相关账号 |
| `--etherbase` | 制定挖矿账号 |
| `--rpc` | 开启rpc，默认端口8545 |
| `--rpcapi` | 要开启rpc的相关功能端 |
| `--networkid ` | 网络id，用来区分不同的网络接入 |


## **新成员许可入网流程**

- 1.Taireum客户端编译，联系创始成员得到genesis.json
- 2.CCC控制台启动（即8001和8421端口启动）
- 3.根据genesis.json创建好账号和初始化，联系创始成员，发送enode节点信息和账号地址
- 4.创始成员创建相关信息并且上链，进行二次投票，允许新成员加入。
- 5.新成员在本地CCC控制台写入自身和创始成员enode节点信息
- 6.开始同步区块

## **新成员接入过程**
//todo







































