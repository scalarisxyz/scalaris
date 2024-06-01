# Scalaris: High-Performance, Leaderless, Parallel, and MEV-Mitigated Consensus Framework
**Version 0.1**  
**Scalaris Team**

## 1. Abstract
Scalaris is a novel framework designed to facilitate the rapid development of decentralized applications by leveraging advanced consensus protocols. Initially built on the cutting-edge research of Narwhal and Bullshark, Scalaris introduces a leaderless, parallel, high-throughput, scalable, and MEV-mitigation consensus mechanism. Additionally, we are exploring the integration of a new DAG-based and leaderless consensus protocol called Mysticeti, which promises a throughput of up to 400,000 transactions per second (TPS) and very low latency for consensus commits, around 0.5 seconds. This whitepaper details the architectural design, core features, theory foundations, and performance benefits of Scalaris, providing a comprehensive understanding of its capabilities and advantages.

## 2. Introduction
The evolution of blockchain technology can be categorized into three distinct generations. The first generation, exemplified by Bitcoin and Ethereum 1.0, introduced the foundational concepts of decentralized digital currencies and smart contracts. While revolutionary, these platforms faced significant scalability issues due to their reliance on proof-of-work (PoW) consensus mechanisms, which are inherently resource-intensive and slow.

The second generation of blockchain technology sought to address these limitations through the introduction of proof-of-stake (PoS) chains such as Cosmos, Polkadot, and other Tendermint-based networks. These systems improved scalability and energy efficiency by shifting away from PoW. However, they still suffered from a critical limitation: the reliance on a designated leader in each consensus round or epoch. By design, a leader is nominated in each round or epoch to collect transactions, propose blocks, and drive the consensus process. This leader-centric approach often becomes a bottleneck, as the leader must collect, communicate, and drive consensus, limiting overall system scalability and efficiency. Despite these challenges, numerous successful projects have been built on Tendermint's PoS framework, including Binance Chain, Terra, Oasis Labs, IRISnet, Regen Network, Akash Network, Injective Protocol, Secret Network, Persistence, and Sentinel. While these projects have achieved significant success, they still face fundamental limitations that can be addressed by more advanced consensus mechanisms.

Scalaris represents the third generation of blockchain technology. Built on the innovative research of Narwhal and Bullshark, Scalaris eliminates the need for a leader in each consensus round, thereby overcoming the scalability bottleneck inherent in earlier systems. Unlike Aptos and Sui, which also leverage Narwhal and Bullshark, Scalaris introduces a novel mechanism for mitigating Miner Extractable Value (MEV) by randomizing the order of prior pending transactions during the commitment process. This leaderless, parallel consensus framework ensures high throughput, fault tolerance, and resilience to MEV attacks, setting Scalaris apart as a true innovator in the blockchain space. Additionally, we will introduce Mysticeti into Scalaris to achieve a performance of up to 400,000 transactions per second (TPS) and very low latency for consensus commits, around 0.5 seconds. With Scalaris, we can build superior versions of existing successful projects like Oasis Labs, Celestia, Injective Protocol, which were previously built on the second-generation PoS-based Tendermint, thereby enhancing their performance and scalability to new heights.

## 3. Background
Traditional Byzantine Fault Tolerant (BFT) proof-of-stake (PoS) consensus mechanisms, widely used in many successful blockchain projects such as Binance Chain, Terra, Oasis Labs, and Celestia, have significantly improved scalability and energy efficiency over proof-of-work systems. However, these systems face several critical limitations that hinder their overall performance and scalability.

1. **Leader Bottleneck**: In leader-based consensus protocols, a single nominated leader is responsible for collecting transactions, proposing blocks, and driving the consensus process. This role creates a bottleneck, as the leader's resources (storage, CPU, and bandwidth) are heavily utilized, while other validators remain underutilized. This imbalance limits the system's ability to scale efficiently, as the leader's capacity becomes the primary constraint on performance.

