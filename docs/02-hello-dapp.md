# 开始第一个去中心化应用 Dapp（Decentralized Application）

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

* build：编译好的协议
* contracts：协议的源代码
* migrations：协议部署代码
* test：单元测试
* truffle.js：程序配置文件

## 代码剖析

```
// 使用 solidity 语法版本
pragma solidity ^0.4.18;

// 导入另外一个智能合约
import "./ConvertLib.sol";

contract MetaCoin {
	// 定义 mapping 类型的变量 balances
	// key：address 地址类型：以太坊地址的长度，大小20个字节，160位
 	// value：unit 货币单位类型：一个字面量的数字，可以使用后缀 wei, finney, szabo 或 ether 来在不同面额中转换。不含任何后缀的默认单位是 wei
	mapping (address => uint) balances;

	// 定义事件 Transfer
	// 入参：交易发送方地址
	// 入参：交易接收方地址
	// 入参：转账总额
	// indexed 设置是否被索引。设置为索引后，可以允许通过这个参数来查找日志，甚至可以按特定的值过滤
	event Transfer(address indexed _from, address indexed _to, uint256 _value);

	// 定义公共方法 MetaCoin
	function MetaCoin() public {
		// 给“交易发送方”设置 10000 个以太币
		// tx.origin 为全局变量，表示：交易发送方钱包地址
		balances[tx.origin] = 10000;
	}

	// 定义公共方法 sendCoin
	// 入参：接收者地址, 转账总额
	// 返回：bool 类型
	function sendCoin(address receiver, uint amount) public returns(bool sufficient) {
		// 判断“消息的发送方”的以太币是否小于“转账总额”，小于则返回 false，否则继续执行
		// msg.sender 为全局变量，表示：消息的发送方，即当前调用
		if (balances[msg.sender] < amount) return false;
		
		// 给“消息的发送方”的以太币余额减去“转账总额”
		balances[msg.sender] -= amount;
		// 给“接收方”的以太币余额增加“转账总额”
		balances[receiver] += amount;
		// 执行转账事件
		Transfer(msg.sender, receiver, amount);

		// 返回 true
		return true;
	}

	// 定义公共方法 getBalanceInEth
	// 入参：钱包地址
	// 返回： unit 类型
	function getBalanceInEth(address addr) public view returns(uint){
		// 调用 ConvertLib 对以太币进行转化
		return ConvertLib.convert(getBalance(addr),2);
	}

	// 定义公共方法 getBalance
	// 入参：钱包地址
	// 返回 unit 类型
	function getBalance(address addr) public view returns(uint) {
		// 通过钱包地址获取以太币余额
		return balances[addr];
	}
}

```

## 通过 web3 验证一下结果

```
```

## 用 go-ethereum 来实现



