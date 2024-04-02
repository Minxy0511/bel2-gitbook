# BeL2 Structure

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>



1. TX Proof (交易证明)

TX Proof模块用于为单个比特币交易生成证明。它包括两个部分:

* **ZK Service Contract**: 用于生成零知识证明。
* **ZK Verification Contract**: 用于验证证明。

"ZK Service Contract"是可选的。如果用户可以独立运行Cairo电路程序并生成证明,他们可以自行生成证明,并将其提交到验证合约进行验证。

然而,生成证明仍然需要大量的内存和计算能力。因此,BeL2设计了"ZK Service Contract"作为一个算力市场。用户将需要生成证明的交易连同服务费一起提交到该合约。如果算力提供商有兴趣,他们将接受订单,生成证明,并将其提交到"ZK Verification Contract"进行验证。一旦验证成功,他们将获得用户提供的服务费。

"ZK Verification Contract"不仅可以验证零知识证明,还可以将证明输出的信息以Key-Value格式提供给dApp进行查询。

2. Relayer Network (中继器网络)

Relayer Network模块为智能合约网络中的事件生成必要的签名,如BTC价格事件、代币铸造或销毁事件等。它由三个部分组成:

* **Relayer Contract**: 用于管理中继器。
* **Oracle Contract**: 用于管理需要签名的事件。
* **Taproot TX SDK**: 用于生成和管理BTC交易。

任何人都可以通过在Relayer Contract中质押资产来成为中继器。如果中继器行为不当,他们将通过该合约受到惩罚。

dApp可以通过向Oracle Contract注册需要签名的事件来触发中继器对事件进行签名。

开发者可以使用Taproot TX SDK生成托管BTC的交易脚本,指定所需的签名类型、中继器以及签名阈值(m of n)。

3. ZKP BTC Full Node (零知识证明比特币全节点)

这是BTC全节点模块,它将提供可以从创始块开始验证的证明电路,并生成"有效的UTXO集合"以及相应的证明。证明的有效性可以使用相应的"ZK Verification Contract"进行验证。

一旦生成最新高度的"有效UTXO集合",就可以独立运行一个轻量级的BTC全节点(已剪枝历史数据)。



##

1. TX Proof

The TX Proof module is used to generate proofs for individual Bitcoin transactions. It consists of two parts:

* **ZK Service Contract**: Used to generate zero-knowledge proofs.
* **ZK Verification Contract**: Used to verify proofs.

The "ZK Service Contract" is optional. If users can independently run the Cairo circuit program and generate proofs, they can generate the proofs themselves and submit them to the verification contract for validation.

However, generating proofs still requires a significant amount of memory and computational power. Therefore, BeL2 has designed the "ZK Service Contract" as a computational power market. Users submit transactions that require proof generation along with a service fee to this contract. If computational power providers are interested, they will accept the order, generate the proof, and submit it to the "ZK Verification Contract" for verification. Upon successful verification, they will receive the service fee provided by the user.

The "ZK Verification Contract" not only verifies zero-knowledge proofs but also provides the information output from the proofs in a Key-Value format for dApp queries.

2. Relayer Network

The Relayer Network module generates necessary signatures for events in the smart contract network, such as BTC price events, token minting or burning events, etc. It consists of three parts:

* **Relayer Contract**: Used to manage relayers.
* **Oracle Contract**: Used to manage events that require signatures.
* **Taproot TX SDK**: Used to generate and manage BTC transactions.

Anyone can become a relayer by staking assets in the Relayer Contract. If a relayer misbehaves, they will be penalized through this contract.

dApps can trigger relayers to sign events by registering the events that require signatures with the Oracle Contract.

Developers can use the Taproot TX SDK to generate transaction scripts for custodial BTC, specifying the required signature types, relayers, and signature thresholds (m of n).

3. ZKP BTC Full Node

This is the BTC full node module, which will provide proof circuits that can be validated from the genesis block and generate a "valid UTXO set" along with the corresponding proofs. The validity of the proofs can be verified using the corresponding "ZK Verification Contract".

Once the "valid UTXO set" at the latest height is generated, a lightweight BTC full node (with pruned historical data) can be run independently.
