[block:callout]
{
  "type": "info",
  "title": "gRPC Overview",
  "body": "Please refer to the [gRPC Overview](reference-dapi-endpoints-grpc-overview) for details regarding running the examples shown below, encoding/decoding the request/response data, and clients available for several languages."
}
[/block]
# Endpoint Details
[block:callout]
{
  "type": "danger",
  "title": "Work in Progress",
  "body": "This document is an work in progress. Details may be missing or incomplete and are subject to change."
}
[/block]
## getStatus

**Returns**: Status information from the Core chain
**Parameters**: None

### Example Request and Response
[block:code]
{
  "codes": [
    {
      "code": "grpcurl -plaintext -proto protos/core.proto \\\n  seed.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Core/getStatus",
      "language": "shell",
      "name": "Request (gRPCurl)"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"coreVersion\": 150000,\n  \"protocolVersion\": 70216,\n  \"blocks\": 9380,\n  \"connections\": 18,\n  \"difficulty\": 0.00187190486593008,\n  \"relayFee\": 1e-05,\n  \"network\": \"testnet\"\n}\n",
      "language": "json",
      "name": "Response"
    }
  ]
}
[/block]
## getBlock

**Returns**: A raw block
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| __One of the following:__ | | | |
| `hash` | Bytes | No | Return the block matching the block hash provided |
| `height` | Integer | No | Return the block matching the block height provided |

### Example Request and Response
[block:code]
{
  "codes": [
    {
      "code": "grpcurl -plaintext -proto protos/core.proto \\\n  -d '{\n    \"height\":1\n    }' \\\n  seed.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Core/getBlock",
      "language": "shell",
      "name": "Request (gRPCurl)"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "**Note:** The response block data is Base64 encoded",
  "title": "Block Encoding"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"block\": \"BAAAAC498j7sXNaobt1QlTkCjiw6PcBTFeso8rqkMhjKCAAAt6gicOI3kagmy2RcaqcAJ9AdzNceCiW3FlG1q/C6woG6loBU//9/IAAAAAABAQAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP////8RUQ9kZXZuZXQtZXZvbmV0LTL/////AQDyBSoBAAAAAWoAAAAA\"\n}\n",
      "language": "json",
      "name": "Response"
    }
  ]
}
[/block]
## sendTransaction

**Returns**: The transaction id (TXID) if successful
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `transaction` | Bytes | Yes | A raw transaction |
| `allow_high_fees` | Boolean | No | Enables bypassing the high fee check |
| `bypass_limits` | Boolean | No |  |

### Example Request and Response
[block:code]
{
  "codes": [
    {
      "code": "grpcurl -plaintext -proto protos/core.proto \\\n  -d '{\n    \"transaction\": \"020000000123c52118bfc5da0222a569d379ce3e3a9ca18976175785fd45b3f8990341768b000000006b483045022100a3952306ccb38e1eb22d9956ab40744b79e3072621e634e19225ad8a15603e3102201a3724cb9a8216e78139793c953245b0890c207e13af86bb02735f50a5bccad9012103439cfc2b5fab7fe05c0fbf8fa9217707a5bf5badb7c7e6db05bd0fb1231c5c8bfeffffff0200e1f505000000001976a91468b39aad690ffb710b4ba522d742670b763b501988ac1ec34f95010000001976a91445ada709129f7b6381559c8a16f1ec83c0b3ca8c88acb4240000\"\n    }' \\\n  seed.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Core/sendTransaction",
      "language": "shell",
      "name": "Request (gRPCurl)"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "d4b4617e1fbd70f005495c831ac6ab4b9370f3f464dd6d5604386a5154ae9f72",
      "language": "text",
      "name": "Response"
    }
  ]
}
[/block]
## getTransaction

**Returns**: A raw transaction
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `id` | String | Yes | A transaction id (TXID) |

### Example Request and Response
[block:code]
{
  "codes": [
    {
      "code": "grpcurl -plaintext -proto protos/core.proto \\\n  -d '{\n    \"id\":\"81c2baf0abb55116b7250a1ed7cc1dd02700a76a5c64cb26a89137e27022a8b7\"\n    }' \\\n  seed.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Core/getTransaction",
      "language": "shell",
      "name": "Request (gRPCurl)"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "**Note:** The response transaction data is Base64 encoded",
  "title": "Transaction Encoding"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"transaction\": \"AQAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP////8RUQ9kZXZuZXQtZXZvbmV0LTL/////AQDyBSoBAAAAAWoAAAAA\"\n}\n",
      "language": "json",
      "name": "Response"
    }
  ]
}
[/block]

[block:html]
{
  "html": "<div></div>\n<!-- Not yet implemented in DAPI-client\n## subscribeToBlockHeadersWithChainLocks \n\n**Returns**: Block headers and associated ChainLock signatures\n**Parameters**:\n\n| Name | Type | Required | Description |\n| - | - | - | - |\n| __One of the following:__ | | | |\n| `from_block_hash` | Bytes | No | Return records beginning with the block hash provided |\n| `from_block_height` | Integer | No | Return records beginning with the block height provided |\n| ---------- | | | |\n| `count` | Integer | No |  |\n\n### Example Request and Response\ngrpcurl -plaintext -proto protos/core.proto \\\n  -d '{\n    \"from_block_height\":1\n    }' \\\n  seed.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Core/subscribeToBlockHeadersWithChainLocks\n\n-->\n<style></style>"
}
[/block]
# Code Reference

Implementation details related to the information on this page can be found in:
- The [DAPI repository]((https://github.com/dashevo/dapi) `lib/grpcServer/handlers/core` folder
- The [DAPI-gRPC repository](https://github.com/dashevo/dapi-grpc) `protos` folder