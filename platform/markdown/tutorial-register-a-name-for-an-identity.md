The purpose of this tutorial is to walk through the steps necessary to register a [Dash Platform Name Service (DPNS)](reference-glossary#dash-platform-naming-service-dpns) name.

# Overview
Dash Platform names make cryptographic identities easy to remember and communicate. An identity may have multiple alias names (`dashAliasIdentityId`) in addition to its default name (`dashUniqueIdentityId`). Additional details regarding identities can be found in the [Identity description](explanation-identity).

**Note**: An identity must have a default name before any aliases can be created for the identity.

## Prerequisites
- [General prerequisites](tutorials-introduction#prerequisites) (Node.js / Dash SDK installed)
- A wallet mnemonic with some funds in it: [Tutorial: Create and Fund a Wallet](tutorial-create-and-fund-a-wallet)
- A Dash Platform identity: [Tutorial: Register an Identity](tutorial-register-an-identity) 
- A name you want to register: [Name restrictions](explanation-dpns#implementation)

# Code

 The examples below demonstrate creating both the default name and alias names.

> 🚧
>
> The name must be the full domain name including the parent domain (i.e. `myname.dash` instead of just `myname`). Currently `dash` is the only top-level domain that may be used.

[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  wallet: {\n    mnemonic: 'a Dash wallet mnemonic with testnet funds goes here',\n    unsafeOptions: {\n      skipSynchronizationBeforeHeight: 650000, // only sync from early-2022\n    },\n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst registerName = async () => {\n  const { platform } = client;\n\n  const identity = await platform.identities.get('an identity ID goes here');\n  const nameRegistration = await platform.names.register(\n    '<identity name goes here>.dash',\n    { dashUniqueIdentityId: identity.getId() },\n    identity,\n  );\n\n  return nameRegistration;\n};\n\nregisterName()\n  .then((d) => console.log('Name registered:\\n', d.toJSON()))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript",
      "name": "Register Name for Identity"
    },
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  wallet: {\n    mnemonic: 'a Dash wallet mnemonic with testnet funds goes here',\n    unsafeOptions: {\n      skipSynchronizationBeforeHeight: 650000, // only sync from early-2022\n    },\n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst registerAlias = async () => {\n  const platform = client.platform;\n  const identity = await platform.identities.get('an identity ID goes here');\n  const aliasRegistration = await platform.names.register(\n    '<identity alias goes here>.dash',\n    { dashAliasIdentityId: identity.getId() },\n    identity,\n  );\n\n  return aliasRegistration;\n};\n\nregisterAlias()\n  .then((d) => console.log('Alias registered:\\n', d.toJSON()))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript",
      "name": "Register Alias for Identity"
    }
  ]
}
[/block]
# What's Happening

After initializing the Client, we fetch the Identity we'll be associating with a name. This is an asynchronous method so we use _await_ to pause until the request is complete. Next, we call `platform.names.register` and pass in the name we want to register, the type of identity record to create, and the identity we just fetched. We wait for the result, and output it to the console.

> 📘Wallet Operations
>
> The JavaScript SDK does not cache wallet information. It re-syncs the entire Core chain for some wallet operations (e.g. `client.getWalletAccount()`) which can result in wait times of  5+ minutes. 
>
> A future release will add caching so that access is much faster after the initial sync. For now, the `skipSynchronizationBeforeHeight` option can be used to sync the wallet starting at a certain block height.