[block:callout]
{
  "type": "info",
  "title": "gRPC Overview",
  "body": "Please refer to the [gRPC Overview](reference-dapi-endpoints-grpc-overview) for details regarding running the examples shown below, encoding/decoding the request/response data, and clients available for several languages."
}
[/block]
# Endpoint Details

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
    },
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\n\nconst client = new DAPIClient({\n  seeds: [{\n    service: 'seed-1.evonet.networks.dash.org',\n    port: 3000\n  }],\n});\n\nclient.getStatus().then((response) => {\n  console.log(response);\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"coreVersion\": 150000,\n  \"protocolVersion\": 70216,\n  \"blocks\": 6056,\n  \"connections\": 62,\n  \"difficulty\": 0.001477193138737096,\n  \"relayFee\": 1e-05,\n  \"network\": \"testnet\"\n}\n",
      "language": "json",
      "name": "Response"
    },
    {
      "code": "{\n  coreVersion: 150000,\n  protocolVersion: 70216,\n  blocks: 6056,\n  timeOffset: 0,\n  connections: 65,\n  proxy: '',\n  difficulty: 0.001477193138737096,\n  testnet: false,\n  relayFee: 0.00001,\n  errors: '',\n  network: 'testnet'\n}",
      "language": "json",
      "name": "Response (dapi-client)"
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
      "code": "{\n  \"block\": \"BAAAAC498j7sXNaobt1QlTkCjiw6PcBTFeso8rqkMhjKCAAAGIrsfCKw44jdFcMK7FOSYlv2BLmgl0VVcKFTyQDdWgS6loBU//9/IAEAAAABAQAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP////8PUQ1kZXZuZXQtZXZvbmV0/////wEA8gUqAQAAAAFqAAAAAA==\"\n}\n",
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
      "code": "grpcurl -plaintext -proto protos/core.proto \\\n  -d '{\n    \"transaction\": \"03000000014f83880b387e1d4a639f8dd59083ab68f464516a060e4725cc1530a0ee2c3d41000000006b483045022100e7ea589971130f6221ec129b66696ecdd359576b7421e8ee5ab0c7e8c4dc3c460220075c25bd0384148de5636af8bc16f4cc271156d20a3eae26e6b590bfc84033d2012103a65caff6ca4c0415a3ac182dfc2a6d3a4dceb98e8b831e71501df38aa156f2c1ffffffff02204e0000000000001976a91409cf4b155dd5ca22979c1390df14aaaa1009bbee88ac44701a3d050000001976a91416b93a3b9168a20605cc3cda62f6135a3baa531a88ac00000000\"\n    }' \\\n  seed.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Core/sendTransaction",
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
      "code": "0f8409a5239150bc9a12c2d3b9a430dcc515ef562906a46e2bfb3ba418d8c9e3",
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
      "code": "grpcurl -plaintext -proto protos/core.proto \\\n  -d '{\n    \"id\":\"0f8409a5239150bc9a12c2d3b9a430dcc515ef562906a46e2bfb3ba418d8c9e3\"\n    }' \\\n  seed.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Core/getTransaction",
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
      "code": "{\n  \"transaction\": \"AwAAAAFPg4gLOH4dSmOfjdWQg6to9GRRagYORyXMFTCg7iw9QQAAAABrSDBFAiEA5+pYmXETD2Ih7BKbZmluzdNZV2t0IejuWrDH6MTcPEYCIAdcJb0DhBSN5WNq+LwW9MwnEVbSCj6uJua1kL/IQDPSASEDplyv9spMBBWjrBgt/CptOk3OuY6Lgx5xUB3ziqFW8sH/////AiBOAAAAAAAAGXapFAnPSxVd1coil5wTkN8UqqoQCbvuiKxEcBo9BQAAABl2qRQWuTo7kWiiBgXMPNpi9hNaO6pTGoisAAAAAA==\"\n}\n\n",
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