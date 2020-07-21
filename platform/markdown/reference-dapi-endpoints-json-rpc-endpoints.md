# Overview

The endpoints described on this page provide access to information from the Core chain (layer 1).
[block:callout]
{
  "type": "warning",
  "title": "gRPC Migration",
  "body": "The remaining JSON-RPC endpoints will be migrated to gRPC in a future release"
}
[/block]
## Required Parameters
All valid JSON-RPC requests require the inclusion the parameters listed in the following table.

| Name | Type | Description |
| - | - | - |
| `method` | String | Name of the endpoint |
| `id` | Integer | Request id (returned in the response to differentiate results from the same endpoint) |
| `jsonrpc` | String | JSON-RPC version ("2.0") |

Additional information may be found in the [JSON-RPC 2.0 specification](https://www.jsonrpc.org/specification#request_object).

# Endpoint Details

## getAddressSummary

**Returns**:  a summary of details related to the provided address(es)
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `address` | String or Array of Strings | Yes | Dash address(es) |

### Example Request and Response
[block:code]
{
  "codes": [
    {
      "code": "curl --request POST \\\n  --url http://seed.evonet.networks.dash.org:3000/ \\\n  --header 'content-type: application/json' \\\n  --data '{\n      \"method\":\"getAddressSummary\",\n      \"id\":1,\n      \"jsonrpc\":\"2.0\",\n      \"params\":{\n        \"address\": [\n          \"yPan6DeKoRSzvLBXvdWijh5rWzJGPYUr9B\",\n          \"ySnJVXXx9FtKUBTkovPaPPqCkTMNzDLPCu\"\n        ]\n      }\n    }'",
      "language": "curl",
      "name": "Curl"
    },
    {
      "code": "var request = require(\"request\");\n\nvar options = {\n  method: 'POST',\n  url: 'http://seed.evonet.networks.dash.org:3000',\n  headers: {'content-type': 'application/json'},\n  body: '{\"method\":\"getAddressSummary\",\"id\":1,\"jsonrpc\":\"2.0\",\"params\":{\"address\": [\"yPan6DeKoRSzvLBXvdWijh5rWzJGPYUr9B\", \"ySnJVXXx9FtKUBTkovPaPPqCkTMNzDLPCu\"]}}'\n};\n\nrequest(options, function (error, response, body) {\n  if (error) throw new Error(error);\n\n  console.log(body);\n});",
      "language": "javascript"
    },
    {
      "code": "var XMLHttpRequest = require('xhr2');\nvar data = '{\"method\":\"getAddressSummary\",\"id\":1,\"jsonrpc\":\"2.0\",\"params\":{\"address\": [\"yPan6DeKoRSzvLBXvdWijh5rWzJGPYUr9B\", \"ySnJVXXx9FtKUBTkovPaPPqCkTMNzDLPCu\"]}}';\n\nvar xhr = new XMLHttpRequest();\n\nxhr.addEventListener(\"readystatechange\", function () {\n  if (this.readyState === this.DONE) {\n    console.log(this.responseText);\n  }\n});\n\nxhr.open(\"POST\", \"http://seed.evonet.networks.dash.org:3000\");\nxhr.setRequestHeader(\"content-type\", \"application/json\");\n\nxhr.send(data);",
      "language": "javascript",
      "name": "Node"
    },
    {
      "code": "import requests\nimport json\n\nurl = \"http://seed.evonet.networks.dash.org:3000/\"\nheaders = {'content-type': 'application/json'}\n\npayload_json = {\n    \"method\": \"getAddressSummary\",\n    \"id\": 1,\n    \"jsonrpc\": \"2.0\",\n    \"params\":{\n      \"address\": [\n        \"yPan6DeKoRSzvLBXvdWijh5rWzJGPYUr9B\",\n        \"ySnJVXXx9FtKUBTkovPaPPqCkTMNzDLPCu\"\n      ]\n    }\n}\n\nresponse = requests.request(\"POST\", url, data=json.dumps(payload_json), headers=headers)\n\nprint(response.text)",
      "language": "python"
    },
    {
      "code": "require 'uri'\nrequire 'net/http'\n\nurl = URI(\"http://seed.evonet.networks.dash.org:3000/\")\nhttp = Net::HTTP.new(url.host, url.port)\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"content-type\"] = 'application/json'\n\nrequest.body = '{\n    \"method\":\"getAddressSummary\",\n    \"id\":1,\n    \"jsonrpc\":\"2.0\",\n    \"params\":{\n      \"address\": [\n        \"yPan6DeKoRSzvLBXvdWijh5rWzJGPYUr9B\",\n        \"ySnJVXXx9FtKUBTkovPaPPqCkTMNzDLPCu\"\n      ]\n    }\n}'\n\nresponse = http.request(request)\nputs response.read_body",
      "language": "ruby"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"jsonrpc\": \"2.0\",\n  \"id\": 1,\n  \"result\": {\n    \"addrStr\": [\n      \"yPan6DeKoRSzvLBXvdWijh5rWzJGPYUr9B\",\n      \"ySnJVXXx9FtKUBTkovPaPPqCkTMNzDLPCu\"\n    ],\n    \"balance\": 0,\n    \"balanceSat\": 0,\n    \"totalReceived\": 0.001,\n    \"totalReceivedSat\": 100000,\n    \"totalSent\": 0.001,\n    \"totalSentSat\": 100000,\n    \"unconfirmedBalance\": 0,\n    \"unconfirmedBalanceSat\": 0,\n    \"unconfirmedTxApperances\": 0,\n    \"unconfirmedAppearances\": 0,\n    \"txApperances\": 2,\n    \"txAppearances\": 2,\n    \"transactions\": [\n      \"c78df31795184aa6582318bd748e1367d0e45d22428aed902dd7ad2e1b6940d8\",\n      \"2fc47fd031c2c4577ddc7248aded5b1b1c4338b2c408c33f480cad9ece61d1fc\"\n    ]\n  }\n}\n",
      "language": "json",
      "name": "Example Response"
    }
  ]
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
      "code": "curl --request POST \\\n  --url http://seed.evonet.networks.dash.org:3000/ \\\n  --header 'content-type: application/json' \\\n  --data '{\n      \"method\":\"getBestBlockHash\",\n      \"id\":1,\n      \"jsonrpc\":\"2.0\",\n      \"params\":{}\n    }'",
      "language": "curl",
      "name": "Curl"
    },
    {
      "code": "var request = require(\"request\");\n\nvar options = {\n  method: 'POST',\n  url: 'http://seed.evonet.networks.dash.org:3000',\n  headers: {'content-type': 'application/json'},\n  body: '{\"method\":\"getBestBlockHash\",\"id\":1,\"jsonrpc\":\"2.0\"}'\n};\n\nrequest(options, function (error, response, body) {\n  if (error) throw new Error(error);\n\n  console.log(body);\n});",
      "language": "javascript"
    },
    {
      "code": "var XMLHttpRequest = require('xhr2');\nvar data = '{\"method\":\"getBestBlockHash\",\"id\":1,\"jsonrpc\":\"2.0\"}';\n\nvar xhr = new XMLHttpRequest();\n\nxhr.addEventListener(\"readystatechange\", function () {\n  if (this.readyState === this.DONE) {\n    console.log(this.responseText);\n  }\n});\n\nxhr.open(\"POST\", \"http://seed.evonet.networks.dash.org:3000\");\nxhr.setRequestHeader(\"content-type\", \"application/json\");\n\nxhr.send(data);",
      "language": "javascript",
      "name": "Node"
    },
    {
      "code": "import requests\nimport json\n\nurl = \"http://seed.evonet.networks.dash.org:3000/\"\nheaders = {'content-type': 'application/json'}\n\npayload_json = {\n    \"method\": \"getBestBlockHash\",\n    \"id\": 1,\n    \"jsonrpc\": \"2.0\",\n    \"params\": {}\n}\n\nresponse = requests.request(\"POST\", url, data=json.dumps(payload_json), headers=headers)\n\nprint(response.text)",
      "language": "python"
    },
    {
      "code": "require 'uri'\nrequire 'net/http'\n\nurl = URI(\"http://seed.evonet.networks.dash.org:3000/\")\nhttp = Net::HTTP.new(url.host, url.port)\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"content-type\"] = 'application/json'\n\nrequest.body = '{\n    \"method\":\"getBestBlockHash\",\n    \"id\":1,\n    \"jsonrpc\":\"2.0\",\n    \"params\":{ }\n}'\n\nresponse = http.request(request)\nputs response.read_body",
      "language": "ruby"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"jsonrpc\": \"2.0\",\n  \"id\": 1,\n  \"result\": \"000001c61729f702adae5266adf68f378211a5ea33970683724894f0e1bae56d\"\n}\n",
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
      "code": "curl --request POST \\\n  --url http://seed.evonet.networks.dash.org:3000/ \\\n  --header 'content-type: application/json' \\\n  --data '{\n      \"method\":\"getBlockHash\",\n      \"id\":1,\n      \"jsonrpc\":\"2.0\",\n      \"params\": {\n        \"height\": 1\n       }\n    }'",
      "language": "shell",
      "name": "Curl"
    },
    {
      "code": "import requests\nimport json\n\nurl = \"http://seed.evonet.networks.dash.org:3000/\"\nheaders = {'content-type': 'application/json'}\n\npayload_json = {\n    \"method\": \"getBlockHash\",\n    \"id\": 1,\n    \"jsonrpc\": \"2.0\",\n    \"params\": {\n        \"height\": 100\n    }\n}\n\nresponse = requests.request(\"POST\", url, data=json.dumps(payload_json), headers=headers)\n\nprint(response.text)",
      "language": "python"
    },
    {
      "code": "require 'uri'\nrequire 'net/http'\n\nurl = URI(\"http://seed.evonet.networks.dash.org:3000/\")\nhttp = Net::HTTP.new(url.host, url.port)\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"content-type\"] = 'application/json'\n\nrequest.body = '{\n    \"method\":\"getBlockHash\",\n    \"id\":1,\n    \"jsonrpc\":\"2.0\",\n    \"params\":{\n        \"height\":100\n    }\n}'\n\nresponse = http.request(request)\nputs response.read_body",
      "language": "ruby"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"jsonrpc\": \"2.0\",\n  \"id\": 1,\n  \"result\": \"13d210271ede6692c39244c613a6d3aab8bdb71be2c0d269749f6a202ec5e324\"\n}",
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
      "code": "curl --request POST \\\n  --url http://seed.evonet.networks.dash.org:3000/ \\\n  --header 'content-type: application/json' \\\n  --data '{\n      \"method\":\"getMnListDiff\",\n      \"id\":1,\n      \"jsonrpc\":\"2.0\",\n      \"params\": {\n        \"baseBlockHash\": \"000028374d1bdcf0fd9c88b32f73f5cb931671ed61a6836cda508b38f89ad980\",\n        \"blockHash\": \"000001c61729f702adae5266adf68f378211a5ea33970683724894f0e1bae56d\"\n      }\n    }'",
      "language": "shell",
      "name": "Curl"
    },
    {
      "code": "import requests\nimport json\n\nurl = \"http://seed.evonet.networks.dash.org:3000/\"\nheaders = {'content-type': 'application/json'}\n\npayload_json = {\n    \"method\":\"getMnListDiff\",\n    \"id\":1,\n    \"jsonrpc\":\"2.0\",\n    \"params\": {\n        \"baseBlockHash\": \"0000248ff415f1a3155b30c4041bdb681c25a69a319b83676f878d8214441e0b\",\n        \"blockHash\": \"000001c61729f702adae5266adf68f378211a5ea33970683724894f0e1bae56d\"\n    }\n}  \n\nresponse = requests.request(\"POST\", url, data=json.dumps(payload_json), headers=headers)\n\nprint(response.text)",
      "language": "python"
    },
    {
      "code": "require 'uri'\nrequire 'net/http'\n\nurl = URI(\"http://seed.evonet.networks.dash.org:3000/\")\nhttp = Net::HTTP.new(url.host, url.port)\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"content-type\"] = 'application/json'\n\nrequest.body = '{\n    \"method\":\"getMnListDiff\",\n    \"id\":1,\n    \"jsonrpc\":\"2.0\",\n    \"params\": {\n        \"baseBlockHash\": \"0000248ff415f1a3155b30c4041bdb681c25a69a319b83676f878d8214441e0b\",\n        \"blockHash\": \"000001c61729f702adae5266adf68f378211a5ea33970683724894f0e1bae56d\"\n    }\n}'\n\nresponse = http.request(request)\nputs response.read_body",
      "language": "ruby"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"jsonrpc\": \"2.0\",\n  \"id\": 1,\n  \"result\": {\n    \"baseBlockHash\": \"000028374d1bdcf0fd9c88b32f73f5cb931671ed61a6836cda508b38f89ad980\",\n    \"blockHash\": \"000001c61729f702adae5266adf68f378211a5ea33970683724894f0e1bae56d\",\n    \"cbTxMerkleTree\": \"0200000002ae436fd5b784556551f5c7d328154560c970e5bb72e9bad62a9a3b69e8b6765708d50dd305e6ffefcaf63b633052328645b94aa20b85b484ce6e27f193a68a690103\",\n    \"cbTx\": \"03000500010000000000000000000000000000000000000000000000000000000000000000ffffffff050235130105ffffffff0200c11a3d050000001976a91416b93a3b9168a20605cc3cda62f6135a3baa531a88ac00c11a3d050000001976a91416b93a3b9168a20605cc3cda62f6135a3baa531a88ac00000000260100351300001f34fc81526a4525c8860ad6f4a9e242b67995a77a28ef84c662e6fccd9436ac\",\n    \"deletedMNs\": [],\n    \"mnList\": [\n      {\n        \"proRegTxHash\": \"9a10460e6414889e46e1a132a55a8cbb2ac8bf5095ada34892c16dadc5810f83\",\n        \"confirmedHash\": \"00002a4ab528605d24f3da41fa95cb705da61465207ccfc1fd8586c6262d362a\",\n        \"service\": \"34.222.46.203:20001\",\n        \"pubKeyOperator\": \"02a9a65cd9b228d3d79aec818213eafe37eb5c564337a4bd405cb6048aa92b612c70fdc5949ff8bd2a038a9f020672c6\",\n        \"votingAddress\": \"yP31544VWmsSjLUyun8u84ecpYfYuiE43W\",\n        \"isValid\": true\n      },\n      {\n        \"proRegTxHash\": \"d716bf38d6aad3a1b48d375cbdcd8ba9729f0caa258f56f40242912eddc15ce4\",\n        \"confirmedHash\": \"00002a4ab528605d24f3da41fa95cb705da61465207ccfc1fd8586c6262d362a\",\n        \"service\": \"35.167.1.167:20001\",\n        \"pubKeyOperator\": \"90c20febc30d1565453f6b50770df7716304df570aabcb73e2ff058541ec5166f8d3ee67ae769b67e489e1c019b67544\",\n        \"votingAddress\": \"yiKQqUVMbY3vXCnJ5hpQmvqwd4G1LhXwv5\",\n        \"isValid\": true\n      },\n      {\n        \"proRegTxHash\": \"b7c633363ac1fa9a0e2142cbff0fbc002f9bf157beb1b5be75cd4eda3b3f1d6c\",\n        \"confirmedHash\": \"00002a4ab528605d24f3da41fa95cb705da61465207ccfc1fd8586c6262d362a\",\n        \"service\": \"54.190.26.164:20001\",\n        \"pubKeyOperator\": \"15d2a4d88e3c0f543b5447f9ea9e218901c4413308109d935d9fdafa2bb2463dc8a04d9ae8b5825d8f76552e9874393a\",\n        \"votingAddress\": \"yWUZKFYbnRQ1rzj59t86WedtWonbE4BUgo\",\n        \"isValid\": true\n      },\n      {\n        \"proRegTxHash\": \"f40e33f8a324ae03677a6fd8d348903d1944ed3e49882819421537385a8a35db\",\n        \"confirmedHash\": \"00001f266c286dabf1b3d21c3d74b553449f36851c07fec28b7217e859084238\",\n        \"service\": \"34.218.66.230:20001\",\n        \"pubKeyOperator\": \"04feab7e24867c7cdd05e66cec3590e31a753ac7f96130704a6b20054e0b56098d4b9b287f6d56cd421271aaa35daa26\",\n        \"votingAddress\": \"yiv8RLA4Qiivg39Fk1CbiAbSy2gjYcJKan\",\n        \"isValid\": true\n      },\n      {\n        \"proRegTxHash\": \"1916e5c4cea747eecd862f215fd89dece234ef4a60cfc4e8f9ec92ac3ad8c0c8\",\n        \"confirmedHash\": \"00002a4ab528605d24f3da41fa95cb705da61465207ccfc1fd8586c6262d362a\",\n        \"service\": \"54.245.150.218:20001\",\n        \"pubKeyOperator\": \"06f64812ad5b76bc1afe1888e6a396fe07054509c20dc765e04f0c2019334120c3e7b1b82357354d818b4f78bf86fa2f\",\n        \"votingAddress\": \"yaRxyJoxguKKe9z53zTucg3e9SDJd4KxiK\",\n        \"isValid\": true\n      },\n      {\n        \"proRegTxHash\": \"1226705f0ee9611a98cfffaf77572e2530c253e7ac9f8a67012a461efa57efbd\",\n        \"confirmedHash\": \"00002a4ab528605d24f3da41fa95cb705da61465207ccfc1fd8586c6262d362a\",\n        \"service\": \"34.222.243.95:20001\",\n        \"pubKeyOperator\": \"8bdd067141601be5780048c07d1604008c01607456492bea3abf935f8ebd574a8ff9f16d5b3c3f1d6a4f08e7fbae7c62\",\n        \"votingAddress\": \"ydUjb4BAQeJf6fxDEC7n1LeYRHo8MWVT6x\",\n        \"isValid\": true\n      }\n    ],\n    \"deletedQuorums\": [],\n    \"newQuorums\": [\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"00000047818648589afcc2ed70aaef033b76f017bfab12f96fe64309411d801d\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"966cceb6ecccd8c434beee6e538fe53cda9d8d72b65ac8a63d1933b73671ce9edbbfa45ef878e688176d0753c3793c36\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"0000014a4f152f0967d611375511ef47454b62a98ca3896c21aca6b1d1887322\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"0ff13bdc8e2aa03929e0a809f51ca388a5ab7e310034d62c8580116f5a558fb522ef58ff620f5a49946b7ecc73e2ff69\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"000001197b385d1203c4462f128a46fdd19838267b2beb5a6f1f32ef32dcc133\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"0a767014eef7b7e3dc46f46f6933b7b45d36ff3a285991bb172b530eb73c195946955c9e0b258e2cce83b0289d71c33a\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"000000047769c284231c4ce002cd8f976ae5a8d248f168a71ea98d5f94bdd433\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"0a0d716e5fd28935109bf4291ad438ec02d3c0e87acd4c172dec93822e373c978673c7c39aa100fd71fc403cb79153d5\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"0000010801525f1e57ba82466ac28951ba923d649ae7377d5234dd0e4e20e444\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"05fca8c16532a367da0e73b4b53c430635854d4e59fa814049558f1be6970b98adfba8c0f836b5b65653be1fb34e81f8\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"00000125ba41adc2205c1e8adc814bbf77a00e654e83d86b35e53dfac15a3845\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"917ed28a60e5e00056350c7e927e873e61b531ec60b9b10a958843006a9c8adb90a7f8bf8886dc3e7c28a54320fabef1\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"0000007b01feb305e1b5325925dc8d1c13e3371cbdbc4eea677f68d6e086e947\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"15d0a1fea708059ffe1570f6ee5a6c3e6c835f21d37cc55fad9caf493ae983ba033d356dc1de151209d4e18538b156a1\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"000000f1255d44fb070ed748ae9e5fede15024a081a8acc3b839ce8b2ba14a70\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"91cefd6b51b082828ca164ad8b0dc143bc4c431e370de0829ed8707378ab2e228fb561782f732a4345b4cafd6747b475\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"0000008f92785c346b38a94a7a373733bc4e4f7064aaa3041dd1b6a86b917f7d\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"86b1d725b026fd5e79296104439eb395b2237de2c864c7c2a2e5f1cda39c1a6c90f4bfd7898ae32bf9cdd32bc7ad1404\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"00000100ab30c3092b2d33f7ec013e6899acf864ce289350346ceca9f0a9927f\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"0e29bb92b5638da22827c83e75852cb69d6342daf08c6cbbd1be96a99d881b5f98a2cdb1842d8bc3e04f736cb0a1e3ff\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"000001ec77603463070547f553df735551fc000dc7785c5adfed7d757c415383\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"113445850b07bd2a65222dd21331404650136889526d12221e799d3f5a0703e5b0edb70f2463aa3b466b51d287f53296\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"00000113d83d24d8b264c02b23649671a8624ae0d810e1bc25507f529bd22e8b\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"93cf226f0bdd9c48510910d54a26cd6009e98e4a2f2c33511a9c4ab39c0128b4f5e66ab17203233716157d3b1202ac46\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"000001498690a569663d15d0268f788c531b2f9347932dc3344fc1528c28e89e\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"841f10acabde4246bef85e99226f623f8029a1ec8bf30255fb64627c34909b57435d77e9a7e0501ab6176861f299d72d\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"000000e37aeb9aef3a98900c0b588db48439bc56fd7f7ae3be023361f9d6d5a8\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"950475b0617d622d06b192b4cf7d2782fda8b3c4bd3d32b2767e4e83a1c0e5e256938236421fcd7ebcfe303f60d8981f\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"000003440780432b7855ee5b7a0717f712ca3f668a1d749f7e3fd2f1cd2cadb4\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"91f675218c326caf3ca51ed2b017fbe7479c88046680dafd76d280ad78e43f066d29ffbec52c11700d78bab7bc3c1e05\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"000000059b154abb42031fd8cec94322fcdba2cf77402e0d57ccf8adf173b7ba\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"89f8f37f5121c541c1fa01a58443b57135c522aa8187c17f5a63ac2174e907562ecd6863ef432646cd7bb8ffe5e63bf6\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"0000036ff469a1278c80f7a3101887fbeff752c2f76777a08e466bc9f9f1c5be\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"0835b4e2ba7bb0ceb1588b1441af0dbf7060dff35dac97a591727e2535823feb2784588e499b85277b5c56577a2d3890\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"0000003d0641a351f153ed4b672eadf1549324388fda824fd096445d2e8178c5\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"09a1a5f0b3a4f4f24e006e289ced712809f9352e50b7787fe1b92e5445a2c353560ddeb5a4d5b931e3375493f78bfb39\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"0000010fed8e007a5b28d975668dc9720a8d3af180014cbf3a675ca5cd73afc6\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"12ed08ec43e92158d45d3a81b4e0e0a319527d56f34872c7703718542d39afcbb2fb43462e5390a351481833e35d114c\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"00000294602e87e5a2b7491ea009c62b9855254513cc80e4195969ad999178e7\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"0e722532fc9beea6a56f5a5e54428abb389a11f101f91eb31ee5c6ffb8fea51b08aa9e18ba15ff066634c2b2ed82c3b5\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"000002c4973242529a2e8b752ebff9d2212d569d26d9fc31e2f02f172b0ab7ee\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"0d40d96fc83b4e8cb50a4c1403d2633941032fc167bd6c4cd67fba0d75cd8268c28f5399e8fffc092d24aefa5030e625\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"0000002501edf46e9284450e97edb18c74b33d092869ae04e54539d6f7991bf8\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"97fb126d626f1cbb1bbd0b2ce737a9237e5000b8239d431f3e0e87974bb9938ff45c74e5c270ba88ba9c2e44194dd2b9\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"0000068c0426cdc4e858f070c722098fcfded1a5448b35b7acbb5326307e3cf9\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"82f20d917a7325d7d89d191364666330d7cbe8d5871934b1df9bf51cc37ed0ae36b076234044b69e7f39f1496a5759b8\"\n      },\n      {\n        \"version\": 1,\n        \"llmqType\": 1,\n        \"quorumHash\": \"0000002044e43004114b301aa3b36f1aec64f79b22231e8b8003a63cef3199fe\",\n        \"signersCount\": 50,\n        \"validMembersCount\": 50,\n        \"quorumPublicKey\": \"111ac846cc414c1bf6ed31921e3421f637c65f80db8c9fdf8ca9e08e06e568dc1b040fd7a09e38567f4a9ef8c8ee891c\"\n      }\n    ],\n    \"merkleRootMNList\": \"ac3694cdfce662c684ef287aa79579b642e2a9f4d60a86c825456a5281fc341f\"\n  }\n}\n",
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
      "code": "curl --request POST \\\n  --url http://seed.evonet.networks.dash.org:3000/ \\\n  --header 'content-type: application/json' \\\n  --data '{\n      \"method\":\"getUTXO\",\n      \"id\":1,\n      \"jsonrpc\":\"2.0\",\n      \"params\": {\n        \"address\": [\n          \"yWfKKt8JCGJWZRsNGe2ZJ43pUpmxEKiqwL\",\n          \"yN7E9PWBT9c5NBJnzHBU3ZfwzFpQZG9Wpe\"\n        ],\n        \"from\": 0,\n        \"to\": 5,\n        \"fromHeight\": 1000,\n        \"toHeight\": 20000\n      }\n    }'",
      "language": "shell",
      "name": "Curl"
    },
    {
      "code": "import requests\nimport json\n\nurl = \"http://seed.evonet.networks.dash.org:3000/\"\nheaders = {'content-type': 'application/json'}\n\npayload_json = {\n    \"method\":\"getUTXO\",\n    \"id\":1,\n    \"jsonrpc\":\"2.0\",\n    \"params\": {\n        \"address\": [\n            \"yeVomBV7cQgdEqUsm3vWxQsLgrwqw7viRH\",\n            \"yN7E9PWBT9c5NBJnzHBU3ZfwzFpQZG9Wpe\"\n          ],\n        \"from\": 0,\n        \"to\": 5,\n        \"fromHeight\": 5000,\n        \"toHeight\": 20000\n    }\n}\n\nresponse = requests.request(\"POST\", url, data=json.dumps(payload_json), headers=headers)\n\nprint(response.text)",
      "language": "python"
    },
    {
      "code": "require 'uri'\nrequire 'net/http'\n\nurl = URI(\"http://seed.evonet.networks.dash.org:3000/\")\nhttp = Net::HTTP.new(url.host, url.port)\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"content-type\"] = 'application/json'\n\nrequest.body = '{\n    \"method\":\"getUTXO\",\n    \"id\":1,\n    \"jsonrpc\":\"2.0\",\n    \"params\": {\n        \"address\": [\n            \"yeVomBV7cQgdEqUsm3vWxQsLgrwqw7viRH\",\n            \"yN7E9PWBT9c5NBJnzHBU3ZfwzFpQZG9Wpe\"\n          ],\n        \"from\": 0,\n        \"to\": 5,\n        \"fromHeight\": 5000,\n        \"toHeight\": 20000\n    }\n}'\n\nresponse = http.request(request)\nputs response.read_body",
      "language": "ruby"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"jsonrpc\": \"2.0\",\n  \"id\": 1,\n  \"result\": {\n    \"totalItems\": 1,\n    \"from\": 0,\n    \"to\": 1,\n    \"fromHeight\": 1000,\n    \"items\": [\n      {\n        \"address\": \"yWfKKt8JCGJWZRsNGe2ZJ43pUpmxEKiqwL\",\n        \"txid\": \"c78df31795184aa6582318bd748e1367d0e45d22428aed902dd7ad2e1b6940d8\",\n        \"outputIndex\": 2,\n        \"script\": \"76a91471734c2117e141f687deabc1aa7e232867b776ca88ac\",\n        \"satoshis\": 79320,\n        \"height\": 4887\n      }\n    ]\n  }\n}\n",
      "language": "json",
      "name": "Example Response"
    }
  ]
}
[/block]
# Code Reference

Implementation details related to the information on this page can be found in:
- The [DAPI repository](https://github.com/dashevo/dapi) `lib/rpcServer/commands` folder