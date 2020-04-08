The purpose of this tutorial is to walk through the steps necessary to register a user identity.

# Overview
Identities serve as the basis for interactions with Dash Platform. They consist primarily of a public key used to register a unique entity on the network. There are two identity types to support different functionality:
 - User identities
 - Application identities

Additional details regarding identities can be found in the [Identity description](explanation-identity).

For this tutorial you will need a wallet mnemonic with some funds in it: [How to Create and Fund a Wallet](tutorial-create-and-fund-a-wallet)

# Code
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  mnemonic: 'a Dash wallet mnemonic with evonet funds goes here',\n};\nconst client = new Dash.Client(clientOpts);\n\nconst createIdentity = async function () {\n  try {\n    await client.isReady();\n    const platform = client.platform;\n    const identity = await platform.identities.register('user');  // literally 'user', do not change\n    console.log({identity});\n  } catch (e) {\n    console.error('Something went wrong:', e);\n  } finally {\n    client.disconnect();\n  }\n}\n\ncreateIdentity()",
      "language": "javascript"
    }
  ]
}
[/block]
The identity ID will be output to the console. The identity will need to have one confirmation before it is accessible via `client.platform.identity.get`.

# What's Happening

After connecting to the Client, we call `platform.identities.register` with the parameter `'user'`, which indicates that we want to create a user identity. This will generate a keypair and submit an _Identity Open Transaction_. After the identity is registered, we output it to the console.