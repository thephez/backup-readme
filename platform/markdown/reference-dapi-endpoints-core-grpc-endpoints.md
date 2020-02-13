[block:callout]
{
  "type": "info",
  "title": "gRPC Overview",
  "body": "Please refer to the [gRPC Overview](reference-dapi-endpoints-grpc-overview) for details regarding running the examples shown below, encoding/decoding the request/response data, and clients available for several languages."
}
[/block]
# --- IMPORTANT NOTE ---
[block:callout]
{
  "type": "warning",
  "body": "These endpoints will be **unavailable on Evonet** until its nodes are running an updated version of dapi-grpc necessary to support these feature."
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
      "code": "grpcurl -plaintext -proto protos/core.proto \\\n  evonet.thephez.com:3010 \\\n  org.dash.platform.dapi.v0.Core/getStatus",
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
      "code": "",
      "language": "shell",
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
      "code": "grpcurl -plaintext -proto protos/core.proto \\\n  -d '{\n    \"height\":1\n    }' \\\n  evonet.thephez.com:3010 \\\n  org.dash.platform.dapi.v0.Core/getBlock",
      "language": "shell",
      "name": "Request (gRPCurl)"
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
| `allow_high_fees` | Boolean | No |  |
| `bypass_limits` | Boolean | No |  |

### Example Request and Response

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
      "code": "grpcurl -plaintext -proto protos/core.proto \\\n  -d '{\n    \"id\":\"4cdc8a08e907b2fe4f33d47c0d9c8e17a2a0c60f1b8eea51fb754d02458d7cd5\"\n    }' \\\n  evonet.thephez.com:3010 \\\n  org.dash.platform.dapi.v0.Core/getTransaction",
      "language": "shell",
      "name": "Request (gRPCurl)"
    }
  ]
}
[/block]
## subscribeToBlockHeadersWithChainLocks

**Returns**: Block headers and associated ChainLock signatures
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| __One of the following:__ | | | |
| `from_block_hash` | Bytes | No | Return records beginning with the block hash provided |
| `from_block_height` | Integer | No | Return records beginning with the block height provided |
| ---------- | | | |
| `count` | Integer | No |  |

### Example Request and Response
[block:code]
{
  "codes": [
    {
      "code": "grpcurl -plaintext -proto protos/core.proto \\\n  -d '{\n    \"from_block_height\":1\n    }' \\\n  evonet.thephez.com:3010 \\\n  org.dash.platform.dapi.v0.Core/subscribeToBlockHeadersWithChainLocks",
      "language": "shell",
      "name": "Request (gRPCurl)"
    }
  ]
}
[/block]
# Code Reference

Implementation details related to the information on this page can be found in:
- The [DAPI repository]((https://github.com/dashevo/dapi) `lib/rpcServer/commands/platform` folder
- The [DAPI-gRPC repository](https://github.com/dashevo/dapi-grpc) `protos` folder