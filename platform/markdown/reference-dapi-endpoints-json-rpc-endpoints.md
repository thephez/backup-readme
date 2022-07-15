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

## getBestBlockHash

**Returns**: the block hash of the chaintip
**Parameters**: none

### Example Request and Response
[block:code]
{
  "codes": [
    {
      "code": "curl --request POST \\\n  --url http://seed-1.testnet.networks.dash.org:3000/ \\\n  --header 'content-type: application/json' \\\n  --data '{\n      \"method\":\"getBestBlockHash\",\n      \"id\":1,\n      \"jsonrpc\":\"2.0\",\n      \"params\":{}\n    }'",
      "language": "curl",
      "name": "Curl"
    },
    {
      "code": "var request = require(\"request\");\n\nvar options = {\n  method: 'POST',\n  url: 'http://seed-1.testnet.networks.dash.org:3000',\n  headers: {'content-type': 'application/json'},\n  body: '{\"method\":\"getBestBlockHash\",\"id\":1,\"jsonrpc\":\"2.0\"}'\n};\n\nrequest(options, function (error, response, body) {\n  if (error) throw new Error(error);\n\n  console.log(body);\n});",
      "language": "javascript"
    },
    {
      "code": "var XMLHttpRequest = require('xhr2');\nvar data = '{\"method\":\"getBestBlockHash\",\"id\":1,\"jsonrpc\":\"2.0\"}';\n\nvar xhr = new XMLHttpRequest();\n\nxhr.addEventListener(\"readystatechange\", function () {\n  if (this.readyState === this.DONE) {\n    console.log(this.responseText);\n  }\n});\n\nxhr.open(\"POST\", \"http://seed-1.testnet.networks.dash.org:3000\");\nxhr.setRequestHeader(\"content-type\", \"application/json\");\n\nxhr.send(data);",
      "language": "javascript",
      "name": "Node"
    },
    {
      "code": "import requests\nimport json\n\nurl = \"http://seed-1.testnet.networks.dash.org:3000/\"\nheaders = {'content-type': 'application/json'}\n\npayload_json = {\n    \"method\": \"getBestBlockHash\",\n    \"id\": 1,\n    \"jsonrpc\": \"2.0\",\n    \"params\": {}\n}\n\nresponse = requests.request(\"POST\", url, data=json.dumps(payload_json), headers=headers)\n\nprint(response.text)",
      "language": "python"
    },
    {
      "code": "require 'uri'\nrequire 'net/http'\n\nurl = URI(\"http://seed-1.testnet.networks.dash.org:3000/\")\nhttp = Net::HTTP.new(url.host, url.port)\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"content-type\"] = 'application/json'\n\nrequest.body = '{\n    \"method\":\"getBestBlockHash\",\n    \"id\":1,\n    \"jsonrpc\":\"2.0\",\n    \"params\":{ }\n}'\n\nresponse = http.request(request)\nputs response.read_body",
      "language": "ruby"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"jsonrpc\": \"2.0\",\n  \"id\": 1,\n  \"result\": \"000000bbdda70535d4fa854ca8c12d67f5788bcc6f7d85d39722bc2abed10f58\"\n}",
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
      "code": "curl --request POST \\\n  --url http://seed-1.testnet.networks.dash.org:3000/ \\\n  --header 'content-type: application/json' \\\n  --data '{\n      \"method\":\"getBlockHash\",\n      \"id\":1,\n      \"jsonrpc\":\"2.0\",\n      \"params\": {\n        \"height\": 1\n       }\n    }'",
      "language": "shell",
      "name": "Curl"
    },
    {
      "code": "import requests\nimport json\n\nurl = \"http://seed-1.testnet.networks.dash.org:3000/\"\nheaders = {'content-type': 'application/json'}\n\npayload_json = {\n    \"method\": \"getBlockHash\",\n    \"id\": 1,\n    \"jsonrpc\": \"2.0\",\n    \"params\": {\n        \"height\": 100\n    }\n}\n\nresponse = requests.request(\"POST\", url, data=json.dumps(payload_json), headers=headers)\n\nprint(response.text)",
      "language": "python"
    },
    {
      "code": "require 'uri'\nrequire 'net/http'\n\nurl = URI(\"http://seed-1.testnet.networks.dash.org:3000/\")\nhttp = Net::HTTP.new(url.host, url.port)\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"content-type\"] = 'application/json'\n\nrequest.body = '{\n    \"method\":\"getBlockHash\",\n    \"id\":1,\n    \"jsonrpc\":\"2.0\",\n    \"params\":{\n        \"height\":100\n    }\n}'\n\nresponse = http.request(request)\nputs response.read_body",
      "language": "ruby"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"jsonrpc\": \"2.0\",\n  \"id\": 1,\n  \"result\": \"6c2f69e9696baf3c28af0d21d85af0e61c6a2adbe319c3714ec4b16adc3e1d2f\"\n}",
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
      "code": "curl --request POST \\\n  --url http://seed-1.testnet.networks.dash.org:3000/ \\\n  --header 'content-type: application/json' \\\n  --data '{\n      \"method\":\"getMnListDiff\",\n      \"id\":1,\n      \"jsonrpc\":\"2.0\",\n      \"params\": {\n        \"baseBlockHash\": \"00000016b4d13db8395b31d87c76ca88824b26e03e54480d8c9ddf6f11857a7c\",\n        \"blockHash\": \"0000002266d8e7836eb116fe467597d13d9862c6612e31bbd6161c35b6485493\"\n      }\n    }'",
      "language": "shell",
      "name": "Curl"
    },
    {
      "code": "import requests\nimport json\n\nurl = \"http://seed-1.testnet.networks.dash.org:3000/\"\nheaders = {'content-type': 'application/json'}\n\npayload_json = {\n    \"method\":\"getMnListDiff\",\n    \"id\":1,\n    \"jsonrpc\":\"2.0\",\n    \"params\": {\n        \"baseBlockHash\": \"00000016b4d13db8395b31d87c76ca88824b26e03e54480d8c9ddf6f11857a7c\",\n        \"blockHash\": \"0000002266d8e7836eb116fe467597d13d9862c6612e31bbd6161c35b6485493\"\n    }\n}  \n\nresponse = requests.request(\"POST\", url, data=json.dumps(payload_json), headers=headers)\n\nprint(response.text)",
      "language": "python"
    },
    {
      "code": "require 'uri'\nrequire 'net/http'\n\nurl = URI(\"http://seed-1.testnet.networks.dash.org:3000/\")\nhttp = Net::HTTP.new(url.host, url.port)\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"content-type\"] = 'application/json'\n\nrequest.body = '{\n    \"method\":\"getMnListDiff\",\n    \"id\":1,\n    \"jsonrpc\":\"2.0\",\n    \"params\": {\n        \"baseBlockHash\": \"00000016b4d13db8395b31d87c76ca88824b26e03e54480d8c9ddf6f11857a7c\",\n        \"blockHash\": \"0000002266d8e7836eb116fe467597d13d9862c6612e31bbd6161c35b6485493\"\n    }\n}'\n\nresponse = http.request(request)\nputs response.read_body",
      "language": "ruby"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"jsonrpc\": \"2.0\",\n  \"id\": 1,\n  \"result\": {\n    \"baseBlockHash\": \"00000016b4d13db8395b31d87c76ca88824b26e03e54480d8c9ddf6f11857a7c\",\n    \"blockHash\": \"0000002266d8e7836eb116fe467597d13d9862c6612e31bbd6161c35b6485493\",\n    \"cbTxMerkleTree\": \"0300000003795f4c55c12757f3783a81a804585545d1844660f0b59e25a93d332bdf98a1032552fe1c2eada657f6714b14e1746a8f09b3a526e88243831133a7e25c9afcde8800af04201e85dc0cffb817c5fe7b4972ccf2647503d3d45f41304b664e8cba0107\",\n    \"cbTx\": \"03000500010000000000000000000000000000000000000000000000000000000000000000ffffffff1202e21b0e2f5032506f6f6c2d74444153482fffffffff04cb525b96010000001976a9144f79c383bc5d3e9d4d81b98f87337cedfa78953688ac26c4609a010000001976a914243f5bceb1ae0a580f5a9415f9a015ad38477e7188ac6e710504000000001976a914badadfdebaa6d015a0299f23fbc1fcbdd72ba96f88ac00000000000000002a6a289c7178341d0097998baa6098724d78fd01b46455890203d99413e86a55ebb610000000000700000000000000260100e21b0000f02004439b25d27e07bb9afbd07db7dfc71aa91338391cef46e08488fe66bfe9\",\n    \"deletedMNs\": [],\n    \"mnList\": [\n      {\n        \"proRegTxHash\": \"682b3e58e283081c51f2e8e7a7de5c7312a2e8074affaf389fafcc39c4805404\",\n        \"confirmedHash\": \"00000018c824355520c6a850076c041b533d05cbe481f8187e541d7e2f856def\",\n        \"service\": \"64.193.62.206:19999\",\n        \"pubKeyOperator\": \"05f2269374676476f00068b7cb168d124b7b780a92e8564e18edf45d77497abd9debf186ee98001a0c9a6dfccbab7a0a\",\n        \"votingAddress\": \"yid7uAsVJzvSLrEekHuGNuY3KWCqJopyJ8\",\n        \"isValid\": true\n      },\n      {\n        \"proRegTxHash\": \"c48a44a9493eae641bea36992bc8c27eaaa33adb1884960f55cd259608d26d2f\",\n        \"confirmedHash\": \"000000237725f8fe7d78153ae9c11193ee0cda18f8b48141acff8e1ac713da5b\",\n        \"service\": \"173.61.30.231:19013\",\n        \"pubKeyOperator\": \"8700add55a28ef22ec042a2f28e25fb4ef04b3024a7c56ad7eed4aebc736f312d18f355370dfb6a5fec9258f464b227e\",\n        \"votingAddress\": \"yTMDce5yEpiPqmgPrPmTj7yAmQPJERUSVy\",\n        \"isValid\": true\n      },\n      {\n        \"proRegTxHash\": \"9f4f9f83ecbcd5739d7f1479ee14b508f2414d044a717acba0960566c4e6091d\",\n        \"confirmedHash\": \"0000000000000000000000000000000000000000000000000000000000000000\",\n        \"service\": \"45.32.211.155:19999\",\n        \"pubKeyOperator\": \"08e37b3fcba972fe0c2c0ea15f8285c8bfb262ad4d8a6741a530154f1abc4edd367a22abd0cb1934647f033913cca58a\",\n        \"votingAddress\": \"ybAZoZ6iybhEwoCfb6utGfU753R1wcQSZT\",\n        \"isValid\": true\n      }\n    ],\n    \"deletedQuorums\": [],\n    \"newQuorums\": [],\n    \"merkleRootMNList\": \"e9bf66fe8884e046ef1c393813a91ac7dfb77dd0fb9abb077ed2259b430420f0\"\n  }\n}",
      "language": "json",
      "name": "Example Response"
    }
  ]
}
[/block]
# Deprecated Endpoints

There are no recently deprecated endpoint, but the previous version of documentation can be [viewed here](https://dashplatform.readme.io/v0.21.0/docs/reference-dapi-endpoints-json-rpc-endpoints).

# Code Reference

Implementation details related to the information on this page can be found in:
- The [DAPI repository](https://github.com/dashevo/platform/tree/master/packages/dapi) `lib/rpcServer/commands` folder