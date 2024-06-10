# 🖥️ Indexing and Validation layers

The Indexing and Validation Layers of Rainbow Protocol are crucial components ensuring the correctness and integrity of transactions and contracts.

### Indexing Layer

Decentralized indexers identify and parse transactions on the Bitcoin network. Transactions that comply with protocol rules are extracted and sent to the validation layer for verification. We need to parse every Bitcoin transaction, so indexers need to connect to Bitcoin full nodes and obtain data in a timely manner. This part of the software works off-chain and is also known as the Worker. Similar to other overlay protocols, users can run indexers independently. However, indexers typically face two unresolved issues:

1. All indexer software must run and provide correct data. Incorrect data will lead to incorrect asset balances and rule errors. Therefore, the index versions of most overlay protocols must be consistent; otherwise, "forks" and divergences will occur.
2. Most overlay protocol index nodes lack effective incentives. Essentially, indexing is a service provided to users. To run efficiently, this software often needs to operate in a server environment. For most ordinary users, the cost of running and maintaining these index nodes is high. As a result, most overlay protocol index nodes are provided by large service providers, which further exacerbates network centralization.

Rainbow Protocol's index nodes should be able to run in a distributed manner and obtain corresponding incentives during development. This ensures the quality of indexing operations and encourages active participation from users.

### **Validation Layer**

The validation layer consists of a network of multiple Bitcoin light nodes and smart contracts. Its purpose is to validate Bitcoin transactions and Rainbow operation scripts, ensuring their integrity and correctness. Unlike the indexing layer, the validation layer works in collaboration with smart contracts, participating in data and identity authentication. Therefore, it can theoretically participate in incentive distribution.

Rainbow Protocol will upgrade the indexing and validation layers to ensure that individuals and organizations participating in indexing and validation can obtain effective incentives, promoting the decentralized development of Rainbow Protocol.

***

