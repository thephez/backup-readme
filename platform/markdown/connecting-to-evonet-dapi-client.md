The purpose of this tutorial is to walk through the steps necessary to access the network.

# Overview
EvoNet is the initial development network provided for experimentation and evaluation of Dash Platform features.
[block:callout]
{
  "type": "warning",
  "body": "As a development network, EvoNet may be subject to frequent updates and changes that break backwards compatibility.",
  "title": "EvoNet Stability"
}
[/block]
Platform services are provided via a combination of HTTP and gRPC connections to DAPI, and some connections to an Insight API. Although one could interact with DAPI by connecting to these directly, it's easier to use [DAPI-client](https://github.com/dashevo/platform/tree/master/packages/js-dapi-client).

# Connect via DAPI-client

## 1. Install dapi-client
The JavaScript dapi-client package is available from [npmjs.com](https://www.npmjs.com/package/@dashevo/dapi-client) and can be installed by running `npm install`:
[block:code]
{
  "codes": [
    {
      "code": "npm install @dashevo/dapi-client",
      "language": "shell"
    }
  ]
}
[/block]
## 2. Verify a Successful Response

### a. Create `index.js` and add the following code:
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\n\nlet client = new DAPIClient({\n  seeds: [{\n    service: '18.236.131.253:3000',\n    port: 3000\n  }],\n});\n\nconst start = async () => {\n  let height = await client.getBestBlockHash();\n  console.log('Best block hash:', height);\n};\n\nstart();\n",
      "language": "javascript",
      "name": "JavaScript - index.js"
    }
  ]
}
[/block]
### b. Run `index.js`
[block:code]
{
  "codes": [
    {
      "code": "node index.js",
      "language": "shell"
    }
  ]
}
[/block]
The best block hash will be retrieved and printed to the console.
[block:code]
{
  "codes": [
    {
      "code": "Best block hash: \n  00000038fd1a1f6f691889d986a563372f3f4887320ad27b635caf4fcc9768c1\n",
      "language": "text"
    }
  ]
}
[/block]
Once this returns successfully, you're ready to begin developing!