2. **Redundant Communication**: Traditional BFT consensus protocols necessitate multiple rounds of communication among validators to reach consensus on the same set of transactions. Each round involves broadcasting messages to all validators, resulting in redundant communication that increases overall message complexity and network load. This redundancy not only reduces efficiency but also lengthens block time.

3. **Transaction Data Sharing**: In these consensus mechanisms, transaction data is shared among validators as part of the consensus process. This approach leads to inefficiencies and increased overhead, as the same data must be transmitted and processed multiple times. The repeated transmission of transaction data contributes to higher network congestion and resource consumption.

The aforementioned limitations result in significant constraints on the transactions per second (TPS) that traditional BFT-based PoS consensus protocols can achieve. Scalaris addresses these limitations by building on the innovative research of Narwhal & Bullshark, introducing a leaderless consensus mechanism that fundamentally improves upon traditional BFT-based PoS protocols.

* **Zero Communication Overhead**: In Scalaris, validators independently examine their local view of the Directed Acyclic Graph (DAG) to fully order all vertices without the need for additional message exchanges. This leaderless approach eliminates the communication overhead typically associated with multiple rounds of consensus communication in leader-based systems. By allowing each validator to rely solely on its local view for consensus, Scalaris enhances overall efficiency and significantly reduces latency.

* **Separation of Data Propagation and Consensus**: Scalaris separates the tasks of data propagation and consensus, allowing them to run in parallel. Transaction data is disseminated efficiently, while the consensus protocol focuses on ordering metadata references rather than the full transaction data. This separation enhances throughput and reduces the complexity of the consensus process.

* **Metadata Consensus**: By focusing on metadata rather than full transaction data, Scalaris reduces the complexity and size of messages that need to be transmitted during the consensus process. This approach significantly enhances scalability and performance by minimizing the amount of data that must be communicated and processed.

Scalaris leverages these innovations to create a high-performance, leaderless, parallel consensus framework that offers several key advantages:

* **High Throughput**: Scalaris achieves significantly higher throughput compared to traditional BFT-based PoS consensus protocols, as demonstrated by Narwhal's ability to handle over 150,000 transactions per second (tx/sec) with low latency.

* **MEV Mitigation**: Scalaris introduces a novel mechanism for mitigating Miner Extractable Value (MEV) by randomizing the order of fixed set of transactions during the commitment process. This prevents transaction ordering manipulation and ensures fair transaction processing.

Scalaris is a framework for building blockchains quickly, utilizing state-of-the-art DAG leaderless consensus protocols such as Narwhal & Bullshark, and Mysticeti. Mysticeti, in particular, offers very high throughput, up to 400,000 transactions per second (TPS), and low latency for consensus commits, around 0.5 seconds. By integrating these advanced consensus mechanisms, Scalaris sets a new standard for blockchain performance and scalability, making it an ideal choice for building next-generation decentralized applications.

## 4. DAG-based Consensus
In the context of blockchain technology, Byzantine Fault Tolerant (BFT) consensus involves n validators, up to f of whom may act maliciously, working to agree on an ever-expanding sequence of transactions. The concept behind DAG-based BFT consensus, as seen in protocols like HashGraph and Aleph, is to decouple the network communication layer from the consensus logic. Each message comprises a set of transactions and references to previous messages. Collectively, these messages form a continuously expanding Directed Acyclic Graph (DAG) – with each message as a vertex and its references as edges.

One of the key advantages of DAG-based consensus is that it introduces zero additional communication overhead. Each validator independently examines its local view of the DAG and fully orders all vertices without sending any extra messages. The structure of the DAG itself is interpreted as the consensus protocol, where a vertex represents a proposal and an edge signifies a vote.

A significant challenge in this context arises from the asynchronous nature of the network, meaning that different validators may see slightly different DAGs at any given time. The primary difficulty lies in ensuring that all validators ultimately agree on the same total order of transactions.

## 5. Narwhal: Achieving Scalability and Throughput
The key insight from the Narwhal & Tusk paper is that scalable, high-throughput consensus systems should separate data dissemination from the ordering mechanism for reliability.

