# Overview
In this tutorial we will retrieve the list of identities associated with a specified mnemonic-based account. Since multiple identities may be created using the same mnemonic, it is helpful to have a way to quickly retrieve all these identities (e.g. if importing the mnemonic into a new device).

## Prerequisites
- [node.js](https://nodejs.org/en/) (v12+)
- Basic familiarity with JavaScript asychronous functions using [async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
- The Dash JavaScript SDK is initialized (covered in [Connecting to a Network](tutorial-connecting-to-testnet))
- A wallet mnemonic
- A Dash Platform Identity: [Tutorial: Register an Identity](tutorial-register-an-identity) 

# Code
[block:callout]
{
  "type": "info",
  "title": "Wallet Operations",
  "body": "Currently, the JavaScript SDK does not cache wallet information, and therefore, it re-syncs the entire Core chain for some wallet operations (e.g. `client.getWalletAccount()`). This can result in wait times of  5+ minutes.\n\nAn upcoming release will add a persistence feature to cache wallet information during initial sync so that subsequent access is much faster. For now, the `skipSynchronizationBeforeHeight` option can be used to only sync the wallet starting at a certain block height."
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Dash Platform 0.18 Breaking Change",
  "body": "The derivation path used for [identities](explanation-identity) was updated to align with [DIP13](https://github.com/dashpay/dips/blob/master/dip-0013.md#identity-authentication-keys) and provide compatibility with the mobile DashPay wallets. Consequently, identities registered with previous versions of the SDK [will be not be retrievable](https://github.com/dashevo/js-dash-sdk/pull/188).\n\nAlso note the change from `account.getIdentityIds()` to  `account.identities.getIdentityIds()` for retrieving identities ([PR](https://github.com/dashevo/wallet-lib/pull/227))."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst client = new Dash.Client({\n  network: 'testnet',\n  wallet: {\n    mnemonic: 'a Dash wallet mnemonic with testnet funds goes here',\n    unsafeOptions: {\n      skipSynchronizationBeforeHeight: 415000, // only sync from start of 2021\n    },\n  },\n});\n\nconst retrieveIdentityIds = async () => {\n  const account = await client.getWalletAccount();\n  return account.identities.getIdentityIds();\n};\n\nretrieveIdentityIds()\n  .then((d) => console.log('Mnemonic identities:\\n', d))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript"
    }
  ]
}
[/block]
Example Response
[block:code]
{
  "codes": [
    {
      "code": "[\n  '6Jz8pFZFhssKSTacgQmZP14zGZNnFYZFKSbx4WVAJFy3',\n  '8XoJHG96Vfm3eGh1A7HiDpMb1Jw2B9opRJe8Z38urapt',\n  'CEPMcuBgAWeaCXiP2gJJaStANRHW6b158UPvL1C8zw2W',\n  'GTGZrkPC72tWeBaqopSCKgiBkVVQR3s3yBsVeMyUrmiY'\n]",
      "language": "json"
    }
  ]
}
[/block]
# What's Happening

After we initialize the Client and getting the account, we call `account.identities.getIdentityIds()` to retrieve a list of all identities created with the wallet mnemonic. The list of identities is output to the console.