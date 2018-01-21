# 以太坊区块链开发环境搭建

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