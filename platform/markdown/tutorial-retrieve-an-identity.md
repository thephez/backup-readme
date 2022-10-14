# Overview

In this tutorial we will retrieve the identity created in the [Register an Identity tutorial](tutorial-register-an-identity).

## Prerequisites
- [General prerequisites](tutorials-introduction#prerequisites) (Node.js / Dash SDK installed)
- A Dash Platform Identity: [Tutorial: Register an Identity](tutorial-register-an-identity) 

# Code
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst client = new Dash.Client({ network: 'testnet' });\n\nconst retrieveIdentity = async () => {\n  return client.platform.identities.get('an identity ID goes here');\n};\n\nretrieveIdentity()\n  .then((d) => console.log('Identity retrieved:\\n', d.toJSON()))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript"
    }
  ]
}
[/block]
# Example Identity

The following example response shows a retrieved identity:
[block:code]
{
  "codes": [
    {
      "code": "{\n   \"protocolVersion\":0,\n   \"id\":\"6Jz8pFZFhssKSTacgQmZP14zGZNnFYZFKSbx4WVAJFy3\",\n   \"publicKeys\":[\n      {\n         \"id\":0,\n         \"type\":0,\n         \"data\":\"A4zZl0EaRBB6IlDbyR80YUM2l02qqNUCoIizkQxubtxi\"\n      }\n   ],\n   \"balance\":10997588,\n   \"revision\":0\n}",
      "language": "json"
    }
  ]
}
[/block]
# What's Happening

After we initialize the Client, we request an identity. The `platform.identities.get` method takes a single argument: an identity ID. After the identity is retrieved, it is displayed on the console.