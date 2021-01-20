The purpose of this tutorial is to walk through the steps necessary to register a [Dash Platform Name Service (DPNS)](reference-glossary#dash-platform-naming-service-dpns) name.

# Overview
Dash Platform names make cryptographic identities easy to remember and communicate. 

Additional details regarding identities can be found in the [Identity description](explanation-identity).

## Prerequisites
- [node.js](https://nodejs.org/en/) (v12+)
- Basic familiarity with JavaScript asychronous functions using [async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
- The Dash JavaScript SDK is initialized (covered in [Connecting to EvoNet](tutorial-connecting-to-evonet))
- A wallet mnemonic with some funds in it: [Tutorial: Create and Fund a Wallet](tutorial-create-and-fund-a-wallet)
- A Dash Platform identity: [Tutorial: Register an Identity](tutorial-register-an-identity) 
- A name you want to register: [Name restrictions](explanation-dpns#implementation)

# Code
[block:callout]
{
  "type": "success",
  "body": "As of Dash Platform v0.15, an identity may have multiple alias names (`dashAliasIdentityId`) in addition to its default name (`dashUniqueIdentityId`).\n\n*Note*: An identity must have a default name before any aliases can be created for the identity.",
  "title": "Added in Dash Platform v0.15"
}
[/block]
 The examples below demonstrate creating both the default name and alias names.
[block:callout]
{
  "type": "warning",
  "body": "As of v3.14.0 of the Dash JavaScript SDK, the name must be the full domain name including the parent domain (i.e. `myname.dash` instead of just `myname`). Currently `dash` is the only top-level domain that may be used."
}
[/block]
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
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  wallet: {\n    mnemonic: 'a Dash wallet mnemonic with testnet funds goes here',\n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst registerName = async () => {\n  const platform = client.platform;\n\n  const identity = await platform.identities.get('an identity ID goes here');\n  const nameRegistration = await platform.names.register(\n    '<identity name goes here>.dash',\n    { dashUniqueIdentityId: identity.getId() },\n    identity,\n  );\n\n  return nameRegistration;\n};\n\nregisterName()\n  .then((d) => console.log('Name registered:\\n', d.toJSON()))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript",
      "name": "Register Name for Identity"
    },
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  wallet: {\n    mnemonic: 'a Dash wallet mnemonic with evonet funds goes here',\n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst registerAlias = async () => {\n  const platform = client.platform;\n  const identity = await platform.identities.get('an identity ID goes here');\n  const aliasRegistration = await platform.names.register(\n    'a name goes here',\n    { dashAliasIdentityId: identity.getId() },\n    identity,\n  );\n\n  return aliasRegistration;\n};\n\nregisterAlias()\n  .then((d) => console.log('Alias registered:\\n', d.toJSON()))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript",
      "name": "Register Alias for Identity"
    }
  ]
}
[/block]
# What's Happening

After initializing the Client, we fetch the Identity we'll be associating with a name. This is an asynchronous method so we use _await_ to pause until the request is complete. Next, we call `platform.names.register` and pass in the name we want to register, the type of identity record to create, and the identity we just fetched. We wait for the result, and output it to the console.