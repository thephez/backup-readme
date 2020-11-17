[block:callout]
{
  "type": "info",
  "title": "gRPC Overview",
  "body": "Please refer to the [gRPC Overview](reference-dapi-endpoints-grpc-overview) for details regarding running the examples shown below, encoding/decoding the request/response data, and clients available for several languages."
}
[/block]
# Endpoint Details

## broadcastTransaction
[block:callout]
{
  "type": "success",
  "body": "Previously named `sendTransaction`",
  "title": "New in Dash Platform v0.15.0"
}
[/block]
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
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst { Transaction } = require('@dashevo/dashcore-lib');\n\nconst client = new DAPIClient({\n  seeds: [{\n    service: 'seed-1.evonet.networks.dash.org',\n    port: 3000\n  }],\n});\n\nconst tx = Transaction('02000000012a853bf4bf4965540210b8a1ff11cde1bf17c47b37b1bf97633d81f642136d52010000006a47304402207a6f967119ea7432414a44f26b75c62053626ac6c918781c22e0eca8be6ffd6002201967858618c139a57e417a02c253888f9be79a7d4ca2381054be93141fd31c6e0121029c0d10e04cb436f76fc8223e1c7a37601f1d097a059674a45391f19b642a8e17feffffff0200e1f505000000001976a914f862c9d89384b14139d79b485dde663a5a02268f88ac9c3260f6000000001976a91417b2e8501e9f956575102c5ba87d15de9dcd5bb088ac0f180000');\n\nclient.core.broadcastTransaction(tx.toBuffer())\n  .then((response) => console.log(response));",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    CorePromiseClient,\n  },\n} = require('@dashevo/dapi-grpc');\nconst { Transaction } = require('@dashevo/dashcore-lib');\n\nconst corePromiseClient = new CorePromiseClient('http://seed.evonet.networks.dash.org:3010');\n\nconst tx = Transaction('02000000012a853bf4bf4965540210b8a1ff11cde1bf17c47b37b1bf97633d81f642136d52010000006a47304402207a6f967119ea7432414a44f26b75c62053626ac6c918781c22e0eca8be6ffd6002201967858618c139a57e417a02c253888f9be79a7d4ca2381054be93141fd31c6e0121029c0d10e04cb436f76fc8223e1c7a37601f1d097a059674a45391f19b642a8e17feffffff0200e1f505000000001976a914f862c9d89384b14139d79b485dde663a5a02268f88ac9c3260f6000000001976a91417b2e8501e9f956575102c5ba87d15de9dcd5bb088ac0f180000');\n\ncorePromiseClient.client.broadcastTransaction({ transaction: tx.toBuffer() })\n  .then((response) => console.log(response));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  transactionId: 'a519f41be5de67a917094346e85111d571e198dbd2646c9170358c7439fc2b10'\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    }
  ]
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
      "code": "const DAPIClient = require('@dashevo/dapi-client');\n\nconst client = new DAPIClient({\n  seeds: [{\n    service: 'seed-1.evonet.networks.dash.org',\n    port: 3000\n  }],\n});\n\nclient.core.getStatus()\n  .then((response) => console.log(response));",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    GetStatusRequest,\n    CorePromiseClient,\n  },\n} = require('@dashevo/dapi-grpc');\n\nconst corePromiseClient = new CorePromiseClient('http://seed.evonet.networks.dash.org:3010');\n\ncorePromiseClient.client.getStatus(new GetStatusRequest())\n  .then((response) => console.log(response));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "grpcurl -plaintext -proto protos/core/v0/core.proto \\\n  seed.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Core/getStatus",
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
      "code": "{\n  coreVersion: 160001,\n  protocolVersion: 70218,\n  blocks: 10761,\n  timeOffset: 0,\n  connections: 15,\n  proxy: '',\n  difficulty: 0.00139064070266453,\n  testnet: false,\n  relayFee: 0.00001,\n  errors: '',\n  network: 'testnet'\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"coreVersion\": 160001,\n  \"protocolVersion\": 70218,\n  \"blocks\": 6154,\n  \"connections\": 25,\n  \"difficulty\": 0.00176467490021852,\n  \"relayFee\": 1e-05,\n  \"network\": \"testnet\"\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
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
      "code": "const DAPIClient = require('@dashevo/dapi-client');\n\nconst client = new DAPIClient({\n  seeds: [{\n    service: 'seed-1.evonet.networks.dash.org',\n    port: 3000\n  }],\n});\n\nclient.core.getBlockByHeight(1)\n  .then((response) => console.log(response.toString('hex')));",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    CorePromiseClient,\n  },\n} = require('@dashevo/dapi-grpc');\n\nconst corePromiseClient = new CorePromiseClient('http://seed.evonet.networks.dash.org:3010');\n\ncorePromiseClient.client.getBlock({ height: 1 })\n  .then((response) => console.log(response.block.toString('hex')));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "const {\n  v0: {\n    CorePromiseClient,\n  },\n} = require('@dashevo/dapi-grpc');\n\nconst corePromiseClient = new CorePromiseClient('http://seed.evonet.networks.dash.org:3010');\n\ncorePromiseClient.client.getBlock({\n  hash: '5dd32f6c03b041c5e701323ccc92e62ee0b40ac19d214ad721ae5321f1dce38c',\n}).then((response) => {\n  console.log(response.block.toString('hex'));\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "grpcurl -plaintext -proto protos/core/v0/core.proto \\\n  -d '{\n    \"height\":1\n    }' \\\n  seed.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Core/getBlock",
      "language": "shell",
      "name": "Request (gRPCurl)"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "**Note:** The gRPCurl response block data is Base64 encoded",
  "title": "Block Encoding"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "040000002e3df23eec5cd6a86edd509539028e2c3a3dc05315eb28f2baa43218ca080000c4b1069b3cf0b36e57a33e4f85c3b5b7b4470b9fda358ae8e04499473c51e205ba968054ffff7f20030000000101000000010000000000000000000000000000000000000000000000000000000000000000ffffffff10510e6465766e65742d70616c696e6b61ffffffff0100f2052a01000000016a00000000",
      "language": "text",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"block\": \"BAAAAC498j7sXNaobt1QlTkCjiw6PcBTFeso8rqkMhjKCAAAO7rZNoARgNKNj0JldKtFvqTpbdeiOJQXCiDy09a92HO6loBU//9/IAIAAAABAQAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP////8RUQ9kZXZuZXQtZXZvbmV0LTj/////AQDyBSoBAAAAAWoAAAAA\"\n}\n",
      "language": "json",
      "name": "Response (gRPCurl)"
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
      "code": "const {\n  v0: {\n    CorePromiseClient,\n  },\n} = require('@dashevo/dapi-grpc');\n\nconst corePromiseClient = new CorePromiseClient('http://seed.evonet.networks.dash.org:3010');\n\nconst txid = 'a519f41be5de67a917094346e85111d571e198dbd2646c9170358c7439fc2b10';\n\ncorePromiseClient.client.getTransaction({ id: txid })\n  .then((response) => console.log(response.transaction.toString('hex')));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\n\nconst client = new DAPIClient({\n  seeds: [{\n    service: 'seed-1.evonet.networks.dash.org',\n    port: 3000\n  }],\n});\n\nconst txid = 'a519f41be5de67a917094346e85111d571e198dbd2646c9170358c7439fc2b10';\nclient.core.getTransaction(txid)\n  .then((response) => console.log(response.toString('hex')));",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "grpcurl -plaintext -proto protos/core/v0/core.proto \\\n  -d '{\n    \"id\":\"a519f41be5de67a917094346e85111d571e198dbd2646c9170358c7439fc2b10\"\n    }' \\\n  seed.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Core/getTransaction",
      "language": "shell",
      "name": "Request (gRPCurl)"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "**Note:** The gRPCurl response transaction data is Base64 encoded",
  "title": "Transaction Encoding"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "02000000012a853bf4bf4965540210b8a1ff11cde1bf17c47b37b1bf97633d81f642136d52010000006a47304402207a6f967119ea7432414a44f26b75c62053626ac6c918781c22e0eca8be6ffd6002201967858618c139a57e417a02c253888f9be79a7d4ca2381054be93141fd31c6e0121029c0d10e04cb436f76fc8223e1c7a37601f1d097a059674a45391f19b642a8e17feffffff0200e1f505000000001976a914f862c9d89384b14139d79b485dde663a5a02268f88ac9c3260f6000000001976a91417b2e8501e9f956575102c5ba87d15de9dcd5bb088ac0f180000",
      "language": "text",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"transaction\": \"AgAAAAEqhTv0v0llVAIQuKH/Ec3hvxfEezexv5djPYH2QhNtUgEAAABqRzBEAiB6b5ZxGep0MkFKRPJrdcYgU2JqxskYeBwi4Oyovm/9YAIgGWeFhhjBOaV+QXoCwlOIj5vnmn1MojgQVL6TFB/THG4BIQKcDRDgTLQ292/IIj4cejdgHx0JegWWdKRTkfGbZCqOF/7///8CAOH1BQAAAAAZdqkU+GLJ2JOEsUE515tIXd5mOloCJo+IrJwyYPYAAAAAGXapFBey6FAen5VldRAsW6h9Fd6dzVuwiKwPGAAA\"\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    }
  ]
}
[/block]