Narwhal employs a highly scalable and efficient Directed Acyclic Graph (DAG) structure, which, in conjunction with Tusk/Bullshark, processes over 100,000 transactions per second (tps) in a geo-replicated environment while maintaining latency below three seconds. In contrast, Hotstuff handles fewer than 2,000 tps under similar conditions.

Narwhal's symmetric data dissemination among validators ensures fault resilience, as the system's performance is only impacted when faulty validators fail to disseminate data.

Each validator in Narwhal consists of multiple workers and a primary node. Workers continuously exchange data batches and forward batch digests to their primaries. The primaries construct a round-based DAG using these digests. Crucially, data dissemination by workers occurs at network speed, independent of the DAG construction pace by the primaries. Validators use a broadcast protocol ensuring reliable communication with a linear message count on the critical path.

During each round (refer to Figure 2 in the Narwhal & Tusk paper for a visual guide):

1. Every validator constructs and dispatches a message to all other validators containing the metadata for the DAG vertex, which includes batch digests and n-f references to vertices from the previous round.
2. Upon receiving this message, a validator responds with a signature if:
   1. Its workers have stored the data corresponding to the digests in the vertex (ensuring data availability).
   2. It has not yet responded to this validator in the current round (ensuring non-equivocation).
3. The sender aggregates these signatures to form a quorum certificate from n-f responses and includes this certificate in its vertex for the round.
4. A validator progresses to the next round once it has received n-f vertices accompanied by valid certificates.

Quorum certificates serve three main purposes:

1. **Non-equivocation**: Validators sign only one vertex per round, ensuring consistency in the DAG. Byzantine validators cannot generate conflicting quorum certificates for the same round.
2. **Data availability**: Validators sign only if they have stored the corresponding data, guaranteeing that data can be retrieved later by any validator.
3. **History availability**: Certificates of blocks from the previous round ensure the availability of the entire causal history. A new validator can join by learning the certificates from the prior round, simplifying data management.

By decoupling data dissemination from metadata ordering, Narwhal ensures network-speed throughput, regardless of the DAG construction or consensus latency. Data dissemination happens at network speed, while the DAG references all disseminated data. Once a DAG vertex is committed, its entire causal history (including most disseminated data) is ordered.

## 6. The Bullshark Protocol
The Bullshark paper introduces two versions of the protocol: an asynchronous protocol with a 2-round fast path during synchrony, and a partially synchronous 2-round protocol. Let's focus on the partially synchronous version.

In traditional BFT leader-based consensus protocols, the complexity often lies in the view-change and view-synchronization mechanisms. Many protocols, such as Hotstuff and Raft, simplify their presentation by omitting synchronization details. For instance, Hotstuff was chosen for the Diem project due to its perceived simplicity. However, the engineering team later discovered that the Pacemaker component, assumed to handle view synchronization seamlessly, was complex and challenging to implement, becoming a significant bottleneck under fault conditions.

Bullshark eliminates the need for view-change and view-synchronization mechanisms!

With Bullshark, the DAG encodes complete information, eliminating the need to "agree" on skipping or discarding slow or faulty leaders via timeout complaints. Once a validator commits an anchor (leader) vertex v on the DAG, it traverses back through v’s causal history, ordering anchors it previously skipped but could have been committed by other validators. The round-based DAG’s all-to-all communication inherently handles synchronization.

Additionally, the DAG construction in Bullshark is leaderless, ensuring perfect symmetry and load balancing among validators. DAG vertices are totally ordered without any extra communication among validators. Instead, each validator independently interprets its view of the DAG structure. Below is an illustration of the Bullshark protocol with n=4 and f=1.

In each odd round of the DAG, there is a predefined anchor vertex (highlighted in solid green above). The primary objective is to determine which anchors to commit first. To achieve a total ordering of all vertices in the DAG, a validator sequentially processes all committed anchors, ordering their causal histories according to a deterministic rule. In the illustration above, the causal history of anchor A2 is marked by green vertices.

