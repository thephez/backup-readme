# Overview
In this tutorial we will retrieve the list of identities associated with a specified mnemonic-based account. Since multiple identities may be created using the same mnemonic, it is helpful to have a way to quickly retrieve all these identities (e.g. if importing the mnemonic into a new device).

## Prerequisites
- [node.js](https://nodejs.org/en/) (v12+)
- Basic familiarity with JavaScript asychronous functions using [async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
- The Dash JavaScript SDK is initialized (covered in [Connecting to EvoNet](tutorial-connecting-to-evonet))
- A wallet mnemonic
- A Dash Platform Identity: [Tutorial: Register an Identity](tutorial-register-an-identity) 

# Code
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst client = new Dash.Client({\n  network: 'evonet',\n  wallet: {\n    mnemonic: 'a Dash wallet mnemonic with evonet funds goes here',\n  },\n});\n\nconst retrieveIdentityIds = async () => {\n  const account = await client.getWalletAccount();\n  return account.getIdentityIds();\n};\n\nretrieveIdentityIds()\n  .then((d) => console.log('Mnemonic identities:\\n', d))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
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
      "code": "[\n  '8DpCxc6CY1vHQzQVv8EezN4fpKis5K2YuxGYRSiccsC8',\n  '4SUjmwadgF8LLxnjXchhkNtw7tfgacg82CuZegWktW61'\n]",
      "language": "json"
    }
  ]
}
[/block]
# What's Happening

After we intialize the Client and getting the account, we call `account.getIdentityIds` to retrieve a list of all identities created with the wallet mnemonic. The list of identities is output to the console.