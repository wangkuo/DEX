## 操作步骤
### 安装Cli命令行工具
参考https://docs.binance.org/api-reference/cli.html

```bash
git clone https://github.com/binance-chain/node-binary.git
```

命令行区分主网和测试网，例如下面进入到最新的测试网络命令行

```bash
cd node-binary/cli/testnet/0.5.8.1
```

里面的命令可能没有执行权限，用chmod加上权限

```bash
chmod +x tbnbcli
```



> 以下操作均为测试网络

### 创建 local keystore
```
./tbnbcli keys add test_key
```

使用 `--from`参数来调用相应keystore，例如：

```shell
./tbnbcli send --chain-id=<chain-id> --from=test_key --amount=100:BNB --to=<address>
```



也可以通过网也创建keystore：<https://testnet.binance.org/en/create> 注意保存助记词



列出当前key列表

```
./tbnbcli keys list
```

通过助记词导入keystore

```
./tbnbcli keys add test_key --recover
```





### 获取免费BNB（Testnet）

https://www.binance.com/en/dex/testnet/address

需要登录币安账户，并且账户内至少有1个BNB。每个地址可以冲入200个测试BNB，每个币安账户可以给20个地址充值测试币。



### 发行 Token

```bash
./tbnbcli token issue --token-name "Energy Eco Token" --total-supply 60000000000000000 --symbol EET --from test_key --chain-id=Binance-Chain-Nile --node=data-seed-pre-2-s1.binance.org:80 --trust-node
```

小数位 8 位，发行数量后面需加 8 个 0。

测试网络发行 Token 需支付 400 BNB，正式网络需要 1000 BNB，注意保持余额充足。



### 查询余额及转账

查询余额

```
./tbnbcli account tbnb1xn8vyswpv06a94llpgvmms8jk6t7g353z3umtc --chain-id Binance-Chain-Nile --node=data-seed-pre-2-s1.binance.org:80 --indent
```

转账

```
./tbnbcli send --from test_key2 --to tbnb1t7ftfdsf6gncuwqu7wk9y7katr2ggvfk8j9nt2 --amount 10000000000:BNB --chain-id=Binance-Chain-Nile --node=data-seed-pre-2-s1.binance.org:80 --json --memo "Test transfer"
```



### 上币交易

上币流程文档

<https://docs.binance.org/list_instruction.html>

<https://docs.binance.org/governance.html>



只提交申请不交押金

```bash
./tbnbcli gov submit-list-proposal --from test_key --base-asset-symbol EET-673 --quote-asset-symbol BNB --init-price 100000000 --title "list EET-673/BNB" --description "list EET-673/BNB" --expire-time 1570665600 --chain-id=Binance-Chain-Nile --node=data-seed-pre-2-s1.binance.org:80 --json --voting-period 604800 
```

提交申请同时交押金

```bash
./tbnbcli gov submit-list-proposal --from test_key --deposit 200000000000:BNB
--base-asset-symbol EET-673 --quote-asset-symbol BNB --init-price 100000000 --title "list EET-673/BNB" --description "list EET-673/BNB" --expire-time 1570665600 --chain-id=Binance-Chain-Nile --node=data-seed-pre-2-s1.binance.org:80 --json --voting-period 604800 
```



查询上线申请

```
./tbnbcli gov query-proposal --proposal-id 410 --chain-id=Binance-Chain-Nile --node=data-seed-pre-2-s1.binance.org:80
```



交押金1000BNB

```
./tbnbcli gov deposit --from test_key --proposal-id 410 --deposit 100000000000:BNB --chain-id=Binance-Chain-Nile --node=data-seed-pre-2-s1.binance.org:80
```

主网需要交1000BNB押金，测试网交2000BNB押金



查询投票状态

```
./tbnbcli gov query-votes --proposal-id 410 --chain-id=Binance-Chain-Nile --node=data-seed-pre-2-s1.binance.org:80
```



执行上线交易所命令（需要 proposal 状态为 Passed ）

```
./tbnbcli dex list -s EET-673 --quote-asset-symbol BNB --from test_key --init-price 100000000 --proposal-id 410 --chain-id=Binance-Chain-Nile --node=data-seed-pre-2-s1.binance.org:80 --json
```



## 文档参考

### 币安链Token协议

BEP stands for Binance Chain Evolution Proposal
<https://github.com/binance-chain/BEPs/blob/master/BEP2.md>

### 发行 Token 文档

<https://docs.binance.org/tokens.html>

### 上币文档

<https://docs.binance.org/list_instruction.html>

<https://docs.binance.org/list.html>

### 发送Token文档

<https://docs.binance.org/transfer.html>

### 币安链测试网浏览器

<https://testnet-explorer.binance.org/>

### 上币指南（中英文）

<https://community.binance.org/t/guidelines-on-how-to-list-your-token-on-binance-dex-dex/1596/6>

<https://docs.binance.org/list_instruction.html>