In each even round, vertices can cast a vote for the anchor of the preceding round. Specifically, a vertex in round r votes for the anchor of round r−1 if there is an edge connecting them. The commit rule is straightforward: an anchor is committed if it receives at least f+1 votes. In the figure above, Anchor A2 is committed with 3 votes, whereas Anchor A1 only has 1 vote and remains uncommitted.

Due to the asynchronous nature of the network, validators may have different local views of the DAG. Some vertices may be delivered and added to the local DAG view of some validators but not yet to others. Consequently, while some validators may not have committed A1, others might have.

In the example provided, Validator 2 commits anchor A1 after seeing two (f+1) votes for it, even though Validator 1 has not yet committed it. To ensure total order (safety), Validator 1 must order anchor A1 before anchor A2. Bullshark achieves this through quorum intersection.

The commit rule requires f+1 votes, and each vertex in the DAG has at least n-f edges to vertices from the previous round. This guarantees that if any validator commits an anchor A, all future anchors will have a path to at least one vertex that voted for A, and consequently, to A itself.

This leads to the following corollary:

**Safe to skip**: If there is no path from a future anchor to an anchor A, it is guaranteed that no validator committed A, making it safe to skip A.

The anchor ordering mechanism operates as follows: when an anchor i is committed, the validator checks for a path from anchor i to anchor i−1. If such a path exists, anchor i−1 is ordered before anchor i, and the process is recursively applied starting from i−1. If no path exists, anchor i−1 is skipped, and the validator checks for a path from i to i−2. This process continues, ordering anchors if paths exist and skipping them otherwise, until an anchor that was previously committed is reached, ensuring that all preceding anchors are already ordered.

In the figure above, anchors A1 and A2 do not receive enough votes to be committed. When the validator commits A3, it must determine whether to order A1 and A2. Since there is no path from A3 to A2, according to the "safe to skip" corollary, A2 can be skipped. However, there is a path from A3 to A1, so A1 must be ordered before A3 (it is possible that some validators committed A1).

The validator then checks for a path from A1 to A0 (not shown in the figure) to decide whether A0 should be ordered before A1.

For further details on the specific DAG construction, refer to the Bullshark paper, which provides formal pseudocode and rigorous proofs of Safety and Liveness.

### 6.1 Fairness and Garbage Collection in DAG-Based BFT
A key fairness property in blockchain systems is ensuring that no validator is ignored, allowing even slow validators to contribute transactions to the total order. In the context of DAG-based Byzantine Fault Tolerance (BFT), it is essential that every validator can add vertices to the DAG.

Since up to f validators can be Byzantine and might never broadcast their vertices, it is challenging to distinguish between these and slow validators during asynchronous periods. Honest validators should not wait for more than n−f vertices to proceed to the next round. Consequently, a slow validator may always be late, unable to add its vertex to the DAG as other validators move to round i before receiving its vertex for round i−1.

To address this issue, DAG-Rider introduced weak edges that point to vertices from older rounds. These edges are ignored by the consensus mechanism and do not count as votes. Their sole purpose is to ensure that all vertices from all honest validators eventually get added to the DAG and become part of the total order in a future anchor's causal history.

A challenge with DAG-based BFT systems is garbage collection. During asynchronous periods, it may take an indefinite amount of time for a vertex to be delivered, making it unsafe to garbage collect rounds that do not have all vertices in the DAG. This is impractical for real-world applications.

Bullshark strikes a balance between garbage collection and fairness. It ensures constant garbage collection while maintaining fairness during synchronous periods.

To achieve this, validators attach their local time to each vertex they broadcast. Once an anchor is committed and ready for ordering, the anchor’s timestamp is calculated as the median of the timestamps of its parent vertices. While traversing back through the anchor’s causal history to deterministically order it, the timestamp for each round is computed as the median of the timestamps in that round's vertices. When encountering a round r with a timestamp that is older than the anchor’s timestamp by more than a predefined Δ time, round r and all earlier rounds are garbage collected. Vertices not yet added to the DAG in these rounds will need to be re-broadcast in future rounds. However, it is guaranteed that if a slow validator can broadcast its vertex within the Δ time, the vertex will be added to the DAG and be totally ordered.

