# Overview

In this tutorial we will retrieve the data contract created in the [Register a Data Contract tutorial](tutorial-register-a-data-contract).

# Code

## Retrieving a data contract
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n};\nconst client = new Dash.Client(clientOpts);\n\nconst retrieveContract = async function () {\n  try {\n    const platform = client.platform;\n    const contractId = 'ARQGUnPH3YMK8FZuqwUjnTWEF6Zu4Cf3sT6e1Ruu1RXk';\n    \n    const contract = await platform.contracts.get(contractId);\n    console.dir({contract});\n  } catch (e) {\n    console.error('Something went wrong:', e);\n  } finally {\n    client.disconnect();\n  }\n}\n\nretrieveContract();",
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
      "code": "{\n  id: 'ARQGUnPH3YMK8FZuqwUjnTWEF6Zu4Cf3sT6e1Ruu1RXk',\n  ownerId: 'Gb9YEHMYxcMiSnBhtBQQmttmAoqN1asi6HKQxRR7YdXA',\n  schema: 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',\n  documents: {\n    note: {\n      properties: {\n        message: {\n          type: 'string'\n        }\n      },\n      additionalProperties: false\n    }\n  },\n  definitions: undefined\n}",
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