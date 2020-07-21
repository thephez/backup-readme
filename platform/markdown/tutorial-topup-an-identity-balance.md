[block:callout]
{
  "type": "success",
  "body": "New in Dash Platform Protocol v0.13"
}
[/block]
The purpose of this tutorial is to walk through the steps necessary to add credits to an identity's balance.

# Overview

As users interact with Dash Platform applications, the credit balance associated with their identity will decrease. Eventually it will be necessary to topup the balance by converting some Dash to credits.  Additional details regarding credits can be found in the [Credits description](explanation-identity#credits).

For this tutorial you will need:

- A wallet mnemonic with some funds in it: [Tutorial: Create and Fund a Wallet](tutorial-create-and-fund-a-wallet)
- A dash platform identity: [Tutorial: Register an Identity](tutorial-register-an-identity) 

# Code
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  wallet: {\n    mnemonic: 'a Dash wallet mnemonic with evonet funds goes here',\n  }\n};\nconst client = new Dash.Client(clientOpts);\n\nconst topupIdentity = async function () {\n  try {\n    const identityId = 'an identity ID goes here';\n    const topUpAmount = 1000;  // Number of duffs\n\n    const platform = client.platform;\n    const status = await platform.identities.topUp(identityId, topUpAmount);\n    const identity = await platform.identities.get(identityId);\n    console.log('Identity credit balance:', identity.balance);\n  } catch (e) {\n    console.error('Something went wrong:', e);\n  } finally {\n    client.disconnect();\n  }\n}\n\ntopupIdentity()",
      "language": "javascript"
    }
  ]
}
[/block]

# What's Happening

After connecting to the Client, we call `platform.identities.topUp` with an identity ID and a topup amount in duffs. This creates a lock transaction and increases the identity's credit balance by the relevant amount (minus fee). The updated balance is output to the console.
[block:callout]
{
  "type": "info",
  "title": "Dash / Credit Conversion",
  "body": "Dash is converted to credits at a ratio of `1 duff : 1000 credits`. This provides flexibility for very granular platform fees."
}
[/block]