# Overview

Once you have a wallet and some funds ([tutorial](tutorial-create-and-fund-a-wallet)), another common task is sending Dash to an address. (Sending Dash to a contact or a DPNS identity requires the Dashpay app, which has not been registered yet.)

# Code
[block:callout]
{
  "type": "warning",
  "title": "Wallet Operations",
  "body": "Currently, the JavaScript SDK does not cache wallet information, and therefore, it re-syncs the entire Core chain for some wallet operations (e.g. `client.getWalletAccount()`). This can result in wait times of  5+ minutes. An upcoming release will add a persistence feature to cache wallet information during initial sync so that subsequent access is much faster."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  wallet: {\n    mnemonic: 'your wallet mnemonic goes here',\n    unsafeOptions: {\n      skipSynchronizationBeforeHeight: 415000, // only sync from start of 2021\n    },\n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst sendFunds = async () => {\n  const account = await client.getWalletAccount();\n\n  const transaction = account.createTransaction({\n    recipient: 'yP8A3cbdxRtLRduy5mXDsBnJtMzHWs6ZXr', // Testnet2 faucet\n    satoshis: 100000000, // 1 Dash\n  });\n  return account.broadcastTransaction(transaction);\n};\n\nsendFunds()\n  .then((d) => console.log('Transaction broadcast!\\nTransaction ID:', d))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());\n\n// Handle wallet async errors\nclient.on('error', (error, context) => {\n  console.error(`Client error: ${error.name}`);\n  console.error(context);\n});",
      "language": "javascript"
    }
  ]
}
[/block]
# What's Happening

After initializing the Client, we build a new transaction with `account.createTransaction`. It requires a recipient and an amount in satoshis (often called "duffs" in Dash). 100 million satoshis equals one Dash. We pass the transaction to `account.broadcastTransaction` and wait for it to return. Then we output the result, which is a transaction ID. After that we disconnect from the Client so node can exit.