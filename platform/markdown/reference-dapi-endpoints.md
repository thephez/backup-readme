[DAPI](explanation-dapi) currently provides 2 types of endpoints: [JSON-RPC](https://www.jsonrpc.org/) and [gRPC](https://grpc.io/docs/guides/). The JSON-RPC endpoints expose some layer 1 information while the gRPC endpoints support layer 2 as well as streaming of events related to blocks and transactions/transitions.

# JSON-RPC Endpoints
[block:callout]
{
  "type": "info",
  "title": "gRPC Migration",
  "body": "The remaining JSON-RPC endpoints will be migrated to gRPC in a future release"
}
[/block]

[block:callout]
{
  "type": "danger",
  "title": "Deprecated RPCs",
  "body": "The `getAddressSummary` and `getUTXO` RPCs are deprecated and no longer usable as of Dash Platform v0.15.0. \n\nServices that previously used these RPCs should join the [wallet-lib library](https://github.com/dashevo/wallet-lib/pull/119) in switching to using the [`subscribeToTransactionsWithProofs`](reference-dapi-endpoints-transaction-streaming-endpoints#subscribetotransactionswithproofs) gRPC endpoint for similar (and more efficient) functionality."
}
[/block]
| Layer | Endpoint | Description |
| :-: | - | - |
| 1 | `getAddressSummary` | **Deprecated in Dash Platform v0.15.0**<br>Returns summary information for provided address(es) |
| 1 | [`getBestBlockHash`](reference-dapi-endpoints-json-rpc-endpoints#getbestblockhash) | Returns block hash of the chaintip |
| 1 | [`getBlockHash`](reference-dapi-endpoints-json-rpc-endpoints#getblockhash) | Returns block hash of the requested block |
| 1 | [`getMnListDiff`](reference-dapi-endpoints-json-rpc-endpoints#getmnlistdiff) | Returns masternode list diff for the provided block hashes |
| 1 | `getUTXO` | **Deprecated in Dash Platform v0.15.0**<br>Returns unspent transaction outputs for a given address |

# gRPC Endpoints

## Core gRPC Service

| Layer | Endpoint | |
| :-: | - | - |
| 1 | [`broadcastTransaction`](reference-dapi-endpoints-core-grpc-endpoints#broadcasttransaction) | Broadcasts the provided transaction |
| 1 | [`getBlock`](reference-dapi-endpoints-core-grpc-endpoints#getblock) | Returns information for the requested block |
| 1 | [`getStatus`](reference-dapi-endpoints-core-grpc-endpoints#getstatus) | Returns blockchain status information |
| 1 | [`getTransaction`](reference-dapi-endpoints-core-grpc-endpoints#gettransaction) | Returns details for the requested transaction |
| 1 | `sendTransaction` | Renamed to [`broadcastTransaction`](reference-dapi-endpoints-core-grpc-endpoints#broadcasttransaction) in Dash Platform v0.15.0 |
| 1 | [`subscribeTo` `TransactionsWithProofs`](reference-dapi-endpoints-core-grpc-endpoints#subscribetotransactionswithproofs) | Returns transactions matching the provided bloom filter along with the associated [`islock` message](https://dashcore.readme.io/docs/core-ref-p2p-network-instantsend-messages#islock) and [merkle block](https://dashcore.readme.io/docs/core-ref-p2p-network-data-messages#merkleblock) |

## Platform gRPC Service

[block:callout]
{
  "type": "danger",
  "body": "The `getIdentityByFirstPublicKey` and `getIdentityIdByFirstPublicKey` RPCs are deprecated and no longer usable as of Dash Platform v0.16.0. They were replaced by [`getIdentitiesByPublicKeyHashes`](reference-dapi-endpoints-platform-endpoints#getidentitiesbypublickeyhashes) and [`getIdentityIdsByPublicKeyHashes`](reference-dapi-endpoints-platform-endpoints#getidentityidsbypublickeyhashes).",
  "title": "Deprecated RPCs"
}
[/block]
| Layer | Endpoint | |
| :-: | - | - |
| 2 | `applyStateTransition` | Renamed to [`broadcastStateTransition`](reference-dapi-endpoints-platform-endpoints#broadcaststatetransition) in Dash Platform v0.15.0 |
| 2 | [`broadcastStateTransition`](reference-dapi-endpoints-platform-endpoints#broadcaststatetransition) | Broadcasts the provided State Transition |
| 2 | [`getIdentity`](reference-dapi-endpoints-platform-endpoints#getidentity) | Returns the requested identity |
| 2 | `getIdentityByFirstPublicKey` | Returns the identity associated with the provided public key (replaced by [`getIdentitiesByPublicKeyHashes`](reference-dapi-endpoints-platform-endpoints#getidentitiesbypublickeyhashes)) |
| 2 | `getIdentityIdByFirstPublicKey` | Returns the identity ID associated with the provided public key (replaced by  [`getIdentityIdsByPublicKeyHashes`](reference-dapi-endpoints-platform-endpoints#getidentityidsbypublickeyhashes)) |
| 2 | [`getIdentitiesByPublicKeyHashes`](reference-dapi-endpoints-platform-endpoints#getidentitiesbypublickeyhashes) | Returns the identities associated with the provided public key hashes<br>_Added in Dash Platform v0.16_ |
| 2 | [`getIdentityIdsByPublicKeyHashes`](reference-dapi-endpoints-platform-endpoints#getidentityidsbypublickeyhashes) | Returns the identity IDs associated with the provided public key hashes<br>_Added in Dash Platform v0.16_ |
| 2 | [`getDataContract`](reference-dapi-endpoints-platform-endpoints#getdatacontract) | Returns the requested data contract |
| 2 | [`getDocuments`](reference-dapi-endpoints-platform-endpoints#getdocuments) | Returns the requested document(s) |
[block:html]
{
  "html": "<div></div>\n<!-- Not implemented yet\n| 1 | [`subscribeTo` `BlockHeaders` `WithChainLocks`](reference-dapi-endpoints-core-grpc-endpoints#section-subscribe-to-block-headers-with-chain-locks) | Returns block headers and associated ChainLock signatures |\n\n-->\n<style></style>"
}
[/block]