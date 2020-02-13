# Overview

The endpoints described on this page provide access to information from the Core chain (layer 1).

## Required Parameters
All valid JSON-RPC requests require the inclusion the parameters listed in the following table.

| Name | Type | Description |
| - | - | - |
| `method` | String | Name of the endpoint |
| `id` | Integer | Request id (returned in the response to differentiate results from the same endpoint) |
| `jsonrpc` | String | JSON-RPC version ("2.0") |

Additional information may be found in the [JSON-RPC 2.0 specification](https://www.jsonrpc.org/specification#request_object).

# Endpoint Details
[block:callout]
{
  "type": "danger",
  "title": "Work in Progress",
  "body": "This document is an work in progress. Details may be missing or incomplete and are subject to change."
}
[/block]
## getBestBlockHash

**Returns**: the block hash of the chaintip
**Parameters**: none

### Example Request and Response
[block:code]
{
  "codes": [
    {
      "code": "curl --request POST \\\n  --url http://54.148.62.72:3000/ \\\n  --header 'content-type: application/json' \\\n  --data '{\n      \"method\":\"getBestBlockHash\",\n      \"id\":1,\n      \"jsonrpc\":\"2.0\",\n      \"params\":{}\n    }'",
      "language": "curl",
      "name": "Curl"
    },
    {
      "code": "var request = require(\"request\");\n\nvar options = {\n  method: 'POST',\n  url: 'http://54.148.62.72:3000',\n  headers: {'content-type': 'application/json'},\n  body: '{\"method\":\"getBestBlockHash\",\"id\":1,\"jsonrpc\":\"2.0\"}'\n};\n\nrequest(options, function (error, response, body) {\n  if (error) throw new Error(error);\n\n  console.log(body);\n});",
      "language": "javascript"
    },
    {
      "code": "var XMLHttpRequest = require('xhr2');\nvar data = '{\"method\":\"getBestBlockHash\",\"id\":1,\"jsonrpc\":\"2.0\"}';\n\nvar xhr = new XMLHttpRequest();\n\nxhr.addEventListener(\"readystatechange\", function () {\n  if (this.readyState === this.DONE) {\n    console.log(this.responseText);\n  }\n});\n\nxhr.open(\"POST\", \"http://54.148.62.72:3000\");\nxhr.setRequestHeader(\"content-type\", \"application/json\");\n\nxhr.send(data);",
      "language": "javascript",
      "name": "Node"
    },
    {
      "code": "import requests\nimport json\n\nurl = \"http://54.148.62.72:3000/\"\nheaders = {'content-type': 'application/json'}\n\npayload_json = {\n    \"method\": \"getBestBlockHash\",\n    \"id\": 1,\n    \"jsonrpc\": \"2.0\",\n    \"params\": {}\n}\n\nresponse = requests.request(\"POST\", url, data=json.dumps(payload_json), headers=headers)\n\nprint(response.text)",
      "language": "python"
    },
    {
      "code": "require 'uri'\nrequire 'net/http'\n\nurl = URI(\"http://54.148.62.72:3000/\")\nhttp = Net::HTTP.new(url.host, url.port)\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"content-type\"] = 'application/json'\n\nrequest.body = '{\n    \"method\":\"getBestBlockHash\",\n    \"id\":1,\n    \"jsonrpc\":\"2.0\",\n    \"params\":{ }\n}'\n\nresponse = http.request(request)\nputs response.read_body",
      "language": "ruby"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"jsonrpc\": \"2.0\",\n  \"id\": 1,\n  \"result\": \"000002a50628d34735397e58bb604cdef441f3a0d1455815b94ea6e5ded9e694\"\n}\n",
      "language": "json",
      "name": "Example Response"
    }
  ]
}
[/block]

## getBlockHash

**Returns**:  the block hash for the given height
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `height` | Integer | Yes | Block height |

### Example Request and Response
[block:code]
{
  "codes": [
    {
      "code": "curl --request POST \\\n  --url http://54.148.62.72:3000/ \\\n  --header 'content-type: application/json' \\\n  --data '{\n      \"method\":\"getBlockHash\",\n      \"id\":1,\n      \"jsonrpc\":\"2.0\",\n      \"params\": {\n        \"height\": 1\n       }\n    }'",
      "language": "shell",
      "name": "Curl"
    },
    {
      "code": "import requests\nimport json\n\nurl = \"http://54.148.62.72:3000/\"\nheaders = {'content-type': 'application/json'}\n\npayload_json = {\n    \"method\": \"getBlockHash\",\n    \"id\": 1,\n    \"jsonrpc\": \"2.0\",\n    \"params\": {\n        \"height\": 100\n    }\n}\n\nresponse = requests.request(\"POST\", url, data=json.dumps(payload_json), headers=headers)\n\nprint(response.text)",
      "language": "python"
    },
    {
      "code": "require 'uri'\nrequire 'net/http'\n\nurl = URI(\"http://54.148.62.72:3000/\")\nhttp = Net::HTTP.new(url.host, url.port)\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"content-type\"] = 'application/json'\n\nrequest.body = '{\n    \"method\":\"getBlockHash\",\n    \"id\":1,\n    \"jsonrpc\":\"2.0\",\n    \"params\":{\n        \"height\":100\n    }\n}'\n\nresponse = http.request(request)\nputs response.read_body",
      "language": "ruby"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"jsonrpc\": \"2.0\",\n  \"id\": 1,\n  \"result\": \"5ad690bcbedeb8be47e840cd869485d802c9331488604d57a5a14e8e5db3129d\"\n}\n",
      "language": "json",
      "name": "Example Response"
    }
  ]
}
[/block]

## getMnListDiff

**Returns**: a masternode list diff for the provided block hashes
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `baseBlockHash` | String | Yes | Block hash for the starting block |
| `blockHash` | String | Yes | Block hash for the ending block |

### Example Request and Response
[block:code]
{
  "codes": [
    {
      "code": "curl --request POST \\\n  --url http://54.148.62.72:3000/ \\\n  --header 'content-type: application/json' \\\n  --data '{\n      \"method\":\"getMnListDiff\",\n      \"id\":1,\n      \"jsonrpc\":\"2.0\",\n      \"params\": {\n        \"baseBlockHash\": \"000008ca1832a4baf228eb1553c03d3a2c8e02399550dd6ea8d65cec3ef23d2e\",\n        \"blockHash\": \"0000006d8af73d6fbbe94daa4406a4b731c9eeba9bbbef3903f17c8d97bbec9d\"\n      }\n    }'",
      "language": "shell",
      "name": "Curl"
    },
    {
      "code": "import requests\nimport json\n\nurl = \"http://54.148.62.72:3000/\"\nheaders = {'content-type': 'application/json'}\n\npayload_json = {\n    \"method\":\"getMnListDiff\",\n    \"id\":1,\n    \"jsonrpc\":\"2.0\",\n    \"params\": {\n        \"baseBlockHash\": \"000008ca1832a4baf228eb1553c03d3a2c8e02399550dd6ea8d65cec3ef23d2e\",\n        \"blockHash\": \"0000006d8af73d6fbbe94daa4406a4b731c9eeba9bbbef3903f17c8d97bbec9d\"\n    }\n}  \n\nresponse = requests.request(\"POST\", url, data=json.dumps(payload_json), headers=headers)\n\nprint(response.text)",
      "language": "python"
    },
    {
      "code": "require 'uri'\nrequire 'net/http'\n\nurl = URI(\"http://54.148.62.72:3000/\")\nhttp = Net::HTTP.new(url.host, url.port)\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"content-type\"] = 'application/json'\n\nrequest.body = '{\n    \"method\":\"getMnListDiff\",\n    \"id\":1,\n    \"jsonrpc\":\"2.0\",\n    \"params\": {\n        \"baseBlockHash\": \"000008ca1832a4baf228eb1553c03d3a2c8e02399550dd6ea8d65cec3ef23d2e\",\n        \"blockHash\": \"0000006d8af73d6fbbe94daa4406a4b731c9eeba9bbbef3903f17c8d97bbec9d\"\n    }\n}'\n\nresponse = http.request(request)\nputs response.read_body",
      "language": "ruby"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"jsonrpc\": \"2.0\",\n  \"id\": 1,\n  \"result\": {\n    \"baseBlockHash\": \"000008ca1832a4baf228eb1553c03d3a2c8e02399550dd6ea8d65cec3ef23d2e\",\n    \"blockHash\": \"0000006d8af73d6fbbe94daa4406a4b731c9eeba9bbbef3903f17c8d97bbec9d\",\n    \"cbTxMerkleTree\": \"010000000176d12cae417f1ef536460b68badf8432fc5981a74c261bbec674eb00fb9767250101\",\n    \"cbTx\": \"0300050001000000000000000000000000 ... 77420423c63ab192eee1e5059a831c1743\",\n    \"deletedMNs\": [],\n    \"mnList\": [\n      {\n        \"proRegTxHash\": \"85aaf9fff52f90b646021eb0ea7cbd496c138fec1e844f15f4e1a77c2ae8e681\",\n        \"confirmedHash\": \"00002f3fc4a20541a6c1366610aaac9db7619e37c49cbb67dfe87efb3c89ea60\",\n        \"service\": \"34.222.26.118:20001\",\n        \"pubKeyOperator\": \"032754b590628d7b3d33093ff6c08d5f736145ee11087b6c91c271155c9f2f49699af16c8ba57983613eaf6bb5e16ea8\",\n        \"votingAddress\": \"yfwywWMoARJRxfaVaKVwMNrQZ8yMxgW9QZ\",\n        \"isValid\": true\n      },\n      {\n        \"proRegTxHash\": \"c65b9387ec5638c978bd5de4a7c001ba0c40a72b2dde4d7a08af5c74e9c1fe0d\",\n        \"confirmedHash\": \"00001560e8b38c96f9eadb0f374789f88038b7dd75a3dc82f130d0d1796ee969\",\n        \"service\": \"34.208.77.15:20001\",\n        \"pubKeyOperator\": \"1578adb7666f179f07515345bbe7cb2ca939e691281be802f61ed1449f7fe3331fea3c94115aa6cb5b65b3d7c117d65c\",\n        \"votingAddress\": \"yVEaAuLA2zd2KWGZbHVVnKqRpnVgSNBDMP\",\n        \"isValid\": true\n      }\n    ],\n    \"deletedQuorums\": [],\n    \"newQuorums\": [\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"000000d6e2d82bb77ccf0a0f1e8d24bb1123c480d9b81feb9c18316913e49a1b\",\n        \"signersCount\": 49,\n        \"validMembersCount\": 49,\n        \"quorumPublicKey\": \"80e6314b109a34fd798498f7db3876725970fc31dc0639a396fa82935f6d0a2a1a9e5aeb5bcfd5e9fddd5717cb611efc\"\n      }\n    ],\n    \"merkleRootMNList\": \"e6c47c82edfafb60b36cca4285804f4dfeb40e774d5b522641515519150dc00f\",\n    \"merkleRootQuorums\": \"43171c839a05e5e1ee92b13ac623044277f01a43f6f59c16d20c57c9f12a9b43\"\n  }\n}\n",
      "language": "json",
      "name": "Example Response (truncated)"
    }
  ]
}
[/block]

## getUTXO

**Returns**: unspent outputs for the given address
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `address` | String or Array | Yes | A string containing an address OR an array of addresses |
| `from` | Integer | No | Start of range in the ordered list of latest UTXO |
| `to` | Integer | No | End of range in the ordered list of latest UTXO |
| `fromHeight` | Integer | No | Lowest block height to include |
| `toHeight` | Integer | No | Block height to end on |

### Example Request and Response
[block:code]
{
  "codes": [
    {
      "code": "curl --request POST \\\n  --url http://54.148.62.72:3000/ \\\n  --header 'content-type: application/json' \\\n  --data '{\n      \"method\":\"getUTXO\",\n      \"id\":1,\n      \"jsonrpc\":\"2.0\",\n      \"params\": {\n        \"address\": [\n          \"yeVomBV7cQgdEqUsm3vWxQsLgrwqw7viRH\",\n          \"yN7E9PWBT9c5NBJnzHBU3ZfwzFpQZG9Wpe\"\n        ],\n        \"from\": 0,\n        \"to\": 5,\n        \"fromHeight\": 5000,\n        \"toHeight\": 20000\n      }\n    }'",
      "language": "shell",
      "name": "Curl"
    },
    {
      "code": "import requests\nimport json\n\nurl = \"http://54.148.62.72:3000/\"\nheaders = {'content-type': 'application/json'}\n\npayload_json = {\n    \"method\":\"getUTXO\",\n    \"id\":1,\n    \"jsonrpc\":\"2.0\",\n    \"params\": {\n        \"address\": [\n            \"yeVomBV7cQgdEqUsm3vWxQsLgrwqw7viRH\",\n            \"yN7E9PWBT9c5NBJnzHBU3ZfwzFpQZG9Wpe\"\n          ],\n        \"from\": 0,\n        \"to\": 5,\n        \"fromHeight\": 5000,\n        \"toHeight\": 20000\n    }\n}\n\nresponse = requests.request(\"POST\", url, data=json.dumps(payload_json), headers=headers)\n\nprint(response.text)",
      "language": "python"
    },
    {
      "code": "require 'uri'\nrequire 'net/http'\n\nurl = URI(\"http://54.148.62.72:3000/\")\nhttp = Net::HTTP.new(url.host, url.port)\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"content-type\"] = 'application/json'\n\nrequest.body = '{\n    \"method\":\"getUTXO\",\n    \"id\":1,\n    \"jsonrpc\":\"2.0\",\n    \"params\": {\n        \"address\": [\n            \"yeVomBV7cQgdEqUsm3vWxQsLgrwqw7viRH\",\n            \"yN7E9PWBT9c5NBJnzHBU3ZfwzFpQZG9Wpe\"\n          ],\n        \"from\": 0,\n        \"to\": 5,\n        \"fromHeight\": 5000,\n        \"toHeight\": 20000\n    }\n}'\n\nresponse = http.request(request)\nputs response.read_body",
      "language": "ruby"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"jsonrpc\": \"2.0\",\n  \"id\": 1,\n  \"result\": {\n    \"totalItems\": 2,\n    \"from\": 0,\n    \"to\": 2,\n    \"fromHeight\": 5000,\n    \"items\": [\n      {\n        \"address\": \"yN7E9PWBT9c5NBJnzHBU3ZfwzFpQZG9Wpe\",\n        \"txid\": \"0cb4b3239b8769836a7a8cf21981c6474bd03d9fbbea8035862e052d94351338\",\n        \"outputIndex\": 1,\n        \"script\": \"76a91413a0a7cfeb01f59f4b7ea158f346e5bc62fa58bf88ac\",\n        \"satoshis\": 100001,\n        \"height\": 24999\n      },\n      {\n        \"address\": \"yeVomBV7cQgdEqUsm3vWxQsLgrwqw7viRH\",\n        \"txid\": \"c6a543412fed45e3b6b466c485284c1fbd2ed96b528aef99da708ca34665d8e1\",\n        \"outputIndex\": 1,\n        \"script\": \"76a914c767e1a19bf6e84fc8d312a168425c8c25a5ca9188ac\",\n        \"satoshis\": 39320,\n        \"height\": 24970\n      }\n    ]\n  }\n}\n",
      "language": "json",
      "name": "Example Response"
    }
  ]
}
[/block]
# Code Reference

Implementation details related to the information on this page can be found in the DAPI repository [`lib/rpcServer` folder](https://github.com/dashevo/dapi/tree/v0.9-dev/lib/rpcServer/).