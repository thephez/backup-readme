The purpose of this tutorial is to walk through the steps necessary to register an identity.

# Overview
Identities serve as the basis for interactions with Dash Platform. They consist primarily of a public key used to register a unique entity on the network. Additional details regarding identities can be found in the [Identity description](explanation-identity).

## Prerequisites
- [node.js](https://nodejs.org/en/) (v12+)
- Basic familiarity with JavaScript asychronous functions using [async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
- The Dash JavaScript SDK is initialized (covered in [Connecting to EvoNet](tutorial-connecting-to-evonet))
- A wallet mnemonic with some funds in it: [How to Create and Fund a Wallet](tutorial-create-and-fund-a-wallet)

# Code
[block:callout]
{
  "type": "warning",
  "body": "Early versions of Dash Platform did not check if a public key was already used when creating an identity. As of Dash Platform Protocol v0.13, validation ensures that a public key can only be associated with a single identity.",
  "title": "Identity public key reuse"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'evonet',\n  wallet: {\n  \tmnemonic: 'a Dash wallet mnemonic with evonet funds goes here',\n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst createIdentity = async () => {\n  return client.platform.identities.register();\n};\n\ncreateIdentity()\n  .then((d) => console.log('Identity:\\n', d.toJSON()))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript"
    }
  ]
}
[/block]
The Identity will be output to the console. The Identity will need to have one confirmation before it is accessible via `client.platform.identity.get`.

# What's Happening

After connecting to the Client, we call `platform.identities.register`. This will generate a keypair and submit an _Identity Open Transaction_. After the Identity is registered, we output it to the console.