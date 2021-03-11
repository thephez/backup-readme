# Overview

In this tutorial we will retrieve the identity created in the [Register an Identity tutorial](tutorial-register-an-identity).

## Prerequisites
- [node.js](https://nodejs.org/en/) (v12+)
- Basic familiarity with JavaScript asychronous functions using [async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
- The Dash JavaScript SDK is initialized (covered in [Connecting to a Network](tutorial-connecting-to-testnet))
- A Dash Platform Identity: [Tutorial: Register an Identity](tutorial-register-an-identity) 

# Code
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst client = new Dash.Client();\n\nconst retrieveIdentity = async () => {\n  return client.platform.identities.get('an identity ID goes here');\n};\n\nretrieveIdentity()\n  .then((d) => console.log('Identity retrieved:\\n', d.toJSON()))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
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
      "code": "{\n   \"protocolVersion\":0,\n   \"id\":\"DVNRfGUspUc6r1CiUFN92zNMBh5Y5jc5cZqCiP1XSJ7G\",\n   \"publicKeys\":[\n      {\n         \"id\":0,\n         \"type\":0,\n         \"data\":\"A5IZf6peksdOOH2Qcuh2VDH+m5n52bL6dW6Xt1oMGWjq\"\n      }\n   ],\n   \"balance\":10998939,\n   \"revision\":0\n}",
      "language": "json"
    }
  ]
}
[/block]
# What's Happening

After we initialize the Client, we request an identity. The `platform.identities.get` method takes a single argument: an identity ID. After the identity is retrieved, it is displayed on the console.