The purpose of this tutorial is to walk through the steps necessary to register a [Dash Platform Name Service (DPNS)](reference-glossary#section-dash-platform-naming-service-dpns) name.

# Overview
Dash platform names make cryptographic identities easy to remember and communicate. 

Additional details regarding identities can be found in the [Identity description](explanation-identity).

For this tutorial you will need:

- A wallet mnemonic with some funds in it: [Tutorial: Create and Fund a Wallet](tutorial-create-and-fund-a-wallet)
- A dash platform identity: [Tutorial: Register an Identity](tutorial-register-an-identity) 
- A name you want to register. [Name restrictions](explanation-dpns#section-implementation)

# Code
[block:code]
{
  "codes": [
    {
      "code": "const DashJS = require('dash');\n\nconst sdkOpts = {\n  network: 'testnet',\n  mnemonic: 'a Dash wallet mnemonic with evonet funds goes here',\n};\nconst sdk = new DashJS.SDK(sdkOpts);\n\nconst registerName = async function () {\n  try {\n    await sdk.isReady();\n    const platform = sdk.platform;\n    const identity = await platform.identities.get('an identity ID goes here');\n    const nameRegistration = await platform.names.register('a name goes here', identity);\n    console.log({nameRegistration});\n  } catch (e) {\n    console.error('Something went wrong:', e);\n  } finally {\n    sdk.disconnect();\n  }\n}\n\nregisterName();",
      "language": "javascript"
    }
  ]
}
[/block]
# What's Happening

After initializing the SDK, we fetch the identity we'll be associating with a name. This is an asynchronous method so we use _await_ to pause until the request is complete. Next, we call `platform.names.register` and pass in the name we want to register, and the identity we just fetched. We wait for the result, and output it to the console.
[block:callout]
{
  "type": "info",
  "body": "NOTE: 09/Jan/2020: The call to platform.names.register() sends a lot of info to the console because of some `console.dir` statements in the source code.  These will be removed soon."
}
[/block]