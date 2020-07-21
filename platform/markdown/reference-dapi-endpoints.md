# Endpoint Overview

[DAPI](explanation-dapi) currently provides 2 types of endpoints: [JSON-RPC](https://www.jsonrpc.org/) and [gRPC](https://grpc.io/docs/guides/). The JSON-RPC endpoints expose some layer 1 information while the gRPC endpoints support layer 2 as well as streaming of events related to blocks and transactions/transitions.

## JSON-RPC Endpoints
[block:callout]
{
  "type": "info",
  "title": "gRPC Migration",
  "body": "The remaining JSON-RPC endpoints will be migrated to gRPC in a future release"
}
[/block]
| Layer | Endpoint | Description |
| :-: | - | - |
| 1 | [`getAddressSummary`](reference-dapi-endpoints-json-rpc-endpoints#getaddresssummary) | Returns summary information for provided address(es) |
| 1 | [`getBestBlockHash`](reference-dapi-endpoints-json-rpc-endpoints#getbestblockhash) | Returns block hash of the chaintip |
| 1 | [`getBlockHash`](reference-dapi-endpoints-json-rpc-endpoints#getblockhash) | Returns block hash of the requested block |
| 1 | [`getMnListDiff`](reference-dapi-endpoints-json-rpc-endpoints#getmnlistdiff) | Returns masternode list diff for the provided block hashes |
| 1 | [`getUTXO`](reference-dapi-endpoints-json-rpc-endpoints#getutxo) | Returns unspent transaction outputs for a given address |

## gRPC Endpoints

| Layer | Endpoint | |
| :-: | - | - |
| 1 | [`getBlock`](reference-dapi-endpoints-core-grpc-endpoints#getblock) | Returns information for the requested block |
| 1 | [`getStatus`](reference-dapi-endpoints-core-grpc-endpoints#getstatus) | Returns blockchain status information |
| 1 | [`getTransaction`](reference-dapi-endpoints-core-grpc-endpoints#gettransaction) | Returns details for the requested transaction |
| 1 | [`sendTransaction`](reference-dapi-endpoints-core-grpc-endpoints#sendtransaction) | Submits the provided transaction |
| 1 | [`subscribeTo` `TransactionsWithProofs`](reference-dapi-endpoints-transaction-streaming-endpoints#subscribetotransactionswithproofs) | Returns transactions matching the provided bloom filter along with the associated [`islock` message](https://dashcore.readme.io/docs/core-ref-p2p-network-instantsend-messages#islock) and [merkle block](https://dashcore.readme.io/docs/core-ref-p2p-network-data-messages#merkleblock) |
| 2 | [`applyStateTransition`](reference-dapi-endpoints-platform-endpoints#applystatetransition) | Applies the provided State Transition |
| 2 | [`getIdentity`](reference-dapi-endpoints-platform-endpoints#getidentity) | Returns the requested identity |
| 2 | [`getIdentityByFirstPublicKey`](reference-dapi-endpoints-platform-endpoints#getidentitybyfirstpublickey) | Returns the identity associated with the provided public key |
| 2 | [`getIdentityIdByFirstPublicKey`](reference-dapi-endpoints-platform-endpoints#getidentityidbyfirstpublickey) | Returns the identity ID associated with the provided public key |
| 2 | [`getDataContract`](reference-dapi-endpoints-platform-endpoints#getdatacontract) | Returns the requested data contract |
| 2 | [`getDocuments`](reference-dapi-endpoints-platform-endpoints#getdocuments) | Returns the requested document(s) |
[block:html]
{
  "html": "<div></div>\n<!-- Not implemented yet\n| 1 | [`subscribeTo` `BlockHeaders` `WithChainLocks`](reference-dapi-endpoints-core-grpc-endpoints#section-subscribe-to-block-headers-with-chain-locks) | Returns block headers and associated ChainLock signatures |\n\n-->\n<style></style>"
}
[/block]