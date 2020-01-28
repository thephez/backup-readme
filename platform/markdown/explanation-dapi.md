# Overview

Historically, nodes in most cryptocurrency networks communicated with each other, and the outside world, according to a peer-to-peer (P2P) protocol. The use of P2P protocols presented some downsides for developers, namely, network resources were difficult to access without specialized knowledge or trusted third-party services. To overcome these obstacles, the Dash decentralized API (DAPI) uses Dash's robust masternode infrastructure to provide an HTTP API for accessing the network. DAPI supports both layer 1 (Core blockchain) and layer 2 (Dash Platform) functionality so all developers can interact with Dash via a single interface.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9fc50c7-DAPI_Overview.png",
        "DAPI Overview.png",
        689,
        174,
        "#e6e8e8"
      ],
      "caption": "DAPI Overview"
    }
  ]
}
[/block]
# Endpoint Overview

DAPI currently provides 2 types of endpoints: [JSON-RPC](https://www.jsonrpc.org/) and [gRPC](https://grpc.io/docs/guides/). The JSON-RPC endpoints expose some layer 1 information while the gRPC endpoints support layer 2 as well as streaming of events related to blocks and transactions/transitions.

## JSON-RPC Endpoints

| Layer | Endpoint | Description |
| :-: | - | - |
| 1 | `getBestBlockHash` | Returns block hash of the chaintip |
| 1 | `getBlockHash` | Returns block hash of the requested block |
| 1 | `getMnListDiff` | Returns masternode list diff for the provided block hashes |
| 1 | `getUTXO` | Returns unspent transaction outputs for a given address |

## gRPC Endpoints

| Layer | Endpoint | |
| :-: | - | - |
| 1 | `getBlock` | Returns information for the requested block |
| 1 | `getEstimatedTransactionFee` | Returns estimate of required transaction fee |
| 1 | `getStatus` | Returns blockchain status information |
| 1 | `getTransaction` | Returns details for the requested transaction |
| 1 | `sendTransaction` | Submits the provided transaction |
| 1 | `subscribeTo` `TransactionsWithProofs` | Returns transactions matching the provided bloom filter along with the associated [`islock` message](https://dashcore.readme.io/docs/core-ref-p2p-network-instantsend-messages#section-islock) and [merkle block](https://dashcore.readme.io/docs/core-ref-p2p-network-data-messages#section-merkle-block) |
| 2 | `applyStateTransition` | Applies the provided State Transition |
| 2 | `getIdentity` | Returns the requested identity |
| 2 | `getDataContract` | Returns the requested data contract |
| 2 | `getDocuments` | Returns the requested document(s) |
[block:html]
{
  "html": "<div></div>\n<!--\n - Dependent on [DP-2129](https://dashpay.atlassian.net/browse/DP-2129?atlOrigin=eyJpIjoiNzM1YjQzZDliMGI4NDYzMGI5ZDYyNGIyNDg1OWE0ZTYiLCJwIjoiaiJ9), [DP-2130](https://dashpay.atlassian.net/browse/DP-2130?atlOrigin=eyJpIjoiYTYzZmE0NzAzNzhmNDhkOThlNjM4ZDg4NTliZDMzNTYiLCJwIjoiaiJ9), and [DP-2157](https://dashpay.atlassian.net/browse/DP-2157?atlOrigin=eyJpIjoiOGYyODlmZDA3YzdjNDFlNDhkNGY0MTM2NDZmMWI0NDUiLCJwIjoiaiJ9)\n-->\n<style></style>"
}
[/block]