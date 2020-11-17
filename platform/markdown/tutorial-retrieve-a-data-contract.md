# Overview

In this tutorial we will retrieve the data contract created in the [Register a Data Contract tutorial](tutorial-register-a-data-contract).

## Prerequisites
- [node.js](https://nodejs.org/en/) (v12+)
- Basic familiarity with JavaScript asychronous functions using [async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
- The Dash JavaScript SDK is initialized (covered in [Connecting to EvoNet](tutorial-connecting-to-evonet))

# Code

## Retrieving a data contract
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst client = new Dash.Client();\n\nconst retrieveContract = async () => {\n  const contractId = 'Q894cs83D8REQNo7mAetj1wPJK2W3svrwqaN61aP25W';\n  return client.platform.contracts.get(contractId);\n};\n\nretrieveContract()\n  .then((d) => console.dir(d.toJSON(), { depth: 5 }))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript"
    }
  ]
}
[/block]
# Example Data Contract

The following example response shows a retrieved contract:
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"protocolVersion\": 0,\n  \"$id\": \"E18yBYfRLa4HiKgYevL6EEhVZ4HssBgGoiV8pwb1EaQb\",\n  \"$schema\": \"https://schema.dash.org/dpp-0-4-0/meta/data-contract\",\n  \"ownerId\": \"bKad7BSN23Yr4s6DHUQnNgUTGK4RMPx1LYuqoxRvMcv\",\n  \"documents\": {\n    \"note\": {\n      \"properties\": {\n        \"message\": {\n          \"type\": \"string\"\n        }\n      },\n      \"additionalProperties\": false\n    }\n  }\n}",
      "language": "json"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Please refer to the [data contract reference page](reference-data-contracts) for more comprehensive details related to contracts and documents."
}
[/block]
# What's Happening

After we initialize the Client, we request a contract. The `platform.contracts.get` method takes a single argument: a contract ID. After the contract is retrieved, it is displayed on the console.