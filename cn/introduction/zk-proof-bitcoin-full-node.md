# ZK Proof Bitcoin Full Node

比特币的全节点数据同步是一个耗时且资源密集的过程,需要下载和验证从创始块开始的所有区块数据。每个区块由区块头和区块内容组成,区块内容包含了多笔交易,每笔交易对应着我们在比特币网络上发送的一次转账。

当节点收到新区块的广播时,共识代码需要对其进行反序列化,验证其工作量证明、前驱区块、交易默克尔树以及每笔交易的有效性。为了优化这一过程,我们可以引入"有效UTXO集合"的概念,它表示所有尚未被花费的UTXO的集合。每当一笔交易消耗了某些UTXO,就将它们从集合中移除;每当一笔交易产生了新的UTXO,就将它们加入到集合中。

进一步地,我们可以设计一个类似于比特币共识代码的零知识证明电路。该电路以当前的"有效UTXO集合"和最新区块作为输入,对区块进行解析和验证,并输出更新后的"有效UTXO集合"以及相应的证明。通过反复迭代调用这个电路,我们可以从创始块开始,逐步更新"有效UTXO集合",直到最新的区块高度。这样,我们就得到了当前比特币全网的"有效UTXO集合"及其证明。

基于这一机制,新的比特币全节点无需从头同步所有区块数据,只需下载最新的"有效UTXO集合"及其证明。在验证证明的有效性后,节点可以保留这个集合,并从下一个区块开始同步,每次同步后再更新集合。这种方式可以显著减少数据同步的时间,对于低带宽环境下的全节点而言是一个重大改进。

为了避免数据量过大导致证明生成困难,上述迭代过程可能不以区块为单位,而是以交易为单位进行。同时,"有效UTXO集合"可以采用默克尔树或Verkle树等数据结构来实现,尽量只调整需要修改的部分,而不必重构整个树。MIT提出的UTreeXO技术方案也是一种可能的实现路径。

在这个比特币证明电路的基础上,我们还可以进一步实现Ordinals协议和BRC20协议的证明电路。这样,我们不仅可以为BTC生成"有效UTXO集合",还能为BRC20代币生成账户余额证明。这对于构建去中心化的BRC20索引器具有重要意义。

总的来说,引入零知识证明技术可以显著优化比特币全节点的数据同步过程,降低全节点的存储和带宽需求。通过设计合适的证明电路,我们可以生成关于UTXO状态、Ordinals、BRC20余额等关键信息的紧凑证明,进而支持更加高效、安全、易用的比特币基础设施和应用生态。这种"零知识化"的升级路线有望成为比特币协议未来演进的重要方向之一。





Bitcoin's full node data synchronization is a time-consuming and resource-intensive process that requires downloading and verifying all block data starting from the genesis block. Each block consists of a block header and block content, with the block content containing multiple transactions, each corresponding to a transfer we send on the Bitcoin network.

When a node receives a new block broadcast, the consensus code needs to deserialize it and verify its proof-of-work, previous block, transaction Merkle tree, and the validity of each transaction. To optimize this process, we can introduce the concept of a "valid UTXO set," which represents the set of all unspent transaction outputs (UTXOs). Whenever a transaction consumes certain UTXOs, they are removed from the set; whenever a transaction creates new UTXOs, they are added to the set.

Furthermore, we can design a zero-knowledge proof circuit similar to the Bitcoin consensus code. This circuit takes the current "valid UTXO set" and the latest block as inputs, parses and validates the block, and outputs an updated "valid UTXO set" along with a corresponding proof. By repeatedly calling this circuit, we can start from the genesis block and gradually update the "valid UTXO set" until the latest block height. This way, we obtain the current "valid UTXO set" of the entire Bitcoin network and its proof.

Based on this mechanism, new Bitcoin full nodes no longer need to synchronize all block data from scratch. Instead, they only need to download the latest "valid UTXO set" and its proof. After verifying the validity of the proof, the node can retain this set and start synchronizing from the next block, updating the set after each synchronization. This approach can significantly reduce the time required for data synchronization, which is a major improvement for full nodes in low-bandwidth environments.

To avoid the difficulty of generating proofs due to large data sizes, the above iteration process may be performed on a per-transaction basis rather than a per-block basis. Additionally, the "valid UTXO set" can be implemented using data structures such as Merkle trees or Verkle trees, which allow for targeted adjustments to the parts that need to be modified without reconstructing the entire tree. The UTreeXO technology proposed by MIT is also a possible implementation path.

On top of this Bitcoin proof circuit, we can further implement proof circuits for the Ordinals protocol and the BRC20 protocol. This way, we can not only generate a "valid UTXO set" for BTC but also generate account balance proofs for BRC20 tokens. This has significant implications for building decentralized BRC20 indexers.

Overall, introducing zero-knowledge proof technology can significantly optimize the data synchronization process for Bitcoin full nodes, reducing storage and bandwidth requirements. By designing appropriate proof circuits, we can generate compact proofs of key information such as UTXO states, Ordinals, and BRC20 balances, thereby supporting more efficient, secure, and user-friendly Bitcoin infrastructure and application ecosystems. This "zero-knowledge" upgrade path is likely to become an important direction for the future evolution of the Bitcoin protocol.
