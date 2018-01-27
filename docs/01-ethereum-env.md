# 以太坊区块链环境搭建

## 以太坊客户端的下载和安装
* https://github.com/ethereum/go-ethereum
* [install homebrew](https://brew.sh/)
* brew tap ethereum/ethereum
* brew install ethereum

## `geth` 命令的使用
* geth -h
* geth account list
* geth console 2>>log
* geth --dev console 2>>log
* geth --datadir './demo' console 2>>log
* geth attach http://10.5.11.56:8545

## web3
* eth.accounts
* eth.blockNumber
* personal.newAccount('123')
* eth.getBalance('0xe65dab2c272a87dd516bd67cc6f76dbc04bccba9')
* personal.unlockAccount('0xe65dab2c272a87dd516bd67cc6f76dbc04bccba9', '123')
* todo: how
* eth.sendTransaction({from: '', to: '', value: web3.toWei(4, 'ether')})
* miner.stop()
* miner.start()
* web3.eth.getCompilers()

## 创建私有区块链
* 进入 daibi 文件夹
* 创建 genesis.json 配置文件
```
{
    "config": {
        "chainId": 10,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
    "alloc": {},
    "coinbase": "0x0000000000000000000000000000000000000000",
    "difficulty": "0x1",
    "extraData": "",
    "gasLimit": "0x2fefd8",
    "nonce": "0x0000000000000042",
    "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "timestamp": "0x00"
}
```
* 创建创世区块：geth --datadir "./daibi" init genesis.json
* 创建私有链：geth --datadir "./daibi" --nodiscover console 2>>geth.log
    * geth --identity "linoy" --rpc --rpcaddr "localhost" --rpccorsdomain "*" --datadir "./daibi" --port "30303" --nodiscover --rpcapi "personal,db,eth,net,web3,miner" --networkid 1999 console 2>>geth.log
* 创建用户：personal.newAccount("123")
* 启动挖矿：miner.start(1)

### genesis.json 配置字段解释：
* mixhash：与nonce配合用于挖矿，由上一个区块的一部分生成的hash。注意他和nonce的设置需要满足以太坊的Yellow paper, 4.3.4. Block Header Validity, (44)章节所描述的条件。.
* nonce: nonce就是一个64位随机数，用于挖矿，注意他和mixhash的设置需要满足以太坊的Yellow paper, 4.3.4. Block Header Validity, (44)章节所描述的条件。
* difficulty: 设置当前区块的难度，如果难度过大，cpu挖矿就很难，这里设置较小难度
* alloc: 用来预置账号以及账号的以太币数量，因为私有链挖矿比较容易，所以我们不需要预置有币的账号，需要的时候自己创建即可以。
* coinbase: 矿工的账号，随便填
* timestamp: 设置创世块的时间戳
* parentHash: 上一个区块的hash值，因为是创世块，所以这个值是0
* extraData: 附加信息，随便填，可以填你的个性信息
* gasLimit: 该值设置对GAS的消耗总量限制，用来限制区块能包含的交易信息总和，因为我们是私有链，所以填最大。 