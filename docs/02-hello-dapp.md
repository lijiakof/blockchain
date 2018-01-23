# 开始第一个去中心化应用 Dapp

## 准备
* solidity
* node npm
* truffle
* ethereumjs-testrpc
* go-ethereum / cpp-ethereum
 
## 安装 truffle 环境
* npm install -g truffle
* npm install -g ethereumjs-testrpc

## 初始化项目
* truffle unbox metacoin
* truffle init

## 编译部署
* truffle compile
* testrpc
* truffle migrate
* truffle test

## 项目结构

```
.
├── build
│   └── contracts
│       ├── ConvertLib.json
│       ├── MetaCoin.json
│       └── Migrations.json
├── contracts
│   ├── ConvertLib.sol
│   ├── MetaCoin.sol
│   └── Migrations.sol
├── migrations
│   ├── 1_initial_migration.js
│   └── 2_deploy_contracts.js
├── test
│   ├── TestMetacoin.sol
│   └── metacoin.js
├── truffle-config.js
└── truffle.js
```

## 