## 7. Mysticeti: Enhanced Consensus Protocol for Scalaris
### 7.1 DAG Structure
Similar to the Narwhal DAG, Mysticeti builds its DAG in a sequence of logical rounds. For each round, every honest validator proposes a unique signed block. Byzantine validators may attempt to equivocate by sending multiple distinct blocks to different parties or none at all. These equivocated blocks make the Mysticeti DAG uncertified, contrasting with the certified version in the Narwhal DAG. Each block in the Mysticeti DAG contains:

1. **Creator Authority**: Identifies the creator, includes a round number, and a digest (reference for the block).
2. **Base Statements**: Transaction payloads received from users or transaction votes.
3. **Included References**: References that the block depends on, which must be processed before this block is processed.
4. **Signature**: A signature from the creator over all the data.

A block is valid if it satisfies the following conditions:

1. Includes the authority and its signature on the block contents.
2. Contains a round number.
3. Lists transactions (can be empty).
4. Has at least 2f+1 distinct block references from the previous round, or empty references for blocks from the first round.

Validators broadcast their block to others to include as references in proposed blocks in the next round.

### 7.2 Consensus Protocol
Mysticeti introduces the concept of a proposer slot as a tuple (validator, round), which can either be empty or contain the validator’s proposal for the respective round. Unlike Bullshark, which has a proposer slot every two rounds resulting in high latencies, Mysticeti addresses this by introducing multiple proposer slots with three states: to-commit, to-skip, and undecided for each round. The default state of proposer slots is undecided.

The end goal of Mysticeti is to mark all proposer slots as either to-commit or to-skip by detecting specific DAG patterns:

* **Skip Pattern**: The skip pattern is identified if for all proposals, we observe 2f+1 subsequent blocks that do not support it. The skip pattern, illustrated by Figure 2 (left), where at least 2𝑓 + 1 blocks at round 𝑟 + 1 do not support a block (𝐴, 𝑟, ℎ).
* **Certificate Pattern**: The certificate pattern, illustrated by Figure 2 (right), where at least 2𝑓 +1 blocks at round 𝑟 +1 support a block 𝐵 ≡ (𝐴, 𝑟, ℎ). We then say that 𝐵 is certified. Any subsequent block (illustrated at 𝑟 + 2) that constrains in its history such a pattern is called a certificate for the block 𝐵.

### 7.2.1 Decision Steps

1. **Direct decision rule**
   * The validator marks a slot as to-commit if it observes 2𝑓 + 1 commit patterns for that slot, that is, if it accumulates 2𝑓 +1 distinct implicit certificate blocks
   * The direct decision rule marks a slot as to-skip if it observes a skip pattern for that slot.

2. **Indirect decision rule**: if the direct decision rule fails to determine the slot, the validator resorts to the indirect decision rule to attempt to reach a decision for the slot. This rule operates in two stages. It initially searches for an anchor, which is defined as the first slot with the round number (𝑟 ′ > 𝑟 + 2) that is already marked as either undecided or to-commit
   * If the anchor is marked as undecided the validator marks the slot as undecided (Figure 3d)
   * If the anchor is marked as to-commit, the validator marks the slot either as to-commit if the anchor causally references a certificate pattern over the slot (figure 3e) or as to-skip in the absence of a certificate pattern (Figure 3f)

### 7.2.2 Commit Phase
After processing all slots, the validator derives an ordered sequence of slots. The validator then iterates over this sequence, committing all slots marked as to-commit and skipping all slots marked as to-skip. This iteration continues until the first undecided slot is encountered.

### 7.2.3 Summary
Mysticeti enhances the Scalaris framework by providing a more efficient consensus protocol with the potential to achieve up to 400,000 transactions per second (TPS) and very low latency for consensus commits, around 0.5 seconds. By integrating Mysticeti, Scalaris can further improve its performance and scalability, offering a robust solution for the next generation of decentralized applications.

