[block:callout]
{
  "type": "info",
  "title": "gRPC Overview",
  "body": "Please refer to the [gRPC Overview](reference-dapi-endpoints-grpc-overview) for details regarding running the examples shown below, encoding/decoding the request/response data, and clients available for several languages."
}
[/block]
# Endpoint Details

## broadcastTransaction

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
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst { Transaction } = require('@dashevo/dashcore-lib');\n\nconst client = new DAPIClient({\n  seeds: [{\n    host: 'seed-1.testnet.networks.dash.org',\n    port: 3000\n  }],\n});\n\nconst tx = Transaction('02000000022fd1c4583099109524b8216d712373bd837d24a502414fcadd8ae94753c3d87e010000006a47304402202cbdc560898ad389005fbe231fb345da503d838cfadab738a7d2f57bdd7ff77c02206e02b9f05c3dfb380d158949407372f26fa8ecc66956297792509c2f700723d1012103422fa857d5049000c22c3188e84557da5b783c2ef54b83a76a2933a0564c22dafeffffff07e987f3bb114c4370b937915e980657e2706135e21fbd8972a5534c804d5495000000006a473044022041a69c058035a2a8c88715c018efcb77a9ee3a08b72fd24afe8591364cee8dc002203026f115ac9c7206a985f71422ac38d451bde092d708bfb81ef35b2968f4ee34012102f0ce58f50515d04d4ff01a550a4d3246fbdc9d27031ef7d883e845b6b41f0e4efeffffff0269440f00000000001976a91465f6a3d634ba58247825c6fd55174ca72fdcdbd988ac00e1f505000000001976a9144139b147b5cef5fef5bcdb02fcdf55e426f74dbb88ac4d5b0600');\n\nclient.core.broadcastTransaction(tx.toBuffer())\n  .then((response) => console.log(response));",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    CorePromiseClient,\n  },\n} = require('@dashevo/dapi-grpc');\nconst { Transaction } = require('@dashevo/dashcore-lib');\n\nconst corePromiseClient = new CorePromiseClient('http://seed-1.testnet.networks.dash.org:3010');\n\nconst tx = Transaction('02000000022fd1c4583099109524b8216d712373bd837d24a502414fcadd8ae94753c3d87e010000006a47304402202cbdc560898ad389005fbe231fb345da503d838cfadab738a7d2f57bdd7ff77c02206e02b9f05c3dfb380d158949407372f26fa8ecc66956297792509c2f700723d1012103422fa857d5049000c22c3188e84557da5b783c2ef54b83a76a2933a0564c22dafeffffff07e987f3bb114c4370b937915e980657e2706135e21fbd8972a5534c804d5495000000006a473044022041a69c058035a2a8c88715c018efcb77a9ee3a08b72fd24afe8591364cee8dc002203026f115ac9c7206a985f71422ac38d451bde092d708bfb81ef35b2968f4ee34012102f0ce58f50515d04d4ff01a550a4d3246fbdc9d27031ef7d883e845b6b41f0e4efeffffff0269440f00000000001976a91465f6a3d634ba58247825c6fd55174ca72fdcdbd988ac00e1f505000000001976a9144139b147b5cef5fef5bcdb02fcdf55e426f74dbb88ac4d5b0600');\n\ncorePromiseClient.client.broadcastTransaction({ transaction: tx.toBuffer() })\n  .then((response) => console.log(response));",
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
      "code": "{\n  transactionId: '552eaf24a60014edcbbb253dbc4dd68766532cab3854b44face051cedcfd578f'\n}",
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
      "code": "const DAPIClient = require('@dashevo/dapi-client');\n\nconst client = new DAPIClient({\n  seeds: [{\n    host: 'seed-1.testnet.networks.dash.org',\n    port: 3000\n  }],\n});\n\nclient.core.getStatus()\n  .then((response) => console.log(response));",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    GetStatusRequest,\n    CorePromiseClient,\n  },\n} = require('@dashevo/dapi-grpc');\n\nconst corePromiseClient = new CorePromiseClient('http://seed-1.testnet.networks.dash.org:3010');\n\ncorePromiseClient.client.getStatus(new GetStatusRequest())\n  .then((response) => console.log(response));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "grpcurl -plaintext -proto protos/core/v0/core.proto \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Core/getStatus",
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
      "code": "{\n  coreVersion: 170000,\n  protocolVersion: 70218,\n  blocks: 416585,\n  timeOffset: 0,\n  connections: 240,\n  proxy: '',\n  difficulty: 0.004320536256772037,\n  testnet: false,\n  relayFee: 0.00001,\n  errors: 'Warning: unknown new rules activated (versionbit 3)',\n  network: 'testnet'\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"coreVersion\": 170000,\n  \"protocolVersion\": 70218,\n  \"blocks\": 416583,\n  \"connections\": 190,\n  \"difficulty\": 0.00430861051502146,\n  \"relayFee\": 1e-05,\n  \"errors\": \"Warning: unknown new rules activated (versionbit 3)\",\n  \"network\": \"testnet\"\n}\n",
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
      "code": "const DAPIClient = require('@dashevo/dapi-client');\n\nconst client = new DAPIClient({\n  seeds: [{\n    host: 'seed-1.testnet.networks.dash.org',\n    port: 3000\n  }],\n});\n\nclient.core.getBlockByHeight(1)\n  .then((response) => console.log(response.toString('hex')));",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    CorePromiseClient,\n  },\n} = require('@dashevo/dapi-grpc');\n\nconst corePromiseClient = new CorePromiseClient('http://seed-1.testnet.networks.dash.org:3010');\n\ncorePromiseClient.client.getBlock({ height: 1 })\n  .then((response) => console.log(response.block.toString('hex')));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "const {\n  v0: {\n    CorePromiseClient,\n  },\n} = require('@dashevo/dapi-grpc');\n\nconst corePromiseClient = new CorePromiseClient('http://seed-1.testnet.networks.dash.org:3010');\n\ncorePromiseClient.client.getBlock({\n  hash: '0000047d24635e347be3aaaeb66c26be94901a2f962feccd4f95090191f208c1',\n}).then((response) => {\n  console.log(response.block.toString('hex'));\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "grpcurl -plaintext -proto protos/core/v0/core.proto \\\n  -d '{\n    \"height\":1\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Core/getBlock",
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
      "code": "020000002cbcf83b62913d56f605c0e581a48872839428c92e5eb76cd7ad94bcaf0b00007f11dcce14075520e8f74cc4ddf092b4e26ebd23b8d8665a1ae5bfc41b58fdb4c3a95e53ffff0f1ef37a00000101000000010000000000000000000000000000000000000000000000000000000000000000ffffffff0a510101062f503253482fffffffff0100743ba40b0000002321020131f38ae3eb0714531dbfc3f45491b4131d1211e3777177636388bb5a74c3e4ac00000000",
      "language": "text",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"block\": \"AgAAACy8+DtikT1W9gXA5YGkiHKDlCjJLl63bNetlLyvCwAAfxHczhQHVSDo90zE3fCStOJuvSO42GZaGuW/xBtY/bTDqV5T//8PHvN6AAABAQAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP////8KUQEBBi9QMlNIL/////8BAHQ7pAsAAAAjIQIBMfOK4+sHFFMdv8P0VJG0Ex0SEeN3cXdjY4i7WnTD5KwAAAAA\"\n}",
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
      "code": "const DAPIClient = require('@dashevo/dapi-client');\n\nconst client = new DAPIClient({\n  seeds: [{\n    host: 'seed-1.testnet.networks.dash.org',\n    port: 3000\n  }],\n});\n\nconst txid = '4004d3f9f1b688f2babb1f98ea48e1472be51e29712f942fc379c6e996cdd308';\nclient.core.getTransaction(txid)\n  .then((response) => console.log(response.toString('hex')));",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    CorePromiseClient,\n  },\n} = require('@dashevo/dapi-grpc');\n\nconst corePromiseClient = new CorePromiseClient('http://seed-1.testnet.networks.dash.org:3010');\n\nconst txid = '4004d3f9f1b688f2babb1f98ea48e1472be51e29712f942fc379c6e996cdd308';\n\ncorePromiseClient.client.getTransaction({ id: txid })\n  .then((response) => console.log(response.transaction.toString('hex')));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "grpcurl -plaintext -proto protos/core/v0/core.proto \\\n  -d '{\n    \"id\":\"4004d3f9f1b688f2babb1f98ea48e1472be51e29712f942fc379c6e996cdd308\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Core/getTransaction",
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
      "code": "03000500010000000000000000000000000000000000000000000000000000000000000000ffffffff0603c25a060109ffffffff02eefccf31000000001976a9147eb25dc5af472d6bf19a877a96f0a707c2c61b7688ac65fbb74a000000001976a9141ec5c66e9789c655ae068d35088b4073345fe0b088ac00000000460200c25a0600df34a6b9c99dbbbf82698fba9f86861f24891e16c0c5e881de034c192220600d5ceaadc37b4c316e2ec7a407f8838060494aa19a403155f95c43275da3edb318",
      "language": "text",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"transaction\": \"AwAFAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP////8GA8JaBgEJ/////wLu/M8xAAAAABl2qRR+sl3Fr0cta/Gah3qW8KcHwsYbdoisZfu3SgAAAAAZdqkUHsXGbpeJxlWuBo01CItAczRf4LCIrAAAAABGAgDCWgYA3zSmucmdu7+CaY+6n4aGHySJHhbAxeiB3gNMGSIgYA1c6q3De0wxbi7HpAf4g4BgSUqhmkAxVflcQyddo+2zGA==\"\n}",
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
      "code": "grpcurl -proto protos/core/v0/core.proto -plaintext \\\n  -d '{\n  \"from_block_height\": 1,\n  \"count\": 1,\n  \"bloom_filter\": {\n    \"n_hash_funcs\": 11,\n    \"v_data\": \"\",\n    \"n_tweak\": 0,\n    \"n_flags\": 0\n  }\n}' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Core/subscribeToTransactionsWithProofs",
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
      "code": "{\n  \"rawTransactions\": {\n    \"transactions\": [\n      \"AQAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP////8KUQEBBi9QMlNIL/////8BAHQ7pAsAAAAjIQIBMfOK4+sHFFMdv8P0VJG0Ex0SEeN3cXdjY4i7WnTD5KwAAAAA\"\n    ]\n  }\n}\n{\n  \"rawMerkleBlock\": \"AgAAACy8+DtikT1W9gXA5YGkiHKDlCjJLl63bNetlLyvCwAAfxHczhQHVSDo90zE3fCStOJuvSO42GZaGuW/xBtY/bTDqV5T//8PHvN6AAABAAAAAX8R3M4UB1Ug6PdMxN3wkrTibr0juNhmWhrlv8QbWP20AQE=\"\n}\n",
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

There are no recently deprecated endpoint, but the previous version of documentation can be [viewed here](https://dashplatform.readme.io/v0.17.0/docs/reference-dapi-endpoints-core-grpc-endpoints).

# Code Reference

Implementation details related to the information on this page can be found in:
- The [DAPI repository](https://github.com/dashevo/dapi) `lib/grpcServer/handlers/core` folder
- The [DAPI-gRPC repository](https://github.com/dashevo/dapi-grpc) `protos` folder