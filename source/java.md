# 环境要求：
Jdk1.8
# 调用流程
1.	使用智能合约编译工具(solc或remix)编译合约脚本得到bin与abi
2.	调用codegen_chainsql工具生成java类
3.	将Java类导入到工程中，进行合约部署/调用

# 使用codegen_chainsql 工具生成java类
codegen_chainsql.jar 是一个智能合约编译得到的bin与abi数据转化为可调用的java类的可执行jar包，获取链接如下：

https://github.com/ChainSQL/java-chainsql-api/tree/feature/contract/codegen/codegen_chainsql.jar
## 调用格式
```
java -jar codegen_chainsql.jar <input binary file>.bin <input abi file>.abi -p|--package <base package name> -o|--output <destination base directory>

<input binary file>.bin 编译得到的字节码文件（.bin）路径
<input abi file>.abi    编译得到的abi文件路径
–p|--package            指定包名
–o|--output             生成的java文件存储位置
```
示例，合约Greeter.sol经编译后生成Greeter.bin,Greeter.abi文件，通过codegen_chainsql工具生成Greeter.java 文件到 e:/solc 路径下，并且包名为com.peersafe.example:
```
java -jar codegen_chainsql.jar ./Greeter.bin ./Greeter.abi -p com.peersafe.example -o e:/solc
```

# 合约部署及调用
生成合约对应的java类后，就可以进行调用了，首先，需要初始化chainsql对象：
## 1. 初始化chainsql对象：
```
Chainsql c = Chainsql.c;
c.connect("ws://127.0.0.1:6007");
c.as(rootAddress, rootSecret);
```

然后，可用同步/异步两种方式进行合约部署：
## 2. 合约部署
同步
```
//同步调用,共识通过返回
Greeter contract = Greeter.deploy(c,
			 Contract.GAS_LIMIT,Contract.INITIAL_DROPS,
	 "Hello blockchain world!").send();
//输出合约地址
System.out.println("Smart contract deployed to address " + contract.getContractAddress());
```
异步
```
Greeter.deploy(c, Contract.GAS_LIMIT, Contract.INITIAL_DROPS, "hello world",new Callback<Greeter>() {

	@Override
	public void called(Greeter args) {
		 String contractAddress = args.getContractAddress();
		 System.out.println("Smart contract deployed to address " + contractAddress);
	}
		 
});
```
deploy方法说明：
以Greeter合约为例，合约的构造函数为
```
    /* this runs when the contract is executed */
    constructor(string _greeting) public payable{
        greeting = _greeting;
    }
```
对应的deploy方法声明为：
```
 public static Greeter deploy(Chainsql chainsql, BigInteger gasLimit, BigInteger initialDropsValue, String _greeting);
```
参数说明：

参数名 | 描述
---|---
chainsql | Chainsql对象
gasLimit | 用户提供的Gas上限
initialDropsValue | （可选）当合约构造函数被 payable 关键字修饰时，可在合约部署时给合约打系统币（单位为drop)
_greeting | 合约构造函数参数，当参数有多个时，依次向后排列

## 3.合约调用
当合约部署成功后，可调用合约中的其它方法，除了deploy方法可得到合约对象外，通常我们通过load方法，初始化合约对象：
```
//合约加载
Greeter contract = Greeter.load(c,"zKotgrRHyoc7dywd7vf6LgFBXnv3K66rEg", 
	 Contract.GAS_LIMIT);

```
load方法的声明：
```
 public static Greeter load(Chainsql chainsql, String contractAddress, BigInteger gasLimit)
```
参数说明
参数名 | 描述
---|---
chainsql | Chainsql对象
contractAddress | 合约地址
gasLimit | 用户提供的Gas上限
### 3.1 改变合约状态的调用（交易）
合约方法定义：
```
function newGreeting(string _greeting) public {
    emit Modified(greeting, _greeting, greeting, _greeting);
    greeting = _greeting;
}
```
同步调用：
```
JSONObject ret = contract.newGreeting("Well hello again3").submit(SyncCond.validate_success);
System.out.println(ret);
```

异步调用：
```
//set，Lets modify the value in our smart contract
contract.newGreeting("Well hello again3").submit(new Callback<JSONObject>() {
	@Override
	public void called(JSONObject args) {
		System.out.println(args);
	}
});
```
### 3.2 不改变合约状态的调用
合约方法定义：
```
function greet() public view returns (string) {
    return greeting;
}
```
同步：
```
//get
System.out.println("Value stored in remote smart contract: " + contract.greet());

```
异步：
```
greet.greet(new Callback<String>() {

	@Override
	public void called(String args) {
		// TODO Auto-generated method stub
		
	}
	
});
```

## 4. 事件监控
这里只能实时监控正在触发的事件，不能根据indexed的值去过滤之前的log</p>
合约方法定义：
```
/* we include indexed events to demonstrate the difference that can be
captured versus non-indexed */
event Modified(
        string indexed oldGreetingIdx, string indexed newGreetingIdx,
        string oldGreeting, string newGreeting);
```
对应的调用为：
```
contract.onModifiedEvents(new Callback<Greeter.ModifiedEventResponse>() {

	@Override
	public void called(ModifiedEventResponse event) {
	    //todo
		System.out.println("Modify event fired, previous value: " + event.oldGreeting + ", new value: "
				+ event.newGreeting);
		System.out.println("Indexed event previous value: " + Numeric.toHexString(event.oldGreetingIdx)
				+ ", new value: " + Numeric.toHexString(event.newGreetingIdx));
	}

});

```
## 5.通过fallback函数给合约打钱
在solidity中的可以定义一个fallback函数，它没有名字，没有参数，也没有返回值，且必须被 external 关键字修饰，这个方法一般用来在合约未定义转账方法的情况下给合约转账，前提是它必须被 payable 关键字修饰。
```
function() external payable { }
```
Chainsql中通过fallback函数给合约转账的接口为payToContract:
```
Ripple payToContract(String contract_address, String value, int gasLimit)
```
参数说明：
参数名 | 描述
---|---
contractAddress | 合约地址
value | 要转移的ZXC数量，可包含小数
gasLimit | 用户提供的Gas上限

## 6.合约中调用新增的表相关指令
[合约示例](!https://github.com/ChainSQL/java-chainsql-api/blob/feature/contract/chainsql/src/main/resources/solidity/table/solidity-TableTxs.sol)

[对应的java类文件](!https://github.com/ChainSQL/java-chainsql-api/tree/feature/contract/chainsql/src/test/java/com/peersafe/example/contract/DBTest.java)

[测试用例](!https://github.com/ChainSQL/java-chainsql-api/blob/feature/contract/chainsql/src/test/java/com/peersafe/example/contract/TestContractTableTxs.java)

> 说明：调用表相关操作时，submit参数可以传SyncCond.db_success，入库成功后返回