## 8. Scalaris Framework
Scalaris combines the strengths of Narwhal and Bullshark to create a versatile and powerful framework for building app chains. The framework provides several key features:

1. **Leaderless Consensus**: Unlike traditional BFT protocols that rely on a designated leader to propose blocks, Scalaris operates without a leader, thereby reducing the risk of bottlenecks and single points of failure. This is achieved through a randomized approach to block proposal and validation, ensuring fair and distributed participation among validators.
2. **High Throughput**: Scalaris leverages the efficient transaction dissemination of Narwhal and the streamlined consensus process of Bullshark to achieve throughput levels significantly higher than traditional BFT protocols. Experimental results show that Narwhal-HotStuff can achieve over 130,000 transactions per second (tx/sec) with sub-2-second latency, while Tusk (the consensus layer of Bullshark) can reach 160,000 tx/sec with about 3 seconds or less of latency.
3. **Scalability**: The framework supports horizontal scaling through the addition of worker nodes, allowing for quasi-linear increases in throughput without compromising latency. This scale-out design ensures that Scalaris can handle growing demand and large-scale deployment scenarios.
4. **Resilience and Fault Tolerance**: By decoupling data dissemination from consensus and employing a robust DAG-based structure, Scalaris maintains high performance even in the presence of network asynchrony or validator faults. The framework's design ensures that transaction blocks are consistently available and verifiable, contributing to overall system stability and reliability.
5. **MEV Mitigation**: Scalaris implements a novel approach to mitigate the potential risks associated with Maximal Extractable Value (MEV). By incorporating a dedicated MEV mitigation feature into its consensus process, Scalaris aims to prevent malicious actors from exploiting transaction ordering for their own benefit.
6. **Compatibility with Cosmos SDK**: Scalaris Framework combines the strengths of Narwhal and Bullshark to create a versatile and powerful framework for building app chains. In addition to its leaderless consensus and high throughput, Scalaris also provides an adapter that makes it compatible with the Cosmos SDK.

### 8.1 Parallel Consensus
Scalaris leverages DAG-based leaderless consensus protocols such as Narwhal & Bullshark and Mysticeti to implement a novel approach known as parallel consensus. This approach fundamentally enhances the efficiency and scalability of the consensus process by eliminating traditional bottlenecks and communication overhead.

#### Key Features of Parallel Consensus
1. **Elimination of Communication Overhead**: Traditional consensus protocols often require multiple rounds of communication among validators to agree on the same set of transactions. This process can be slow and resource-intensive. Scalaris eliminates this overhead by allowing validators to independently examine their local view of the DAG and fully order all vertices without needing additional message exchanges. This leaderless approach drastically reduces the communication overhead typically associated with consensus processes.
2. **Separation of Data Propagation and Consensus**: Scalaris separates the tasks of data propagation and consensus, allowing these processes to run in parallel. Transaction data is efficiently disseminated throughout the network, while the consensus protocol focuses on ordering metadata references rather than the full transaction data. This separation enhances throughput and simplifies the consensus process, as validators can commit new blocks by referencing the local DAG structure.
3. **Local View Consensus**: Each validator in Scalaris can independently reach consensus by examining its local view of the DAG. Validators make commit decisions based on the local structure of the DAG, which includes all necessary information about previous transactions and blocks. This approach ensures that consensus can be achieved quickly and efficiently without the need for extensive coordination among validators.

### 8.2 Scalaris Architecture
Scalaris is designed as a state-of-the-art framework for building blockchains utilizing DAG leaderless consensus protocols, providing significant performance benefits over traditional consensus mechanisms. The architecture of Scalaris integrates several innovative components to ensure compatibility with existing blockchain ecosystems while maximizing efficiency, scalability, and robustness.

At the core of Scalaris is its parallel consensus mechanism, based on DAG leaderless consensus protocols such as Narwhal & Bullshark and Mysticeti. This mechanism offers several key advantages:

* **High Throughput**: By eliminating the communication overhead associated with multiple rounds of consensus and allowing validators to commit new blocks by examining their local views, Scalaris achieves significantly higher throughput, capable of handling up to 400,000 transactions per second (TPS).
* **Reduced Latency**: Separating data propagation from the consensus process and allowing independent local view consensus reduces overall latency, enabling rapid transaction finalization.
* **Efficiency and Scalability**: Validators can fully utilize their computational resources, continuously processing and committing transactions without waiting for extensive coordination. This enhances the efficiency and scalability of the entire network.

### 8.2.1 Compatibility with ABCI and Cosmos SDK
Scalaris is designed to be compatible with the Application Blockchain Interface (ABCI) from the Cosmos SDK, facilitating seamless integration with existing Cosmos-based blockchains. To achieve this, Scalaris includes a specialized adapter:

* **Scalaris ABCI Adapter**: This component provides a gRPC client for communication with the Scalaris consensus process, enabling Cosmos SDK-based applications to leverage the advanced capabilities of the Scalaris framework without significant modifications.

### 8.2.2 Support for EVM Execution
To ensure broad compatibility and facilitate the transition for Ethereum-based applications, Scalaris supports the Ethereum Virtual Machine (EVM) execution environment through a dedicated consensus client:

* **Scalaris Consensus Client for ETH Execution Layer**: This component includes Scalaris as the consensus engine and a consensus adapter for communication with Ethereum execution nodes. The adapter listens to pending transactions from ETH execution nodes and commits blocks received from Scalaris, enabling Ethereum applications to benefit from Scalaris' high-performance consensus.

### 8.2.3 Support for Move Language
Scalaris is also designed to support the Move language, widely used in the Sui blockchain. This is achieved by replacing local calls in Sui with remote calls utilizing the Scalaris framework:

* **Move Language Integration**: By leveraging Scalaris for remote calls, the framework enhances the scalability and performance of Move-based applications, allowing them to benefit from the robust, parallel consensus mechanism of Scalaris.

### 8.3 MEV Mitigation in the Scalaris Framework
#### 8.3.1 Understanding MEV Attacks
MEV (Miner Extractable Value) attacks occur when validators manipulate transaction order within a block to gain financially. In traditional BFT-based leader-driven proof-of-stake chains, including Ethereum and other Tendermint based chain, the nominated leader can exploit their role by reordering or manipulating specific transactions to maximize their own profit.

#### 8.3.2 MEV in Old BFT-Based Blockchains
In these systems, the leader can follow the protocol while manipulating the transaction order and block contents for financial gain. This centralized decision-making allows the leader to benefit at the expense of transaction fairness.

#### 8.3.3 Scalaris Framework Mitigation
The Scalaris framework addresses MEV attacks through a novel commit phase approach:

1. **Transaction Collection**: Validators collect all transactions in the sub-DAG of an anchor, encompassing all transactions from the previous round.
2. **Fixed Transaction Set**: Validators work with a predetermined set of transactions, ensuring transparency before shuffling.
3. **Random Shuffling**: Transactions are shuffled using a verifiable and unpredictable random seed number generated in the next round. This ensures the final transaction order cannot be predicted in advance.
4. **Deterrence of Malicious Behavior**: While a malicious validator may attempt to insert beneficial transactions, the unpredictable nature of the random seed prevents them from ensuring a favorable order. This significantly reduces the success of MEV attacks.

By employing these mechanisms, the Scalaris framework ensures fair and tamper-resistant transaction ordering, effectively mitigating the risk of MEV attacks and enhancing blockchain integrity.

### 8.4 Parallel Transaction Execution for EVM in Scalaris Framework
#### 8.4.1 Challenges with Parallel Execution
The traditional transaction execution process in blockchains, such as Ethereum, operates in a sequential manner. This involves the following steps:

* The execution module sequentially processes each transaction from the block.
* The latest world state is updated after each transaction is executed, ultimately reflecting the state after the entire block is processed.
* The execution of the next block depends on the final state from the current block, necessitating a sequential, single-threaded process.

This sequential execution model is not conducive to parallel execution due to several conflicts:

