[DAPI](explanation-dapi) currently provides 2 types of endpoints: [JSON-RPC](https://www.jsonrpc.org/) and [gRPC](https://grpc.io/docs/guides/). The JSON-RPC endpoints expose some layer 1 information while the gRPC endpoints support layer 2 as well as streaming of events related to blocks and transactions/transitions.

# JSON-RPC Endpoints
[block:callout]
{
  "type": "info",
  "title": "gRPC Migration",
  "body": "The remaining JSON-RPC endpoints will be migrated to gRPC in a future release"
}
[/block]
| Layer | Endpoint | Description |
| :-: | - | - |
| 1 | [`getBestBlockHash`](reference-dapi-endpoints-json-rpc-endpoints#getbestblockhash) | Returns block hash of the chaintip |
| 1 | [`getBlockHash`](reference-dapi-endpoints-json-rpc-endpoints#getblockhash) | Returns block hash of the requested block |
| 1 | [`getMnListDiff`](reference-dapi-endpoints-json-rpc-endpoints#getmnlistdiff) | Returns masternode list diff for the provided block hashes |

# gRPC Endpoints

## Core gRPC Service

| Layer | Endpoint | |
| :-: | - | - |
| 1 | [`broadcastTransaction`](reference-dapi-endpoints-core-grpc-endpoints#broadcasttransaction) | Broadcasts the provided transaction |
| 1 | [`getBlock`](reference-dapi-endpoints-core-grpc-endpoints#getblock) | Returns information for the requested block |
| 1 | [`getStatus`](reference-dapi-endpoints-core-grpc-endpoints#getstatus) | Returns blockchain status information<br>**Updated in Dash Platform v0.19** |
| 1 | [`getTransaction`](reference-dapi-endpoints-core-grpc-endpoints#gettransaction) | Returns details for the requested transaction |
| 1 | [`subscribeTo` `TransactionsWithProofs`](reference-dapi-endpoints-core-grpc-endpoints#subscribetotransactionswithproofs) | Returns transactions matching the provided bloom filter along with the associated [`islock` message](https://dashcore.readme.io/docs/core-ref-p2p-network-instantsend-messages#islock) and [merkle block](https://dashcore.readme.io/docs/core-ref-p2p-network-data-messages#merkleblock) |

## Platform gRPC Service

| Layer | Endpoint | |
| :-: | - | - |
| 2 | [`broadcastStateTransition`](reference-dapi-endpoints-platform-endpoints#broadcaststatetransition) | Broadcasts the provided State Transition |
| 2 | [`getIdentity`](reference-dapi-endpoints-platform-endpoints#getidentity) | Returns the requested identity |
| 2 | [`getIdentitiesByPublicKeyHashes`](reference-dapi-endpoints-platform-endpoints#getidentitiesbypublickeyhashes) | Returns the identities associated with the provided public key hashes<br>_Added in Dash Platform v0.16_ |
| 2 | [`getIdentityIdsByPublicKeyHashes`](reference-dapi-endpoints-platform-endpoints#getidentityidsbypublickeyhashes) | Returns the identity IDs associated with the provided public key hashes<br>_Added in Dash Platform v0.16_ |
| 2 | [`getDataContract`](reference-dapi-endpoints-platform-endpoints#getdatacontract) | Returns the requested data contract |
| 2 | [`getDocuments`](reference-dapi-endpoints-platform-endpoints#getdocuments) | Returns the requested document(s) |
| 2 | [`waitForStateTransitionResult`](reference-dapi-endpoints-platform-endpoints#waitforstatetransitionresult) | Responds with the state transition hash and either a proof that the state transition was confirmed in a block or an error |
[block:html]
{
  "html": "<div></div>\n<!-- Not implemented yet\n| 1 | [`subscribeTo` `BlockHeaders` `WithChainLocks`](reference-dapi-endpoints-core-grpc-endpoints#section-subscribe-to-block-headers-with-chain-locks) | Returns block headers and associated ChainLock signatures |\n\n-->\n<style></style>"
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "The previous version of documentation can be [viewed here](https://dashplatform.readme.io/v0.18.0/docs/reference-dapi-endpoints)."
}
[/block]