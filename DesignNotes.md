
**CASSANDRA**
he problem with relational database scale up, high available and sharding.

Relational database: host stand-alone, hundreads of concurrent users, ACID guarantees.
replication lag (fairly, which system does not have rep lag).
Relational database for handle load.
Can be denormalize. This redundancy increases. The original ACID? Hmmm.
Sharding separates the data. Joins, aggregation, the problems that the original RDBMS can solve are not available.
Even if you use the first layer index to shard, if you want to query other indexes. It is necessary to hit all the shards, and you must also use external aggregation.
The addition and subtraction of sharding requires manual movement of data.

High availability (using amazon's integrated products can probably be avoided?) implementation has a lot of operational overhead. (multi-DC)

cass (referred to as Cassandra's short name below) is:
* Abandon the difficult to achieve consistency.
* built in sharding.
* No master-slave. This should be a mountain.
* distributed.
* Using some kind of denormalization optimization, the purpose is to use only one shard in the query. External aggregation is of course not used anymore.

Some features:
high performance,
High availability
Linear scalability. Netflix seems to be really verifying this. From 50 nodes to 300 nodes.
Low latency.
There is no single point of attachment, and it is quite confused on multiple data centers.
No master-slave. Peer-to-peer. There is no leader election.
A lot of operational tools. OPS is convenient.
However, it is obvious that RDBMS cannot be completely replaced. And some (or many) features can't be copied. That is to say, you can't do ACID.
AP system. The delay between data centers makes it difficult to guarantee that the guarantee is abandoned. But not completely give up, each query can choose three levels of consistency level: ONE, QUORUM, ALL means query or write consistency. How many ACKs you need to get is considered successful. This setting will affect the consistency but will also affect: read and write speed. Data security, availability.

Famous hash ring
The partition key (consistent hashing) determines which shard to go to.
Replicated to X nodes.
There is no need for centralized zookeeper. Point-to-point gossip.

Replication is automatic. (The re-sharding of the RDBMS mentioned above is directly manual comparison). If a node hangs, the new write data is stored as a hint for the new alternate node to digest.
Because the common mode is multi-data center, there are two replication parameters: one is the number of replication inside the center. The other is the number of replications between centers.
The replication factor parameter is for keyspace.

concept:
The keyspace is the peer of the schema or database.
Hint is a concept in gossip that is a single write operation. Gossip will be learned separately later.
