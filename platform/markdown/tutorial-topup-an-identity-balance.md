The purpose of this tutorial is to walk through the steps necessary to add credits to an identity's balance.

# Overview

As users interact with Dash Platform applications, the credit balance associated with their identity will decrease. Eventually it will be necessary to topup the balance by converting some Dash to credits.  Additional details regarding credits can be found in the [Credits description](explanation-identity#credits).

## Prerequisites
- [General prerequisites](tutorials-introduction#prerequisites) (Node.js / Dash SDK installed)
- A wallet mnemonic with some funds in it: [Tutorial: Create and Fund a Wallet](tutorial-create-and-fund-a-wallet)
- A Dash Platform Identity: [Tutorial: Register an Identity](tutorial-register-an-identity) 

# Code
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  wallet: {\n    mnemonic: 'a Dash wallet mnemonic with testnet funds goes here',\n    unsafeOptions: {\n      skipSynchronizationBeforeHeight: 650000, // only sync from early-2022\n    },\n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst topupIdentity = async () => {\n  const identityId = 'an identity ID goes here';\n  const topUpAmount = 1000; // Number of duffs\n\n  await client.platform.identities.topUp(identityId, topUpAmount);\n  return client.platform.identities.get(identityId);\n};\n\ntopupIdentity()\n  .then((d) => console.log('Identity credit balance: ', d.balance))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript"
    }
  ]
}
[/block]

# What's Happening

After connecting to the Client, we call `platform.identities.topUp` with an identity ID and a topup amount in duffs. This creates a lock transaction and increases the identity's credit balance by the relevant amount (minus fee). The updated balance is output to the console.

> 📘 Dash / Credit Conversion
>
> Dash is converted to credits at a ratio of `1 duff : 1000 credits`. This provides flexibility for very granular platform fees.

> 📘 Wallet Operations
>
> The JavaScript SDK does not cache wallet information. It re-syncs the entire Core chain for some wallet operations (e.g. `client.getWalletAccount()`) which can result in wait times of  5+ minutes. 
>
> A future release will add caching so that access is much faster after the initial sync. For now, the `skipSynchronizationBeforeHeight` option can be used to sync the wallet starting at a certain block height.