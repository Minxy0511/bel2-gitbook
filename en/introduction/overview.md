# Overview

BeL2是一个创新的区块链技术开源项目,旨在通过零知识证明(ZKP)技术提升比特币网络的可扩展性、互操作性和安全性。BeL2的核心是构建与比特币共识代码等价的ZKP电路,为比特币的UTXO(未花费交易输出)状态转换生成紧凑且可验证的证明。

BeL2的技术原理是将比特币共识规则抽象为一个电路,包括区块和交易的验证逻辑、UTXO状态的更新规则等。然后,利用ZKP技术(如zk-STARK)对这个电路进行"零知识压缩",生成一个证明,证明某个新的UTXO状态是按照比特币共识规则从之前的状态推导出来的。通过严格证明这个ZKP电路与原始的比特币共识代码在逻辑上的等价性,BeL2确保了证明的正确性和可靠性。同时,得益于ZKP的零知识性质,证明过程不会泄露关于UTXO状态的任何额外信息。

基于这一技术创新,BeL2提供三项基础服务:

### ZKP全节点

在同步全节点数据时,节点只需要同步最新的UTXO集合状态及其ZKP证明,而无需下载和验证所有历史区块数据。这大大降低了全节点的存储和带宽需求,使得资源受限的设备也能够运行一个"裁剪版"的比特币全节点。

除了减少节点的存储空间之外,还可以提供地址余额的零知识证明,可以将地址所持有的UTXO集合和其证明一起提供给需求方,需求方无需使用全节点对其进行验证,只需要验证证明即可信任这些信息。这样可以实现一个无需信任的BTC RPC节点服务。

### 交易证明服务网络

传统BTC交易的验证需要使用全节点或者轻节点,由于需要同步数据而导致其体验不佳,并且依赖网络环境而限制了使用场景。借助BeL2的BTC交易证明,可以生成一个短小的交易证明,方便使用者在各种环境下验证。这个证明即包括其交易信息,也包括它所在的区块的证明,甚至还可以包括已有几次确认的证明。

这个证明还可以被solidity智能合约验证,将BTC的时间戳信息传递到EVM链上,为两者之间进行互操作提供可能。所有需要被验证的交易都通过一个订单合约发布,任何人都可以做零知识证明矿工,通过帮助用户生成证明获得奖励,无需许可即可参与。

### 仲裁网络

基于交易证明,我们可以监督BTC上发生了哪些交易,可以结合智能合约实现挑战机制。如果任何被监督的地址执行了不在预期内的交易签名,其他人可以在合约里发起挑战。如果再增加质押机制,就可以实现一个完整的挑战证明和惩罚机制。通过这种间接的方式,可以实现由代理人协助交易双方完成交易过程,如果代理人作恶,则通过对其进行挑战和惩罚,约束其行为。

BeL2实现了一个仲裁网络,第三方协议通过调用仲裁合约发布签名任务，由指定的仲裁人辅助签名BTC交易，实现帮助第三方协议推进其业务流程。通过质押和挑战机制,仲裁人必须为签名任务进行签名，并且，其只能为签名任务签名，如果仲裁人签署了任何不在签名任务里的交易，其他人可以通过挑战证明机制对其进行惩罚。



总的来说,BeL2代表了一种将ZKP技术引入比特币生态的重要尝试。它不仅可以显著优化比特币全节点的性能,还为构建更加安全、高效、功能丰富的比特币二层网络、侧链和跨链协议开辟了新的可能性。通过将比特币的共识安全性与ZKP的可验证性和可编程性相结合,BeL2有望成为未来比特币扩容和互操作的一个关键技术基础设施。



BeL2: Bitcoin Consensus Zero-Knowledge Proof Solution

BeL2 is an innovative open-source blockchain technology project that aims to enhance the scalability, interoperability, and security of the Bitcoin network through zero-knowledge proof (ZKP) technology. The core of BeL2 is to construct a ZKP circuit equivalent to the Bitcoin consensus code, generating a compact and verifiable proof for the state transition of Bitcoin's UTXO (unspent transaction output).

The technical principle of BeL2 is to abstract the Bitcoin consensus rules into a circuit, including the validation logic for blocks and transactions, the update rules for UTXO states, etc. Then, using ZKP technology (such as zk-STARK), this circuit is "zero-knowledge compressed" to generate a proof, proving that a new UTXO state is derived from the previous state according to the Bitcoin consensus rules. By strictly proving the logical equivalence between this ZKP circuit and the original Bitcoin consensus code, BeL2 ensures the correctness and reliability of the proof. At the same time, thanks to the zero-knowledge property of ZKP, the proving process does not reveal any additional information about the UTXO state.

Based on this technological innovation, BeL2 provides three fundamental services:



### ZK Proof Bitcoin Full Node

When synchronizing full node data, nodes only need to synchronize the latest UTXO set state and its ZKP proof, without downloading and verifying all historical block data. This greatly reduces the storage and bandwidth requirements of full nodes, enabling even resource-constrained devices to run a "pruned" version of the Bitcoin full node. In addition to reducing the storage space of nodes, BeL2 can also provide zero-knowledge proofs of address balances. The UTXO set held by an address and its proof can be provided together to the requester, who does not need to use a full node for verification; they only need to verify the proof to trust this information. This enables a trustless BTC RPC node service.



### Transaction Proof Service Network

The verification of traditional BTC transactions requires the use of full nodes or light nodes, which leads to a poor user experience due to the need for data synchronization and limits usage scenarios due to network environment dependencies. With the help of BeL2's BTC transaction proofs, a short transaction proof can be generated, making it convenient for users to verify in various environments. This proof includes not only the transaction information but also the proof of the block it belongs to and even the proof of the number of confirmations it has received. This proof can also be verified by Solidity smart contracts, passing the BTC timestamp information to the EVM chain, enabling interoperability between the two. All transactions that need to be verified are published through an order contract, and anyone can become a zero-knowledge proof miner, earning rewards by helping users generate proofs without permission.



### Arbitor Network

Arbiter Network Based on transaction proofs, we can monitor what transactions have occurred on BTC and implement a challenge mechanism combined with smart contracts. If any monitored address executes a transaction signature that is not within expectations, others can initiate a challenge in the contract. If a staking mechanism is added, a complete challenge-proof and punishment mechanism can be realized. Through this indirect approach, transactions can be completed with the assistance of proxies, and if a proxy misbehaves, it can be challenged and punished to constrain its behavior.

BeL2 implements an arbiter network where third-party protocols publish signing tasks by calling the arbiter contract. Designated arbiters assist in signing BTC transactions, helping third-party protocols advance their business processes. Through staking and challenge mechanisms, arbiters must sign the signing tasks, and they can only sign transactions within the signing tasks. If an arbiter signs any transaction that is not included in the signing tasks, others can punish them through the challenge-proof mechanism.



Overall, BeL2 represents an important attempt to introduce ZKP technology into the Bitcoin ecosystem. It not only significantly optimizes the performance of Bitcoin full nodes but also opens up new possibilities for building more secure, efficient, and feature-rich Bitcoin layer-2 networks, sidechains, and cross-chain protocols. By combining the consensus security of Bitcoin with the verifiability and programmability of ZKP, BeL2 is expected to become a key technical infrastructure for future Bitcoin scaling and interoperability.
