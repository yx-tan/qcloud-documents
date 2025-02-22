## 简介

腾讯云命令行工具（TCCLI）是管理腾讯云资源的统一工具。通过 TCCLI，您可以快速轻松的调用腾讯云 API 来管理您的腾讯云资源。本文档指导您安装、配置 TCCLI，并通过 TCCLI 调用 TBaaS 对外提供的云 API 接口管理您的资源。

>! TCCLI 基于 Python 对云 API 的调用进行了封装，所以此方式同云 API 一样，可以对 Fabric、BCOS 和 TrustSQL 进行操作，一般用于单次调用，不适用于集成到业务应用系统中。


## 安装 TCCLI

1.	安装 Python 环境和 Pip 工具。
>! Python 版本必须为 2.7 及以上版本，更多内容请参考 [Python 主页](https://www.python.org/) 和 [Pip 主页](https://pypi.org/project/pip/)。
2.	执行以下命令，安装 TCCLI。
>? TCCLI 依赖于 TencentCloudApi Python SDK。如果 TencentCloudApi Python SDK 的版本号小于安装 TCCLI 版本，安装 TCCLI 时，将自动升级 TencentCloudApi Python SDK。 

 ```
pip install tccli
```
4.	完成 TCCLI 安装后，执行以下命令，检测是否安装成功。
```
tccli version 
```
返回信息类似如下所示，则表示安装成功。
![返回信息](https://main.qcloudimg.com/raw/94ebd9b59a4bdfe554da7e69e051ab4c.png)
5. （可选）如果您使用的是 Linux 操作系统，执行以下命令，自动补全功能。
```
complete -C 'tccli_completer' tccli
```
>? Windows 操作系统暂不支持此操作。 

## 配置 TCCLI

使用腾讯云命令行工具，需进行以下初始化配置，使其完成使用 云 API 的必要前提条件。请根据实际需求，选择配置模式进行配置：
- **交互模式**：执行 `tccli co6nfigure` 命令，进入交互模式快速配置。
```
$  tccli configure
TencentCloud API secretId [*afcQ]:AKIDwLw******fPRle2g9nR2OTI787aBCDP
TencentCloud API secretKey [*ArFd]:OxXj7khcV******SSYNABcdCc1LiArFd
region: ap-guangzhou
output[json]:
```
 - secretId：云 API 密钥 SecretId。
 - secretIKey：云 API 密钥 SecretKey。
 - region：云产品地域，请切换至对应产品页面获取可用的 region。
 - output：可选参数，请求回包输出格式，支持 [json table text] 三种格式，默认为 json。

 更多信息请执行 `tccli configure help` 查看。
 
- **命令行模式**：通过命令行模式您可以在自动化脚本中配置您的信息。
```
# set 子命令可以设置某一配置，也可同时配置多个。
tccli configure set secretId AKIDwLw******fPRle2g9nR2OTI787aBCDP
tccli configure set region ap-guangzhou  output json
# get 子命令用于获取配置信息。
tccli configure get secretKey
secretKey = OxXj7khcV******SSYNABcdCc1LiArFd
# list 子命令打印所有配置信息。
tccli configure list
credential:
secretId =  AKIDwLw******fPRle2g9nR2OTI787aBCDP
secretKey =  OxXj7khcV******SSYNABcdCc1LiArFd
configure:
region =  ap-guangzhou
output =  json
```
更多信息请执行 `tccli configure [list get set] help` 查看。

TCCLI 还支持多账户，方便您同时使用多种配置。以账户名 test 为例：
- 通过交互模式，指定账户名 test。
```
$  tccli configure --profile test
TencentCloud API secretId [*BCDP]:AKIDwLw******fPRle2g9nR2OTI787aBCDP
TencentCloud API secretKey [*ArFd]:OxXj7khcV******SSYNABcdCc1LiArFd
region: ap-guangzhou
output[json]:
```
- 通过命令行模式，指定账户名 test。
```
# set/get/list 子命令指定账户名 test。
tccli configure set region ap-guangzhou  output json  --profile test
tccli configure get secretKey      --profile test
tccli configure list      --profile test
在调用接口时指定账户（以 cvm DescribeZones 接口为例）。
tccli cvm DescribeZones --profile test
```

## 使用 TCCLI

- 通过 `tccli tbaas Invoke` 命令，新增交易（支持同步模式和异步模式）。
- 通过 `tccli tbaas Query` 命令，查询交易。
- 通过 `tccli tbaas GetInvokeTx` 命令，查询 Invoke 异步调用结果。

## 示例

#### 准备工作

通过 TBaaS 控制台创建并初始化合约。

#### 操作步骤

>? 以转账交易合约为例，初始 a、b 两个账号值为100。

- **新增交易（同步模式）**
请求：
```
tccli tbaas Invoke --Module 'transaction' --Operation 'invoke' --ClusterId '1251568418ndemo' --ChaincodeName 'tylercc' --ChannelName 'cfirst' --FuncName 'invoke' --Peers '[{"PeerName":"peer1.aliceorg.ndemo","OrgName":"AliceOrg"}]' --Args '["b", "a", "10"]'
```
返回：
```
{
    "Events": "AliceOrgpeer1.aliceorg.ndemo:VALID", 
    "RequestId": "f305f44d-0697-48e2-b85d-e416c4******", 
    "Txid": "a73158cc7b628fbcefc329d3fd5de9db9dfaa312528ed09cf4ab121ff6******"
}
```
参数说明：
<table>
	<tr>
		<th>操作</th>
		<th>参数</th>
		<th>说明</th>
	</tr>
	<tr>
		<td rowspan="8">请求</td>
		<td>Module</td>
		<td>模块名，固定字段：transaction</td>
	</tr>
	<tr>
		<td>Operation</td>
		<td>操作名，固定字段：invoke</td>
	</tr>
	<tr>
		<td>ClusterId</td>
		<td>区块链网络ID，可在区块链网络详情或列表中获取</td>
	</tr>
	<tr>
		<td>ChaincodeName</td>
		<td>业务所属智能合约名称，可在智能合约详情或列表中获取</td>
	</tr>
	<tr>
		<td>ChannelName</td>
		<td>业务所属通道名称，可在通道详情或列表中获取</td>
	</tr>
	<tr>
		<td>FuncName</td>
		<td>该笔交易需要调用的智能合约中的函数名称</td>
	</tr>
	<tr>
		<td>Peers</td>
		<td>对该笔交易进行背书的节点列表（包括节点名称和节点所属组织名称，详见数据结构一节），可以在通道详情中获取该通道上的节点名称极其所属组织名称</td>
	</tr>
	<tr>
		<td>Args</td>
		<td>被调用的函数参数列表，示例中'["b", "a", "10"]'为b向a转账10</td>
	</tr>
	<tr>
		<td rowspan="3">返回</td>
		<td>Events</td>
		<td>事件处理结果</td>
	</tr>
	<tr>
		<td>RequestId</td>
		<td>请求 ID</td>
	</tr>
	<tr>
		<td>Txid</td>
		<td>交易 ID</td>
	</tr>
</table>

- **新增交易（异步模式）**
请求：
```
tccli tbaas Invoke --Module 'transaction' --Operation 'invoke' --ClusterId '1251568418ndemo' --ChaincodeName 'tylercc' --ChannelName 'cfirst' --FuncName 'invoke' --Peers '[{"PeerName":"peer1.aliceorg.ndemo","OrgName":"AliceOrg"}]' --Args '["b", "a", "10"]' --AsyncFlag '1'
```
返回：
```
{
    "Events": "", 
    "RequestId": "be95c1e4-ffb9-49e4-a1bb-60770c******", 
    "Txid": "1c80cea264a447c9d4d0ba50adbe6e0ff1ac1db313f8d6c83370e410db******"
}
```
参数说明：
<table>
	<tr>
		<th>操作</th>
		<th>参数</th>
		<th>说明</th>
	</tr>
	<tr>
		<td rowspan="9">请求</td>
		<td>Module</td>
		<td>模块名，固定字段：transaction</td>
	</tr>
	<tr>
		<td>Operation</td>
		<td>操作名，固定字段：invoke</td>
	</tr>
	<tr>
		<td>ClusterId</td>
		<td>区块链网络ID，可在区块链网络详情或列表中获取</td>
	</tr>
	<tr>
		<td>ChaincodeName</td>
		<td>业务所属智能合约名称，可在智能合约详情或列表中获取</td>
	</tr>
	<tr>
		<td>ChannelName</td>
		<td>业务所属通道名称，可在通道详情或列表中获取</td>
	</tr>
	<tr>
		<td>FuncName</td>
		<td>该笔交易需要调用的智能合约中的函数名称</td>
	</tr>
	<tr>
		<td>Peers</td>
		<td>对该笔交易进行背书的节点列表（包括节点名称和节点所属组织名称，详见数据结构一节），可以在通道详情中获取该通道上的节点名称极其所属组织名称</td>
	</tr>
	<tr>
		<td>Args</td>
		<td>被调用的函数参数列表，示例中'["b", "a", "10"]'为b向a转账10</td>
	</tr>
	<tr>
		<td>AsyncFlag</td>
		<td>同步调用标识，可选参数，值为0或者不传表示使用同步方法调用，调用后会等待交易执行后再返回执行结果；值为1时表示使用异步方式调用，执行后会立即返回交易对应的Txid，后续需要通过GetInvokeTx查询该交易的执行结果。（对于逻辑较为简单的交易，可以使用同步模式；对于逻辑较为复杂的交易，建议使用异步模式，否则容易导致API因等待时间过长，返回等待超时）</td>
	</tr>
	<tr>
		<td rowspan="3">返回</td>
		<td>Events</td>
		<td>事件处理结果，异步调用返回空字符串</td>
	</tr>
	<tr>
		<td>RequestId</td>
		<td>请求 ID</td>
	</tr>
	<tr>
		<td>Txid</td>
		<td>交易 ID</td>
	</tr>
</table>

- **查询交易**
请求：
```
tccli tbaas Query --Module 'transaction' --Operation 'query' --ClusterId '1251568418ndemo' --ChaincodeName 'tylercc' --ChannelName 'cfirst' --FuncName 'query' --Peers '[{"PeerName":"peer1.aliceorg.ndemo","OrgName":"AliceOrg"}]' --Args '["a"]'
```
返回：
```
{
    "Data": [
        "110"
    ], 
    "RequestId": "237b61b6-c75f-4927-9729-1e117a******"
}
```
参数说明：
<table>
	<tr>
		<th>操作</th>
		<th>参数</th>
		<th>说明</th>
	</tr>
	<tr>
		<td rowspan="8">请求</td>
		<td>Module</td>
		<td>模块名，固定字段：transaction</td>
	</tr>
	<tr>
		<td>Operation</td>
		<td>操作名，固定字段：query</td>
	</tr>
	<tr>
		<td>ClusterId</td>
		<td>区块链网络ID，可在区块链网络详情或列表中获取</td>
	</tr>
	<tr>
		<td>ChaincodeName</td>
		<td>业务所属智能合约名称，可在智能合约详情或列表中获取</td>
	</tr>
	<tr>
		<td>ChannelName</td>
		<td>业务所属通道名称，可在通道详情或列表中获取</td>
	</tr>
	<tr>
		<td>FuncName</td>
		<td>该笔交易需要调用的智能合约中的函数名称</td>
	</tr>
	<tr>
		<td>Peers</td>
		<td>对该笔交易进行背书的节点列表（包括节点名称和节点所属组织名称，详见数据结构一节），可以在通道详情中获取该通道上的节点名称极其所属组织名称</td>
	</tr>
	<tr>
		<td>Args</td>
		<td>被调用的函数参数列表，示例中'["a"]'为查询a账号的值</td>
	</tr>
	<tr>
		<td rowspan="2">返回</td>
		<td>Data</td>
		<td>查询结果</td>
	</tr>
	<tr>
		<td>RequestId</td>
		<td>请求 ID</td>
	</tr>
</table>

- **查询 Invoke 异步调用结果**
请求：
```
tccli tbaas GetInvokeTx --Module 'transaction' --Operation 'query_txid' --ClusterId '1251568418ndemo' --ChannelName 'cfirst' --PeerName 'peer1.aliceorg.ndemo' --PeerGroup 'AliceOrg' --TxId '1c80cea264a447c9d4d0ba50adbe6e0ff1ac1db313f8d6c83370e410db******'
```
返回：
```
{
    "TxValidationCode": 0, 
    "RequestId": "6a1d26ba-b322-4aea-8db7-effc82******", 
    "TxValidationMsg": "VALID"
}
```
参数说明：
<table>
	<tr>
		<th>操作</th>
		<th>参数</th>
		<th>说明</th>
	</tr>
	<tr>
		<td rowspan="7">请求</td>
		<td>Module</td>
		<td>模块名，固定字段：transaction</td>
	</tr>
	<tr>
		<td>Operation</td>
		<td>操作名，固定字段：query_txid</td>
	</tr>
	<tr>
		<td>ClusterId</td>
		<td>区块链网络ID，可在区块链网络详情或列表中获取</td>
	</tr>
	<tr>
		<td>ChannelName</td>
		<td>业务所属通道名称，可在通道详情或列表中获取</td>
	</tr>
	<tr>
		<td>PeerName</td>
		<td>执行该查询交易的节点名称，可以在通道详情中获取该通道上的节点名称极其所属组织名称</td>
	</tr>
	<tr>
		<td>PeerGroup</td>
		<td>执行该查询交易的节点所属组织名称，可以在通道详情中获取该通道上的节点名称极其所属组织名称</td>
	</tr>
	<tr>
		<td>TxId</td>
		<td>事务 ID</td>
	</tr>
	<tr>
		<td rowspan="3">返回</td>
		<td>TxValidationCode</td>
		<td>交易状态码</td>
	</tr>
	<tr>
		<td>RequestId</td>
		<td>请求 ID</td>
	</tr>
	<tr>
		<td>TxValidationMsg</td>
		<td>交易状态信息</td>
	</tr>
</table>
