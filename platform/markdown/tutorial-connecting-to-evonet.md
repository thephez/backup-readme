The purpose of this tutorial is to walk through the steps necessary to access the network.

# Overview
Evonet is the initial development network provided for experimentation and evaluation of Dash Platform features.
[block:callout]
{
  "type": "warning",
  "body": "As a development network, Evonet may be subject to frequent updates and changes that break backwards compatibility.",
  "title": "Evonet Stability"
}
[/block]
Platform services are provided via a combination of HTTP and gRPC connections to DAPI, and some connections to an Insight API. Although one could interact with DAPI by connecting to these directly, or by using [DAPI-client](https://github.com/dashevo/dapi-client), the easiest approach is to use the [JavaScript Dash SDK](https://github.com/dashevo/DashJS/).

# Prerequisites
- An installation of [NodeJS](https://nodejs.org/en/download/)

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
## 2. Connect to Dash platform
Create a file with the following contents. Then run it by typing `node <file.js>` (from the command line):
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n};\nconst client = new Dash.Client(clientOpts);\n\nasync function connect() {\n  try {\n    await client.isReady();\n    console.log('connected');\n  } catch (e) {\n    console.error('Something went wrong:', e);\n  } finally {\n    client.disconnect();\n  }\n}\n\nconnect();",
      "language": "javascript"
    }
  ]
}
[/block]
Once this returns successfully, you're ready to begin developing! For details on all SDK options and methods, please refer to the [SDK documentation](https://dashevo.github.io/DashJS/).

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
      "code": "curl --request POST \\\n  --url http://evonet.thephez.com:3000/ \\\n  --header 'content-type: application/json' \\\n  --data '{\"method\":\"getBlockHash\",\"id\":1,\"jsonrpc\":\"2.0\",\"params\":{\"height\": 100 }}'",
      "language": "shell",
      "name": null
    },
    {
      "code": "import requests\n\nurl = \"http://evonet.thephez.com:3000/\"\n\npayload = \"{\\\"method\\\":\\\"getBlockHash\\\",\\\"id\\\":1,\\\"jsonrpc\\\":\\\"2.0\\\",\\\"params\\\":{\\\"height\\\":100}}\"\nheaders = {'content-type': 'application/json'}\n\nresponse = requests.request(\"POST\", url, data=payload, headers=headers)\n\nprint(response.text)\n",
      "language": "python",
      "name": "Python"
    },
    {
      "code": "require 'uri'\nrequire 'net/http'\n\nurl = URI(\"http://evonet.thephez.com:3000/\")\n\nhttp = Net::HTTP.new(url.host, url.port)\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"content-type\"] = 'application/json'\nrequest.body = \"{\\\"method\\\":\\\"getBlockHash\\\",\\\"id\\\":1,\\\"jsonrpc\\\":\\\"2.0\\\",\\\"params\\\":{\\\"height\\\":100}}\"\n\nresponse = http.request(request)\nputs response.read_body",
      "language": "ruby"
    }
  ]
}
[/block]