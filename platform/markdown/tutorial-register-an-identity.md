The purpose of this tutorial is to walk through the steps necessary to register an identity.

# Overview
Identities serve as the basis for interactions with Dash Platform. They consist primarily of a public key used to register a unique entity on the network. Additional details regarding identities can be found in the [Identity description](explanation-identity).

## Prerequisites
- [node.js](https://nodejs.org/en/) (v12+)
- Basic familiarity with JavaScript asychronous functions using [async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
- The Dash JavaScript SDK is initialized (covered in [Connecting to a Network](tutorial-connecting-to-testnet))
- A wallet mnemonic with some funds in it: [How to Create and Fund a Wallet](tutorial-create-and-fund-a-wallet)

# Code
[block:callout]
{
  "type": "info",
  "title": "Wallet Operations",
  "body": "Currently, the JavaScript SDK does not cache wallet information, and therefore, it re-syncs the entire Core chain for some wallet operations (e.g. `client.getWalletAccount()`). This can result in wait times of  5+ minutes. \n\nAn upcoming release will add a persistence feature to cache wallet information during initial sync so that subsequent access is much faster. For now, the `skipSynchronizationBeforeHeight` option can be used to only sync the wallet starting at a certain block height."
}
[/block]

[block:callout]
{
  "type": "danger",
  "title": "Dash Platform v0.18 Breaking Change",
  "body": "The derivation path used for [identities](explanation-identity) has been updated to align with [DIP13](https://github.com/dashpay/dips/blob/master/dip-0013.md#identity-authentication-keys) and provide compatibility with the mobile DashPay wallets. Consequently, identities registered with previous versions of the SDK [will be not be retrievable](https://github.com/dashevo/js-dash-sdk/pull/188).\n\nAlso note the change from `account.getIdentityIds()` to  `account.identities.getIdentityIds()` for retrieving identities ([PR](https://github.com/dashevo/wallet-lib/pull/227))."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  wallet: {\n  \tmnemonic: 'a Dash wallet mnemonic with testnet funds goes here',\n    unsafeOptions: {\n      skipSynchronizationBeforeHeight: 500000, // only sync from mid-2021\n    },\n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst createIdentity = async () => {\n  return client.platform.identities.register();\n};\n\ncreateIdentity()\n  .then((d) => console.log('Identity:\\n', d.toJSON()))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript"
    }
  ]
}
[/block]
The Identity will be output to the console. The Identity will need to have one confirmation before it is accessible via `client.platform.identity.get`.
[block:callout]
{
  "type": "success",
  "body": "**Make a note of the returned identity `id` as it will be used used in subsequent tutorials throughout the documentation.**"
}
[/block]
# What's Happening

After connecting to the Client, we call `platform.identities.register`. This will generate a keypair and submit an _Identity Create State Transaction_. After the Identity is registered, we output it to the console.