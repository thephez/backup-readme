# Overview

Once you have a wallet and some funds ([tutorial](tutorial-create-and-fund-a-wallet)), another common task is sending Dash to an address. (Sending Dash to a contact or a DPNS identity requires the Dashpay app, which has not been registered yet.)

# Code
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  mnemonic: 'your wallet mnemonic goes here'\n};\nconst client = new Dash.Client(clientOpts);\n\nasync function sendFunds() {\n  await client.isReady();\n  try {\n    const transaction = client.account.createTransaction({\n      recipient: 'yNPbcFfabtNmmxKdGwhHomdYfVs6gikbPf', // Evonet faucet\n      satoshis: 100000000, // 1 Dash\n    });\n    const result = await client.account.broadcastTransaction(transaction);\n    console.log('Transaction broadcast!\\nTransaction ID:', result);\n  } catch (e) {\n    console.error('Something went wrong:', e);\n  } finally {\n    client.disconnect();\n  }\n}\n\nsendFunds();",
      "language": "javascript"
    }
  ]
}
[/block]
# What's Happening

After initializing the Client, we build a new transaction with `client.account.createTransaction`. It requires a recipient and an amount in satoshis (often called "duffs" in Dash). 100 million satoshis equals one Dash. We pass the transaction to `client.account.broadcastTransaction` and wait for it to return. Then we output the result, which is a transaction ID. After that we disconnect from the Client so node can exit.