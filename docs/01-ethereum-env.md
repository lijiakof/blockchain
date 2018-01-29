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
* personal.newAccount(password)
* eth.getBalance(accountAddress)
* personal.unlockAccount(accountAddress, password)
* todo: how
* eth.sendTransaction({from: '', to: '', value: web3.toWei(4, 'ether')})
* miner.stop()
* miner.start()
* eth.getCompilers()
    * eth.compile
* txpool.status
* eth.getBlock('pending', true).transactions

## 创建私有区块链
* 进入 `daibi` 文件夹
* 创建 `genesis.json` 配置文件
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
* 创建创世区块：`geth --datadir "./daibi" init genesis.json`
* 创建私有链：`geth --datadir "./daibi" --nodiscover console 2>>geth.log`
* 使用 RPC 方式运行：
```
geth \
--identity "linoy" \
--rpc \
--rpcaddr "localhost" \
--rpccorsdomain "*" \
--datadir "./daibi" \
--port "30303" \
--nodiscover \
--rpcapi "personal,db,eth,net,web3,miner" \
--networkid 1999 console 2>>geth.log
```
* 创建用户：`personal.newAccount("123")`
* 启动挖矿：`miner.start(1)`
* 打开另外一个命令行窗口，监视区块链日志：`tail -f geth.log`

**注：在日志 `geth.log` 中查看到 `Generating DAG in progress epoch=0 percentage=99 elapsed=3m4.183s` 的进度到达 100% 时，挖矿会开始正式开启，此时你的第 0 个账户才会开始有余额**

### `genesis.json` 配置字段解释：
* mixhash：与 nonce 配合用于挖矿，由上一个区块的一部分生成的 hash；
* nonce: nonce 就是一个64位随机数，用于挖矿；
* difficulty: 设置当前区块的难度，如果难度过大，cpu 挖矿就很难，这里设置较小难度；
* alloc: 用来预置账号以及账号的以太币数量，因为私有链挖矿比较容易，所以我们不需要预置有币的账号，需要的时候自己创建即可以；
* coinbase: 矿工的账号；
* timestamp: 设置创世块的时间戳；
* parentHash: 上一个区块的 hash 值，因为是创世块，所以这个值是 0；
* extraData: 附加信息，随便填，可以填你的个性信息；
* gasLimit: 该值设置对GAS的消耗总量限制，用来限制区块能包含的交易信息总和，因为我们是私有链，所以填最大。 