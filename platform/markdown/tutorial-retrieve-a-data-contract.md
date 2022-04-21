# Overview

In this tutorial we will retrieve the data contract created in the [Register a Data Contract tutorial](tutorial-register-a-data-contract).

## Prerequisites
- [General prerequisites](tutorials-introduction#prerequisites) (Node.js / Dash SDK installed)
- A Dash Platform Contract ID: [Tutorial: Register a Data Contract](tutorial-register-a-data-contract) 

# Code

## Retrieving a data contract
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst client = new Dash.Client();\n\nconst retrieveContract = async () => {\n  const contractId = '3iaEhdyAVbmSjd59CT6SCrqPjfAfMdPTc8ksydgqSaWE';\n  return client.platform.contracts.get(contractId);\n};\n\nretrieveContract()\n  .then((d) => console.dir(d.toJSON(), { depth: 5 }))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript"
    }
  ]
}
[/block]
## Updating the client app list
[block:callout]
{
  "type": "info",
  "body": "In many cases it may be desirable to work with a newly retrieved data contract using the `<contract name>.<contract document>` syntax (e.g. `dpns.domain`). Data contracts that were created after the client was initialized or not included in the initial client options can be added via `client.getApps().set(...)`."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "const Identifier = require('@dashevo/dpp/lib/Identifier');\nconst myContractId = 'a contract ID';\nconst myContract = await client.platform.contracts.get(myContractId);\n\nclient.getApps().set('myNewContract', {\n  contractId: Identifier.from(myContractId),\n  contract: myContract,\n});",
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
      "code": "{\n   \"protocolVersion\":0,\n   \"$id\":\"C96rCVpck4RdBQXG3zzP5KH4RKzfKVTsmTauu8FQenJi\",\n   \"$schema\":\"https://schema.dash.org/dpp-0-4-0/meta/data-contract\",\n   \"ownerId\":\"FgPx8YHbWv4rMeiP52DfmnAXGtjiCiSaNKCbgT7eCRxh\",\n   \"documents\":{\n      \"note\":{\n         \"properties\":{\n            \"message\":{\n               \"type\":\"string\"\n            }\n         },\n         \"additionalProperties\":false\n      }\n   }\n}",
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

The second code example shows how the contract could be assigned a name to make it easily accessible without initializing an additional client.