[block:html]
{
  "html": "<div></div>\n<!-- Not yet implemented in DAPI-client\n## subscribeToBlockHeadersWithChainLocks \n\n**Returns**: Block headers and associated ChainLock signatures\n**Parameters**:\n\n| Name | Type | Required | Description |\n| - | - | - | - |\n| __One of the following:__ | | | |\n| `from_block_hash` | Bytes | No | Return records beginning with the block hash provided |\n| `from_block_height` | Integer | No | Return records beginning with the block height provided |\n| ---------- | | | |\n| `count` | Integer | No |  |\n\n### Example Request and Response\ngrpcurl -plaintext -proto protos/core.proto \\\n  -d '{\n    \"from_block_height\":1\n    }' \\\n  seed.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Core/subscribeToBlockHeadersWithChainLocks\n\n-->\n<style></style>"
}
[/block]

## subscribeToTransactionsWithProofs
[block:callout]
{
  "type": "success",
  "title": "Updated in Dash Platform v0.15.0",
  "body": "This endpoint was moved into the Core gRPC service (from the Transaction Filter Stream service) to consolidate all Core-related endpoints together."
}
[/block]
**Returns**: streams the requested transaction information
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `bloom_filter.v_data` | Bytes | Yes | The filter itself is simply a bit field of arbitrary byte-aligned size. The maximum size is 36,000 bytes |
| `bloom_filter.n_hash_funcs` | Integer | Yes | The number of hash functions to use in this filter. The maximum value allowed in this field is 50 |
| `bloom_filter.n_tweak` | Integer | Yes | A random value to add to the seed value in the hash function used by the bloom filter |
| `bloom_filter.n_flags` | Integer | Yes | A set of flags that control how matched items are added to the filter |
| ---------- | | | |
| __One of the following:__ | | | |
| `from_block_hash` | Bytes | No | Return records beginning with the block hash provided |
| `from_block_height` | Integer | No | Return records beginning with the block height provided |
| ---------- | | | |
| `count` | Integer | No | Number of blocks to sync. If set to 0 syncing is continuously sends new data as well (default: 0) |
| `send_transaction_hashes` * | Boolean | No |  |

** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "grpcurl -proto protos/core/v0/core.proto -plaintext \\\n  -d '{\n  \"from_block_height\": 1,\n  \"count\": 1,\n  \"bloom_filter\": {\n    \"n_hash_funcs\": 11,\n    \"v_data\": \"\",\n    \"n_tweak\": 0,\n    \"n_flags\": 0\n  }\n}' \\\n  seed.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Core/subscribeToTransactionsWithProofs",
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
      "code": "{\n  \"rawTransactions\": {\n    \"transactions\": [\n      \"AQAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP////8RUQ9kZXZuZXQtZXZvbmV0LTT/////AQDyBSoBAAAAAWoAAAAA\"\n    ]\n  }\n}\n{\n  \"rawMerkleBlock\": \"BAAAAC498j7sXNaobt1QlTkCjiw6PcBTFeso8rqkMhjKCAAAD0Oosr0gDJvAxHZ2Y+4dtsmO+XenCUNNoIX0W06eoW26loBU//9/IAAAAAABAAAAAQ9DqLK9IAybwMR2dmPuHbbJjvl3pwlDTaCF9FtOnqFtAQE=\"\n}\n",
      "language": "json",
      "name": "Response"
    }
  ]
}
[/block]

[block:html]
{
  "html": "<div></div>\n<!--\n\ngrpcurl -proto protos/transactions_filter_stream.proto -plaintext   -d '{\n  \"from_block_height\": 30000,\n  \"count\": 1,\n  \"bloom_filter\": {\n    \"n_hash_funcs\": 11,\n    \"v_data\": \"\",\n    \"n_tweak\": 0,\n    \"n_flags\": 0\n  }\n}'   localhost:2510   org.dash.platform.dapi.v0.TransactionsFilterStream/subscribeToTransactionsWithProofs\n{\n  \"rawTransactions\": {\n    \"transactions\": [\n      \"AwAFAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP////8FAjB1AQj/////AkAjDkMAAAAAGXapFBa5OjuRaKIGBcw82mL2E1o7qlMaiKxAIw5DAAAAABl2qRQWuTo7kWiiBgXMPNpi9hNaO6pTGoisAAAAAEYCADB1AAAPwA0VGVVRQSZSW013DrT+TU+AhULKbLNg+/rtgnzE5lca9JYY2DC/1hyqelAuIkJqqcby0zIroYyfLzuhjNso\"\n    ]\n  }\n}\n{\n  \"rawMerkleBlock\": \"AAAAIIGiClhX7zPY2s2DmwiDdlbUJSUpzBjclOIWcgggAwAAak7QtEqCigCc1+U3+R6ElSI/vQz4mXzn1bADpwg41MvxNjxeBaADHhuWAAACAAAAAi6VQ1ZA+oFPPtKYv7OuzUfdLqZ+ZwzAwpztIn0osooZAzrYFIkcfvpIDK6Mg9FgxH4eOkjvyMwXj6qwEqZCJPYBAw==\"\n}\n{\n  \"rawTransactions\": {\n    \"transactions\": [\n      \"AwAFAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP////8FAjF1AQH/////AkAjDkMAAAAAGXapFBa5OjuRaKIGBcw82mL2E1o7qlMaiKxAIw5DAAAAABl2qRQWuTo7kWiiBgXMPNpi9hNaO6pTGoisAAAAAEYCADF1AAAPwA0VGVVRQSZSW013DrT+TU+AhULKbLNg+/rtgnzE5lca9JYY2DC/1hyqelAuIkJqqcby0zIroYyfLzuhjNso\"\n    ]\n  }\n}\n{\n  \"rawMerkleBlock\": \"AAAAIKnRSfNm6oA5kqly1SG3FJ0a/v3hAh9GrFtyApQ+AQAAx/bTc5j8Ctlx0exLl3Xn+GvymqZBllMPdX5f5TuPsFjkNzxepXsDHoBCAAABAAAAAcf203OY/ArZcdHsS5d15/hr8pqmQZZTD3V+X+U7j7BYAQE=\"\n}\n{\n  \"rawTransactions\": {\n    \"transactions\": [\n      \"AwAFAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP////8FAjJ1AQT/////AkAjDkMAAAAAGXapFBa5OjuRaKIGBcw82mL2E1o7qlMaiKxAIw5DAAAAABl2qRQWuTo7kWiiBgXMPNpi9hNaO6pTGoisAAAAAEYCADJ1AAAPwA0VGVVRQSZSW013DrT+TU+AhULKbLNg+/rtgnzE5lca9JYY2DC/1hyqelAuIkJqqcby0zIroYyfLzuhjNso\"\n    ]\n  }\n}\n\n\n-->\n\n<style></style>"
}
[/block]
# Deprecated Endpoints

## sendTransaction
[block:callout]
{
  "type": "danger",
  "body": "Renamed to [`broadcastTransaction`](#broadcasttransaction) in Dash Platform v0.15.0",
  "title": "Deprecated RPC"
}
[/block]


# Code Reference

Implementation details related to the information on this page can be found in:
- The [DAPI repository](https://github.com/dashevo/dapi) `lib/grpcServer/handlers/core` folder
- The [DAPI-gRPC repository](https://github.com/dashevo/dapi-grpc) `protos` folder