[block:callout]
{
  "type": "info",
  "title": "gRPC Overview",
  "body": "Please refer to the [gRPC Overview](reference-dapi-endpoints-grpc-overview) for details regarding running the examples shown below, encoding/decoding the request/response data, and clients available for several languages."
}
[/block]
# Endpoint Details

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
      "code": "grpcurl -proto protos/transactions_filter_stream.proto -plaintext \\\n  -d '{\n  \"from_block_height\": 1,\n  \"count\": 1,\n  \"bloom_filter\": {\n    \"n_hash_funcs\": 11,\n    \"v_data\": \"\",\n    \"n_tweak\": 0,\n    \"n_flags\": 0\n  }\n}' \\\n  seed.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.TransactionsFilterStream/subscribeToTransactionsWithProofs",
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
# Code Reference
Implementation details related to the information on this page can be found in:
- The [DAPI repository](https://github.com/dashevo/dapi) `lib/grpcServer/handlers/tx-filter-stream` folder
- The [DAPI-gRPC repository](https://github.com/dashevo/dapi-grpc/) `protos` folder