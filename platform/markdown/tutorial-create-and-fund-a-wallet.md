# Overview

In order to make changes on Dash platform, you need a wallet with a balance. This tutorial explains how to generate a new wallet, retrieve an address from it, and transfer test funds to the address from a faucet.

# Code
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  mnemonic: null, // this indicates that we want a new wallet to be generated\n                  // if you want to get a new address for an existing wallet\n                  // replace 'null' with an existing wallet mnemonic\n};\nconst client = new Dash.Client(clientOpts);\n\nasync function createWallet() {\n  try {\n    await client.isReady();\n    const mnemonic = client.wallet.exportWallet();\n    const address = client.account.getUnusedAddress();\n    console.log('Mnemonic:', mnemonic);\n    console.log('Unused address:', address.address);\n  } catch (e) {\n    console.error('Something went wrong:', e);\n  } finally {\n    client.disconnect();\n  }\n}\ncreateWallet();",
      "language": "javascript"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "Mnemonic: thrive wolf habit timber birth service crystal patient tiny depart tower focus\nUnused address: yXF7LsyajRvJGX96vPHBmo9Dwy9zEvzkbh",
      "language": "text",
      "name": "Example output"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "**Please save your mnemonic for the next step and for re-use in subsequent tutorials throughout the documentation.**",
  "title": ""
}
[/block]
# What's Happening

Once we connect to Evonet, we output the newly generated mnemonic from `client.wallet.exportWallet()` and an unused address from the wallet from `client.account.getUnusedAddress()`.

# Next Step

Using the Evonet faucet at http://devnet-evonet-28309188.us-west-2.elb.amazonaws.com/, send test funds to the "unused address" from the console output. You will need to wait until the funds are confirmed to use them. There is an Evonet block explorer running at http://devnet-evonet-28309188.us-west-2.elb.amazonaws.com:3001/insight/ which can be used to check confirmations.