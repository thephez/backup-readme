# Introduction

These tutorials aim to give readers a solid foundation on how to interact with [GroveDB](https://github.com/dashpay/grovedb) so that they may begin using it for their own applications.

GroveDB is the Merkle tree structure overlaying Dash Platform's RocksDB backing store which allows for the most complex, cryptographically verifiable, queries of all databases in existence today. Existing solutions have very limited capabilities compared to non-cryptographically verifiable databases such as MongoDB, which motivated Dash Core Group to build their own solution. The design, a *grove* of Merkle AVL trees (specifically [Merks](https://github.com/nomic-io/merk)) interconnected via root hashes and references, is largely inspired by the paper [Database Outsourcing with Hierarchical Authenticated Data Structures](https://ia.cr/2015/351).

# Setup

It’s not required, but if you wish to follow along interactively, clone the [grovedb-tutorials](https://github.com/thephez/grovedb-tutorials) repo and open the tutorials directory.

```
git clone https://github.com/thephez/grovedb-tutorials.git && cd tutorials
```

To run a tutorial, do:

```
cargo run --bin <tutorial name>
```

Valid tutorial names are: open, insert, delete, query-simple, query-complex, proofs.