* **Account Conflict**: Concurrently processing the balance or other attributes of an account by multiple threads can lead to inconsistencies.
* **Storage Conflict of the Same Address**: Simultaneous modifications to the storage of the same global variable by different contracts can cause conflicts.
* **Cross-Contract Call Conflict**: When contracts depend on the deployment or execution order of other contracts, parallel execution without proper sequencing can lead to errors.

#### 8.4.2 Parallel Transaction Executor (PTE)
To address these challenges, Scalaris incorporates a Parallel Transaction Executor (PTE) based on the Directed Acyclic Graph (DAG) model. PTE offers several advantages:

* **Utilization of Multi-Core Processors**: PTE leverages multi-core processors to execute transactions in parallel, significantly improving performance.
* **Simple Programming Interface**: PTE provides a user-friendly interface, abstracting the complexities of parallel execution.
* **Performance Improvement**: Benchmark tests show that PTE can achieve 200% to 300% performance improvement on a 4-core processor compared to traditional serial execution, with performance scaling with the number of cores.

#### 8.4.3 General Scheme
PTE constructs a transaction-dependent DAG based on the sequence of transactions in the block and their mutual resource dependencies. Transactions with no dependencies (inbound degree of 0) can be executed in parallel. The process involves:

* Identifying mutually exclusive resources occupied by each transaction.
* Constructing a transaction DAG according to these dependencies.
* Executing transactions in parallel using topological sorting of the DAG.

#### 8.4.4 Modular Architecture
The architecture of Scalaris for parallel transaction execution involves several key components:

* **Transaction Initiation**: Users initiate transactions directly or indirectly through the SDK.
* **Transaction Synchronization and Packaging**: Transactions are synchronized between nodes, packaged into blocks by the Sealer (TxPool), and sent to the Consensus unit for consensus.
* **Transaction Validation and DAG Construction**: Before consensus, PTE reads transactions from the block, constructs a transaction DAG based on dependencies, and prepares for parallel execution.
* **Parallel Execution and State Calculation**: PTE uses multiple threads to execute the transaction DAG in parallel. After execution, the Joiner calculates the state root and receipt root based on state modifications, returning the results to the upper layer.
* **Block Verification and Storage**: Once the block is verified and consensus is reached, it is written to the blockchain's underlying storage.

#### 8.4.5 Construction Process of Transaction DAG
The DAG construction process involves:

1. Extracting all transactions from the packed block.
2. Initializing a DAG instance with the number of transactions as the maximum number of vertices.
3. Reading transactions sequentially, identifying conflicts, and creating dependency edges based on conflict fields.
4. Ensuring that non-mergeable transactions are executed sequentially by creating dependency edges with all preceding transactions.

#### 8.4.6 DAG Execution Process
The DAG execution process includes:

1. Initializing a pool of threads based on the number of hardware cores.
2. Executing ready transactions (in-degree of 0) using the threads, with each thread processing transactions independently.
3. Continuously executing transactions until all DAG vertices are processed, ensuring efficient utilization of multi-core processors.

## 9. Conclusion
Scalaris represents a significant advancement in the field of decentralized consensus protocols. By integrating the cutting-edge research of Narwhal, Bullshark, and Mysticeti, Scalaris aims to achieve unparalleled performance, reaching up to 400,000 transactions per second (TPS). Scalaris introduces both parallel consensus and parallel execution layers, ensuring maximum efficiency and scalability. Additionally, Scalaris offers the first market-ready MEV-mitigation mechanism, addressing a critical vulnerability in blockchain systems. Its leaderless consensus mechanism, high throughput, and robust architecture make Scalaris an ideal choice for building a wide range of decentralized applications, offering unmatched performance and security.

## References
1. Narwhal and Tusk: A DAG-based Mempool and Efficient BFT Consensus.
2. Bullshark: Asynchronous Byzantine Fault-Tolerant Consensus with High Performance.
3. "DAG meets BFT." Decentralized Thoughts.
4. Mysticeti: Reaching the Limits of Latency with Uncertified DAGs.
