The purpose of this tutorial is to walk through the steps necessary to access the network.

# Overview

Platform services are provided via a combination of HTTP and gRPC connections to DAPI, and some connections to an Insight API. Although one could interact with DAPI by connecting to these directly, or by using [DAPI-client](https://github.com/dashevo/dapi-client), the easiest approach is to use the [JavaScript Dash SDK](https://github.com/dashevo/js-dash-sdk/).

# Prerequisites
- An installation of [NodeJS](https://nodejs.org/en/download/)
[block:callout]
{
  "type": "info",
  "title": "Minimum Supported Version",
  "body": "As of Dash Platform v0.16, NodeJS v12 or higher must be used"
}
[/block]
# Connect via Dash SDK

## 1. Install the Dash SDK
The JavaScript SDK package is available from npmjs.com and can be installed by running `npm install dash` (from the command line):
[block:code]
{
  "codes": [
    {
      "code": "npm install dash",
      "language": "shell"
    }
  ]
}
[/block]
## 2. Connect to Dash Platform
Create a file with the following contents. Then run it by typing `node <file.js>` (from the command line):
[block:callout]
{
  "type": "info",
  "body": "As of Dash Platform v0.17.0, the SDK connects to testnet by default."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst client = new Dash.Client();\n\nasync function connect() {\n  return await client.getDAPIClient().core.getBestBlockHash();\n}\n\nconnect()\n  .then((d) => console.log('Connected. Best block hash:\\n', d))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript"
    }
  ]
}
[/block]
Once this returns successfully, you're ready to begin developing! For details on all SDK options and methods, please refer to the [SDK documentation](https://dashevo.github.io/js-dash-sdk).

# Connect to a Devnet

The SDK also supports connecting to development networks (devnets). Since devnets can be created by anyone, the client library will be unaware of them unless connection information is provided using one of the options described below.
[block:callout]
{
  "type": "info",
  "body": "Some devnets may not be configured provide seed node capabilities (e.g. local development networks created using the [dashmate tool](tutorial-connect-to-a-network-dash-masternode#local-development-network)). In these cases, use the second method described below ([connect via address](#connect-via-address))."
}
[/block]
## Connect via Seed

Using a seed node is the preferred method when one is available. The client uses the provided seed node to a retrieve a list of available masternodes on the network so requests can be spread across the entire network.
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst client = new Dash.Client({\n  seeds: [{\n    host: 'seed-1.testnet.networks.dash.org',\n    httpPort: 3000,\n    grpcPort: 3010,\n  }],\n});\n\nasync function connect() {\n  return await client.getDAPIClient().core.getBestBlockHash();\n}\n\nconnect()\n  .then((d) => console.log('Connected. Best block hash:\\n', d))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript"
    }
  ]
}
[/block]
## Connect via Address

Custom addresses may be directly specified via `dapiAddresses` on networks that do not provide a seed node or in cases where it is beneficial to know exactly what node(s) are being accessed (e.g. debugging, local development, etc.).
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst client = new Dash.Client({\n  dapiAddresses: [\n    '127.0.0.1:3000:3010',\n    '127.0.0.2:3000:3010',\n  ],\n});\n\nasync function connect() {\n  return await client.getDAPIClient().core.getBestBlockHash();\n}\n\nconnect()\n  .then((d) => console.log('Connected. Best block hash:\\n', d))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript"
    }
  ]
}
[/block]
# Connect Directly to DAPI (Optional) 
[block:callout]
{
  "type": "warning",
  "title": "Advanced Topic",
  "body": "Normally, the Dash SDK, dapi-client, or another library should be used to interact with DAPI. This may be helpful for debugging in some cases, but generally is not required."
}
[/block]
The example below demonstrates retrieving the hash of the best block hash directly from a DAPI node via command line and several languages:
[block:code]
{
  "codes": [
    {
      "code": "curl --request POST \\\n  --url http://seed-1.testnet.networks.dash.org:3000/ \\\n  --header 'content-type: application/json' \\\n  --data '{\"method\":\"getBlockHash\",\"id\":1,\"jsonrpc\":\"2.0\",\"params\":{\"height\": 100 }}'",
      "language": "shell",
      "name": null
    },
    {
      "code": "import requests\n\nurl = \"http://seed-1.testnet.networks.dash.org:3000/\"\n\npayload = \"{\\\"method\\\":\\\"getBlockHash\\\",\\\"id\\\":1,\\\"jsonrpc\\\":\\\"2.0\\\",\\\"params\\\":{\\\"height\\\":100}}\"\nheaders = {'content-type': 'application/json'}\n\nresponse = requests.request(\"POST\", url, data=payload, headers=headers)\n\nprint(response.text)\n",
      "language": "python",
      "name": "Python"
    },
    {
      "code": "require 'uri'\nrequire 'net/http'\n\nurl = URI(\"http://seed-1.testnet.networks.dash.org:3000/\")\n\nhttp = Net::HTTP.new(url.host, url.port)\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"content-type\"] = 'application/json'\nrequest.body = \"{\\\"method\\\":\\\"getBlockHash\\\",\\\"id\\\":1,\\\"jsonrpc\\\":\\\"2.0\\\",\\\"params\\\":{\\\"height\\\":100}}\"\n\nresponse = http.request(request)\nputs response.read_body",
      "language": "ruby"
    }
  ]
}
[/block]