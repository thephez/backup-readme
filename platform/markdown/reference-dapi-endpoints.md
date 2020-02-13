# Endpoint Overview

[DAPI](explanation-dapi) currently provides 2 types of endpoints: [JSON-RPC](https://www.jsonrpc.org/) and [gRPC](https://grpc.io/docs/guides/). The JSON-RPC endpoints expose some layer 1 information while the gRPC endpoints support layer 2 as well as streaming of events related to blocks and transactions/transitions.

## JSON-RPC Endpoints

| Layer | Endpoint | Description |
| :-: | - | - |
| 1 | [`getBestBlockHash`](reference-dapi-endpoints-json-rpc-endpoints#section-get-best-block-hash) | Returns block hash of the chaintip |
| 1 | [`getBlockHash`](reference-dapi-endpoints-json-rpc-endpoints#section-get-block-hash) | Returns block hash of the requested block |
| 1 | [`getMnListDiff`](reference-dapi-endpoints-json-rpc-endpoints#section-get-mn-list-diff) | Returns masternode list diff for the provided block hashes |
| 1 | [`getUTXO`](reference-dapi-endpoints-json-rpc-endpoints#section-get-utxo) | Returns unspent transaction outputs for a given address |

## gRPC Endpoints

| Layer | Endpoint | |
| :-: | - | - |
| 1 | `getBlock` | Returns information for the requested block |
| 1 | `getEstimatedTransactionFee` | Returns estimate of required transaction fee |
| 1 | `getStatus` | Returns blockchain status information |
| 1 | `getTransaction` | Returns details for the requested transaction |
| 1 | `sendTransaction` | Submits the provided transaction |
| 1 | `subscribeTo` `TransactionsWithProofs` | Returns transactions matching the provided bloom filter along with the associated [`islock` message](https://dashcore.readme.io/docs/core-ref-p2p-network-instantsend-messages#section-islock) and [merkle block](https://dashcore.readme.io/docs/core-ref-p2p-network-data-messages#section-merkle-block) |
| 2 | [`applyStateTransition`](reference-dapi-endpoints-platform-grpc-endpoints#section-apply-state-transition) | Applies the provided State Transition |
| 2 | [`getIdentity`](reference-dapi-endpoints-platform-grpc-endpoints#section-get-identity) | Returns the requested identity |
| 2 | [`getDataContract`](reference-dapi-endpoints-platform-grpc-endpoints#section-get-data-contract) | Returns the requested data contract |
| 2 | [`getDocuments`](reference-dapi-endpoints-platform-grpc-endpoints#section-get-documents) | Returns the requested document(s) |