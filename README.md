![EOSBank](https://github.com/eosonic/EOSBank/blob/master/image/banner2.png)
 
   
## EOS Bank 柚资银行  
![Bank Stutas](https://github.com/eosonic/EOSBank/blob/master/image/open.png) 

柚资银行是EOS网络基础设施，它允许DApp开发者或者游戏玩家支付极少的利息在短期内获得大量CPU计算资源，这些利息将会支付给银行的所有储户，这些租赁交易通过智能合约执行。

### 需求分析
随着EOS生态的发展，EOS计算资源将会变得越来越昂贵，很多开发者的计算需求并不持续，往往存在短时间内爆发出大量计算需求。在世界杯/圣诞节期间，短期的促销需要大量计算资源，为了应对这样的需求，柚资银行作为EOS网络的基础设施，提供计算资源的短期快速租赁。

### 租赁操作说明

使用任意EOS钱包，向银行合约账户：**eosiocpubank** 转入少量EOS，资源倍数请参阅下一章节**当前租赁价格**，合约将会自动按照租赁利率计算后，0.5秒内为你抵押CPU资源，你甚至可以在备注里写其他账号名字，我们将为你写的的这个账号提供租赁，不写账号名则默认租赁给你自己。  
  
转账数量决定了获得CPU抵押的数量，例如：转入0.05个币并且备注为空时，将立即为你抵押9.1249CPU资源，备注写上“4d/7d”,将会自动为你抵押4天/7天,下图是使用钱包操作转账，这将立即获得5.2146EOS的CPU资源维持4天。   
  
![rent](https://github.com/eosonic/EOSBank/blob/master/image/rent4.png)
  
命令行操作：
  
> cleos transfer 你的账号 **eosiocpubank** "0.1 EOS" ""       
> cleos transfer 你的账号 **eosiocpubank** "0.1 EOS" "4d"      
> cleos transfer 你的账号 **eosiocpubank** "0.1 EOS" "7d" 

如果你需要给其他账号租赁资源，请在备注里写账号名称即可:
  
> cleos transfer 你的账号 **eosiocpubank** "0.1 EOS" "youraccount"  
> cleos transfer 你的账号 **eosiocpubank** "0.1 EOS" "4d youraccount"      
> cleos transfer 你的账号 **eosiocpubank** "0.1 EOS" "7d youraccount" 
  
提醒：由于EOS网络存在大量发送0.0001个EOS的广告行为，柚资银行目前支持的最低租赁发送0.01 EOS，我们推荐你按照自己的需求，使用下方的利息公式，发送一定量的EOS币，获得CPU计算资源

### 当前租赁价格182.5倍

发送 | 获得资源 | 租赁持续 | 
------------ | -------------|-------------
0.1 EOS | 18.2499 EOS CPU | 1天 | 
0.1 EOS | 10.4285 EOS CPU | 4天 | 
0.1 EOS | 7.2989 EOS CPU | 1周 | 

![EOSBank](https://github.com/eosonic/EOSBank/blob/master/image/banner4.png)

### 向银行存款
银行合约eosiocpubank支持储蓄你的EOS币到银行，每当有任意租单成交，我们会立即分红给每一个储蓄的账号。

为了保持储蓄的收益率稳定，柚资银行词用动态的存款模型，限制了储蓄池的大小，当合约eosiocpubank的unstaked小于50000.0000EOS，合约将会自动开放存款功能，这被视为银行缺乏资金需要补充，你可以转账大于10个EOS币到合约，在备注MEMO里写入“deposit”（没有双引号）。这将会被记录为存款。

相反的，每当合约 eosiocpubank的unstaked大于50000.0000个EOS，合约自动关闭存款功能，你将会无法把EOS币发送给银行，任何在备注内包含deposit的转账将会被系统自动拒收。我们以此来保持银行资金池的大小在合适的状态，以防止由于存款过多导致所有储户的收益率明显下降。

### 存款操作说明

使用任意EOS钱包，向合约账户：**eosiocpubank** 转入大于10个EOS，必须小于50000个EOS，备注MEMO写上deposit,支持任意笔存款转账，它们都会记录在你的账号下。不支持替其他账户存款，备注MEMO仅支持写入deposit，参见上一章节，如果柚资银行资金池满了（超过50000EOS），则无法接受储蓄，智能合约将会自动拒绝客户存款。请等待银行储蓄降低后操作储蓄。 
  
  ![EOSBank](https://github.com/eosonic/EOSBank/blob/master/image/deposit2.jpg)


### 分红结算模型

一笔租赁交易达成收到的利息为L，我们将会对当前储蓄资产进行快照，设定平台储蓄总余额为N，某客户储蓄为A，则客户根据储蓄数量A，此次租赁立即获得A/N**L**利息分红。  

注意！你存款利息的总收入受到动态的出租率影响，假设平台有1万个EOS币，你储蓄了5000个，当我们出租出去7500个时候，你获得的实际利息是设定利率的75%，也即，设定利率**乘以**出租率=实际利率。

实际计算展示：
我们收到一笔10个币的租赁请求，计算平台目前总储蓄是5000个币，储户在平台存储了100个币，则A/NL=100储蓄/5000总额X10利息=0.2EOS,即：用户由于这一笔租赁立刻收到0.2个币的分红，每次出租我们都根据当前快照结算一次分红。这种快照型分红跟租赁利率没有任何关系。

### 查询你的储蓄和分红

使用命令行模式：  

>cleos get table **eosiocpubank** 你的账号 deposit    
  
如果你没有EOS软件，也没有Cleos命令的发送工具，请执行以下动作查询你的存款；
1. 打开API查询网站 http://apirequest.io/
2. 选择post，在URL一栏输入https://api.eosnewyork.io/v1/chain/get_table_rows
3. 在Request Body一栏输入：{"json":"true","code":"eosiocpubank","scope":"这里输入你的EOS存款账号","table":"deposit"}
4. 点击Send One
5. 等待几秒后，在Body里，amount会显示储蓄余额+分红数值，图例为显示了账号cpubankfound的余额，请更改为你的账号。  
  
![EOSBank](https://github.com/eosonic/EOSBank/blob/master/image/deposit.jpg)  
  
### 提现和查询提现进度：
 
如你需要提现你的存款，柚资银行仅支持一次性提取所有余额+分红，方法是，请使用任意EOS钱包发送0.0001个EOS币到柚资银行eosiocpubank即可进入提现队列，注意，请勿写入任何备注MEMO，根据EOS网络设定，解除抵押需要等待3天，合约将在3天后自动发送你所有的EOS币到你的账号，使用命令行来提现：
  
> cleos transfer 你的账号 **eosiocpubank** "0.0001 EOS" ""    
  
提现命令发送后，使用命令行模式查询提现进度：  
  
>cleos get table **eosiocpubank** 你的账号 refunds  

如果你没有EOS软件，也没有Cleos命令的发送工具，请执行以下动作查询你的提现进度；
  
1. 打开API查询网站 http://apirequest.io/
2. 选择post，在URL一栏输入https://api.eosnewyork.io/v1/chain/get_table_rows
3. 在Request Body一栏输入：{"json":"true","code":"eosiocpubank","scope":"这里输入你的EOS存款账号","table":"refunds"}
4. 点击Send One
5. 等待几秒后，在Body里，request_time会显示你的提币操作时间，这个数字是Unix时间戳，请百度“Unix时间戳转换工具”转换的时间加三天为体现到账时间。

### 解除你的提现申请

如果你在提现命令发送后的72小时内想要取消你的提现，请使用任意EOS钱包发送0.0002个EOS币到柚资银行eosiocpubank即可取消你的提现请求，注意，请勿写入任何备注MEMO，你也可以使用命令行来取消提现命令，这是立即生效的，你将会立即参与下一次租单的分红并获得收益。

> cleos transfer 你的账号 **eosiocpubank** "0.0002 EOS" ""      

![EOSBank](https://github.com/eosonic/EOSBank/blob/master/image/banner5.png)

### 柚资银行优势

- #### 强大的资本充足率
柚资银行拥有充足的固定资本，同时吸收EOS小额储蓄，我们支持客户以大于10个EOS的数量储蓄自己的EOS币，我们会把利息收入返还给储蓄者。多样的资金来源，强大的资本保证，充分保障快速变化的租赁需求。

- #### 全自动执行租赁订单
采用智能合约响应租赁请求，任何人可以发送EOS币到合约账户**eosiocpubank**，系统按照当前价格立即租赁CPU给发送的客户。

- #### 先进的租赁周期算法
同时满足客户的临时性资源需求和长周期资源租赁需求，智能合约按照需求的周期自动报价。

- #### 全自动动态高流动性资产委托池
全自动的资金池控制模型，全自动的快速委托秒级抵押和反抵押，如此快速的业务模型使得合约的运行非常繁忙，一系列动态的金融模型，使得柚资银行储蓄资本会拥有极高的周转率，同时收益率达到最高状态的同时租赁价格反而很低。这将给储蓄者带来高额回报，同样也降低了使用者的价格。

- #### 实名制的合约开发者
为了保持资金安全，对于大型储户，我们使用了账号权限隔离机制，合约eosiocpubank不拥有transfer EOS币的权限，这从根本上杜绝了大资金的被盗，同时，我们公开了团队开发者的身份信息以供查询。

### 高端金融服务
  
如果您是EOS游戏深度玩家，或者你是量化操作机器人，时间不允许你频繁查询自己的资源状态，或者您的业务需要全天候可用，柚资银行提供高端定制服务，我们将会监控你的EOS账号，每当资源使用率超过95%，我们将会为你自动抵押CPU，使你处于“战神状态”，同样，当你业务下降之后，我们检测到你的资源使用率小于85%我们将会为你解除部分抵押，这种动态的抵押和反抵押操作使你提高租赁效率，大量节省了你的查询时间和抵押成本。请翻阅本文的最后一章，联系我们，将为你设计最佳的自动租赁模型。

### 银行打烊时间 
就像传统银行需要结算一样，柚资银行在每天的UTC16:00-UTC16:01将会打烊一分钟时间，在这个打烊的时间段内，合约会自动拒绝所有租赁需求。柚资银行的智能合约开始自我审查这一天的所有动作，生成报表以供人工审核分析银行的业务状态。
  
### 常见问题  
  
- 去哪里下载APP使用？
柚资银行没有也不需要制作APP，它是一个运行在EOS网络上的智能合约，你可以使用任意EOS钱包开始操作。
  
- 为什么我需要租赁CPU而不是购买？   
大部分人并不是每天都需要CPU资源，在短时间内爆发出的CPU需求将会占用大量资金，我们提供1天/4天/1周的短期租赁合约，假设世界杯来临而你需要发送广告，那么发送1个EOS币将会给你提供304.1个币的CPU资源并维持一整天，你无须抵押182.4992个EOS并且等待长达3天的赎回期。你甚至可以仅仅发送0.01个币而获得1.8249个币的计算资源。
  
- 我可以给其他账号租赁CPU资源吗？  
当然可以，如果你在MEMO备注里写入账号名称，我们将会把资源抵押在你写的这个账号上，目前仅仅支持12位长度的账号。

- 我使用完毕了，我需要归还什么吗？  
你不需要任何操作，智能合约会在租赁到期后自动收回资源。
    
- 如何更划算的租赁CPU？  
租赁周期越长租赁花费越低，很显然，租赁一周要比租赁7个1天的花费少得多。这在酒店业都是如此，长期订房显然比只住一天要便宜的多。当然，如果你不需要租赁那么久，依然是需要用的时候临时租赁更加划算。如果你业务繁忙对资源有特殊要求，请参阅本文档《高端金融服务》一章。

- 为什么我无法存款？转账失败了。
柚资银行采用业内领先的动态储蓄模型，当柚资银行的资本超过50000.0000EOS之后，柚资银行自动拒绝客户储蓄，任何人无法发送EOS币到合约账户，反之当银行资金池小于50000.0000的时候，储蓄合约自动开启。你将可以发送EOS存储，但请注意，你发送的数量无法使柚资银行的资金池大于50000.0000EOS.
  	
### 开源方式 
- 合约hash：40bc10984c596a9caeda33218af2bc10edc3c05499d73517d91dbe212f2d329a
- 合约最后更新时间:2018-08-15 00:10:02
- 柚资银行的合约操作流程和功能已尽详，已属于半开源模式，就像所有的传统银行不会放开参观银行内部的风控模型一样，我们仅会将代码提交Block.one审阅。
- 如需详细资料，[请点击查看](https://eospark.com/MainNet/contract/eosiocpubank)合约ABI接口。 
- 等待Block.one官方合约审查系统, 我们将立即提交实名制和代码安全双重审查。 

### 联系方式  
- EOSonic@outlook.com 
- Wechat@eosiobank/auoka-

#### 柚资银行是虎符钱包旗下DApp 详情参阅https://hoo.com/

---
![EOSBank](https://github.com/eosonic/EOSBank/blob/master/image/EOSBank.png)
