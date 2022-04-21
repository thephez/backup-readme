# Overview

In addition to the SDK methods for interacting with identities, names, contracts, and documents, the SDK also provides direct access to DAPI client methods.

## Prerequisites

- [General prerequisites](tutorials-introduction#prerequisites) (Node.js / Dash SDK installed)

# Code

The following example demonstrates several of the Core DAPI client methods. DAPI client also has several Platform methods accessible via `getDAPIClient().platform.*`. The methods can be found here in the [js-dapi-client repository](https://github.com/dashevo/platform/tree/master/packages/js-dapi-client/lib/methods).
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst client = new Dash.Client();\n\nasync function dapiClientMethods() {\n  console.log(await client.getDAPIClient().core.getBlockHash(1));\n  console.log(await client.getDAPIClient().core.getBestBlockHash());\n  console.log(await client.getDAPIClient().core.getBlockByHeight(1));\n\n  return client.getDAPIClient().core.getStatus();\n}\n\ndapiClientMethods()\n  .then((d) => console.log('Core status:\\n', d))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Examples using DAPI client to access many of the DAPI endpoints can be found in the [DAPI Endpoint Reference section](reference-dapi-endpoints)."
}
[/block]