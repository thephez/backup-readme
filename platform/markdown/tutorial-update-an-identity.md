# Overview

Since Dash Platform v0.23, it is possible to update identities to add new keys or disable existing ones. 

> 📘
>
> Platform retains disabled keys so that any existing data they signed can still be verified while preventing them from signing new data.

# Prerequisites

- [General prerequisites](tutorials-introduction#prerequisites) (Node.js / Dash SDK installed)
- A wallet mnemonic with some funds in it: [Tutorial: Create and Fund a Wallet](tutorial-create-and-fund-a-wallet)
- A Dash Platform Identity: [Tutorial: Register an Identity](tutorial-register-an-identity)

# Code

The two examples below demonstrate updating an existing identity to add a new key and disabling an existing key:

> 🚧
>
> The current SDK version signs all state transitions with public key id `1`. If it is disabled, the SDK will be unable to use the identity. Future SDK versions will provide a way to also sign using keys added in an identity update.
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  wallet: {\n    mnemonic: 'a Dash wallet mnemonic with funds goes here',\n    unsafeOptions: {\n      skipSynchronizationBeforeHeight: 650000, // only sync from early-2022\n    },    \n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst updateIdentityDisableKey = async () => {\n  const identityId = 'an identity ID goes here';\n  const keyId = 'a public key ID goes here'; // One of the identity's public key IDs\n\n  // Retrieve the identity to be updated and the public key to disable\n  const existingIdentity = await client.platform.identities.get(identityId);\n  const publicKeyToDisable = existingIdentity.getPublicKeyById(keyId);\n\n  const updateDisable = {\n    disable: [publicKeyToDisable],\n  };\n\n  await client.platform.identities.update(existingIdentity, updateDisable);\n  return client.platform.identities.get(identityId);\n}\n\nupdateIdentityDisableKey()\n  .then((d) => console.log('Identity updated:\\n', d.toJSON()))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());\n",
      "language": "javascript",
      "name": "Disable identity key"
    },
    {
      "code": "const Dash = require('dash');\nconst IdentityPublicKey = require('@dashevo/dpp/lib/identity/IdentityPublicKey');\n\nconst clientOpts = {\n  wallet: {\n    mnemonic: 'a Dash wallet mnemonic with funds goes here',\n    unsafeOptions: {\n      skipSynchronizationBeforeHeight: 650000, // only sync from early-2022\n    },    \n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst updateIdentityAddKey = async () => {\n  const identityId = 'an identity ID goes here';\n  const existingIdentity = await client.platform.identities.get(identityId);\n  const newKeyId = existingIdentity.toJSON().publicKeys.length;\n\n  // Get an unused identity index\n  const account = await client.platform.client.getWalletAccount();\n  const identityIndex = await account.getUnusedIdentityIndex();\n\n  // Get unused private key and construct new identity public key\n  const { privateKey: identityPrivateKey } =\n    account.identities.getIdentityHDKeyByIndex(identityIndex, 0);\n\n  const identityPublicKey = identityPrivateKey.toPublicKey().toBuffer();\n\n  const newPublicKey = new IdentityPublicKey({\n    id: newKeyId,\n    type: IdentityPublicKey.TYPES.ECDSA_SECP256K1,\n    purpose: IdentityPublicKey.PURPOSES.AUTHENTICATION,\n    securityLevel: IdentityPublicKey.SECURITY_LEVELS.HIGH,\n    data: identityPublicKey,\n    readOnly: false,\n  });\n\n  const updateAdd = {\n    add: [newPublicKey],\n  };\n\n  // Submit the update signed with the new key\n  await client.platform.identities.update(existingIdentity, updateAdd, {\n    [newPublicKey.getId()]: identityPrivateKey,\n  });\n\n  return client.platform.identities.get(identityId);};\n};\n\nupdateIdentityAddKey()\n  .then((d) => console.log('Identity updated:\\n', d.toJSON()))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());\n",
      "language": "javascript",
      "name": "Add identity key"
    }
  ]
}
[/block]
# What's Happening

## Disabling keys

After we initialize the Client, we retrieve our existing identity and provide the `id` of one (or more) of the identity keys to disable. The update is submitted to DAPI using the `platform.identities.update` method with two arguments:
 1. An identity
 1. An object containing the key(s) to be disabled

Internally, the method creates a State Transition containing the updated identity, signs the state transition, and submits the signed state transition to DAPI. After the identity is updated, we output it to the console.

## Adding keys

After we initialize the Client, we retrieve our existing identity and set an `id` for the key to be added. Next, we get an unused private key from our wallet and use it to derive a public key to add to our identity. The update is submitted to DAPI using the `platform.identities.update` method with three arguments:
 1. An identity
 1. An object containing the key(s) to be added
 1. An object containing the id and private key for each public key being added

> 📘
>
> When adding new public keys, they must be signed using the associated private key to prove ownership of the keys.

Internally, the method creates a State Transition containing the updated identity, signs the state transition, and submits the signed state transition to DAPI. After the identity is updated, we output it to the console.