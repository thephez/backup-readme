# Overview

In order to make changes on Dash Platform, you need a wallet with a balance. This tutorial explains how to generate a new wallet, retrieve an address from it, and transfer test funds to the address from a faucet.

## Prerequisites
- [node.js](https://nodejs.org/en/) (v12+)
- Basic familiarity with JavaScript asychronous functions using [async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
- The Dash JavaScript SDK is initialized (covered in [Connect to a Network](tutorial-connecting-to-testnet))

# Code
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  wallet: {\n    mnemonic: null, // this indicates that we want a new wallet to be generated\n                    // if you want to get a new address for an existing wallet\n                    // replace 'null' with an existing wallet mnemonic\n    offlineMode: true,  // this indicates we don't want to sync the chain\n                        // it can only be used when the mnemonic is set to 'null'\n  },\n};\n\nconst client = new Dash.Client(clientOpts);\n\nconst createWallet = async () => {\n  const account = await client.getWalletAccount();\n\n  const mnemonic = client.wallet.exportWallet();\n  const address = account.getUnusedAddress();\n  console.log('Mnemonic:', mnemonic);\n  console.log('Unused address:', address.address);\n};\n\ncreateWallet()\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());\n\n// Handle wallet async errors\nclient.on('error', (error, context) => {\n  console.error(`Client error: ${error.name}`);\n  console.error(context);\n});",
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

Once we connect, we output the newly generated mnemonic from `client.wallet.exportWallet()` and an unused address from the wallet from `account.getUnusedAddress()`.

# Next Step

Using the faucet at http://faucet.testnet.networks.dash.org/, send test funds to the "unused address" from the console output. You will need to wait until the funds are confirmed to use them. There is a block explorer running at https://testnet-insight.dashevo.org/insight/ which can be used to check confirmations.