![EOSBank](https://github.com/eosonic/EOSBank/blob/master/image/banner.png)

  
## 柚资银行 EOS Bank

柚资银行是EOS网络基础设施，它允许DApp开发者支付极少的利息获得大量CPU计算资源，这些利息将会支付给银行的所有储户，这些租赁交易通过智能合约执行。

### 需求分析
随着EOS生态的发展，EOS计算资源将会变得越来越昂贵，很多开发者的计算需求并不持续，往往存在短时间内爆发出大量计算需求。在世界杯/圣诞节期间，短期的促销需要大量计算资源，为了应对这样的需求，柚资银行作为EOS网络的基础设施，提供计算资源的短期快速租赁。

### 操作说明

##### 1. 租赁
向合约账户：**eosiocpubank** 转入EOS，租赁成功后，不可退租，到期自动回收资源。 
由于EOS网络存在大量发送0.0001个EOS的广告行为，柚资银行支持的最低租赁发送0.01 EOS。 
> cleos transfer 你的账号 **eosiocpubank** "0.1 EOS" "" 

MEMO为空时默认租赁24小时。如需租赁3天或7天请在MEMO写入3d/7d： 
> cleos transfer 你的账号 **eosiocpubank** "0.1 EOS" "3d" 
> cleos transfer 你的账号 **eosiocpubank** "0.1 EOS" "7d" 

##### 2. 储蓄  
向合约账户：**eosiocpubank** 转入EOS,memo写：deposit，最低储值10EOS,每笔获取的分红直接转为储蓄，小于10个EOS会转账失败。
> cleos transfer 你的账号 eosiocpubank "100 EOS" "deposit"

查询余额（储值+分红）
> cleos get table **eosiocpubank** 你的账号 deposit

##### 3. 储户提现
>cleos push action **eosiocpubank** withdraw '["你的账号","10.0000 EOS"]' -p 你的账号 

根据EOS主网的设定，提现延迟3天，自动到账,如需查询提现进度： 
>cleos get table **eosiocpubank** 你的账号 refunds 

### 当前租赁价格

利息 | 获得资源 | 租赁持续 | 
------------ | -------------|-------------
0.1 EOS | 30.4166 EOS CPU | 1天 | 
0.1 EOS | 24.3333 EOS CPU | 3天 | 
0.1 EOS | 18.2500 EOS CPU | 1周 | 
0.1 EOS | 17.3152 EOS CPU | 1周以上 | 

### 储蓄分红结算

一笔租赁交易达成收到的利息为L，我们将会对当前储蓄资产进行快照，设定平台储蓄总余额为N，某客户储蓄为A，则客户根据储蓄数量A，此次租赁立即获得A/N*L利息分红。

举例说明：
我们收到一笔10个币的租赁请求，计算平台目前剩余5000个币，储户在平台存储了100个币，则A/NL=100储蓄/5000总额X10利息=0.2EOS,即：用户由于这一笔租赁立刻收到0.2个币的分红。

### 优势

- #### 强大的资本充足率
柚资银行拥有充足的固定资本，同时吸收EOS小额储蓄，我们支持客户以大于10个EOS的数量储蓄自己的EOS币，我们会把利息收入返还给储蓄者。多样的资金来源，强大的资本保证，充分保障快速变化的租赁需求。

- #### 全自动执行租赁订单
采用智能合约响应租赁请求，任何人可以发送EOS币到合约账户**eosiocpubank**，系统按照当前价格立即租赁CPU给发送的客户。

- #### 先进的租赁周期算法
同时满足客户的临时性资源需求和长周期资源租赁需求，智能合约按照需求的周期自动报价。

- #### 高流动资产委托池
快速的租赁需求使得合约的运行非常繁忙，EOS Bank的银行储蓄资本会拥有极高的周转率。这将给储蓄者带来高额回报，同样也降低了使用者的价格。

- #### 实名制的合约开发者
为了保持资金安全，增强信任，我们公开了团队的身份信息以供查询。

### 银行打烊时间
每天15:00 UTC ，柚子银行将会停止服务半小时。请勿发送任何货币到柚资银行，在维护时间，任何储蓄/租赁请求将会转账失败。

### FAQ
- 为什么我需要租赁CPU而不是购买？   
大部分人并不是每天都需要CPU资源，在短时间内爆发出的CPU需求将会占用大量资金，我们提供1天/3天/1周的短期租赁合约，假设世界杯来临而你需要发送广告，那么发送1个EOS币将会给你提供304.1个币的CPU资源并维持一整天，你无须抵押304.1个EOS并且等待长达3天的赎回期。你甚至可以仅仅发送0.01个币而获得3.041个币的计算资源。

- 我有EOS币，我把币放在柚资银行，我可以获得什么好处？
你将会收到租赁利润，通常的年化收益率将会达到20%甚至30%，每笔交易你都可以获得利润，这些利润在你选择提币后一并退还到你的EOS账号。

- 我将如何开始租赁CPU？  
按照你的需求，向EOS合约**EosioCpuBank**发送一定数量的币，系统会自动抵押CPU资源给你的账号。
0.01个EOS将会获得3.0416个币的CPU资源
0.1个EOS将会获得30.4166个币的CPU资源
1个EOS将会获得304.1660个币的CPU资源

- 我可以给其他账号租赁CPU资源吗？  
当然可以，如果你在MEMO备注里写入账号名称，我们将会把资源抵押在你写的这个账号上。

- 我使用完毕了，我需要归还什么吗？  
你不需要任何操作，智能合约会在租赁到期后自动收回资源。

- 我是客户，你们账户余额是0，我无法租赁资源是怎么回事？  
每一家银行都不是无限资本，在EOS区块链浏览器上搜索账号EosioCpuBank，将会显示柚资银行当前已租赁多少EOS币，银行还剩余多少EOS币可以提供给客户租赁。当账号余额显示为0，柚资银行全部资金都已经被租赁，暂时没有资源可以租赁给客户，我们讲尽快补充资本为客户服务。

- 我是储户，**eosiocpubank**的账户显示余额为0，我的钱在哪里？  
你的EOS被租赁给其他客户了，他们都是安全的，我们采用抵押租赁而不是过户租赁模式，这将不会丢失任何EOS资产。如果你选择赎回，我们将会为你锁定满足你提现额度的EOS币，等待三天赎回期满后发送给你。

- 如何更划算的租赁CPU？  
租赁周期越长租赁花费越低，很显然，租赁一周要比租赁7个1天的花费少得多。这在酒店业都是如此，长期订房显然比只住一天要便宜的多。当然，如果你不需要租赁那么久，依然是需要用的时候临时租赁更加划算。

	
### 开源方式 
- 本文档合约操作流程和合约功能已尽详, 已属于半开源模式。
- 合约hash：5a105361718841eda4e30446bc4afc80a7379876bfccb8e790d9686c828d33dc
- 如需详细资料，[请点击查看](https://eospark.com/MainNet/contract/eosiocpubank)合约ABI接口。
- 等待Block.one官方合约审查系统, 我们将立即提交实名制和代码双重审查。

### 联系方式 
- EOSonic@outlook.com
- Wechat@eosiobank

---
![EOSBank](https://github.com/eosonic/EOSBank/blob/master/image/eosbank.png)
