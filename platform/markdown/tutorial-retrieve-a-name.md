In this tutorial we will retrieve the name created in the [Register a Name for an Identity tutorial](tutorial-register-a-name-for-an-identity). Additional details regarding identities can be found in the [Identity description](explanation-identity).

## Prerequisites
- [node.js](https://nodejs.org/en/) (v12+)
- Basic familiarity with JavaScript asychronous functions using [async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
- The Dash JavaScript SDK is initialized (covered in [Connecting to EvoNet](tutorial-connecting-to-evonet))

# Code
[block:callout]
{
  "type": "warning",
  "title": "",
  "body": "Note: When querying by name (via `.resolve` or `.search`), the normalized (lowercase) representation of the name must be used (e.g., `mytestuser`, not `MyTestUser`)."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst client = new Dash.Client();\n\nconst retrieveName = async () => {\n  // Retrieve by full name (e.g., myname.dash)\n  // NOTE: Use lowercase characters only\n  return client.platform.names.resolve('<identity name>.dash');\n};\n\nretrieveName()\n  .then((d) => console.log('Name retrieved:\\n', d.toJSON()))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript",
      "name": "JavaScript - Resolve by Name"
    },
    {
      "code": "const Dash = require('dash');\n\nconst client = new Dash.Client();\n\nconst retrieveNameByRecord = async () => {\n  // Retrieve by a name's identity ID\n  return client.platform.names.resolveByRecord(\n    'dashUniqueIdentityId',\n    '<identity id>',\n  );\n};\n\nretrieveNameByRecord()\n  .then((d) => console.log('Name retrieved:\\n', d[0].toJSON()))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript",
      "name": "JavaScript - Revolve by Record"
    },
    {
      "code": "const Dash = require('dash');\n\nconst client = new Dash.Client();\n\nconst retrieveNameBySearch = async () => {\n  // Search for names (e.g. `user*`)\n  // NOTE: Use lowercase characters only\n  return client.platform.names.search('user', 'dash');\n};\n\nretrieveNameBySearch()\n  .then((d) => {\n    for (const name of d) {\n      console.log('Name retrieved:\\n', name.toJSON());\n    }\n  })\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript",
      "name": "JavaScript - Search for Name"
    }
  ]
}
[/block]
# Example Name

The following example response shows a retrieved name (`user-9999.dash`):
[block:code]
{
  "codes": [
    {
      "code": "{\n  '$protocolVersion': 0,\n  '$id': '4veLBZPHDkaCPF9LfZ8fX3JZiS5q5iUVGhdBbaa9ga5E',\n  '$type': 'domain',\n  '$dataContractId': '566vcJkmebVCAb2Dkj2yVMSgGFcsshupnQqtsz1RFbcy',\n  '$ownerId': 'HBNMY5QWuBVKNFLhgBTC1VmpEnscrmqKPMXpnYSHwhfn',\n  '$revision': 1,\n  label: 'user-9999',\n  records: {\n    dashUniqueIdentityId: 'HBNMY5QWuBVKNFLhgBTC1VmpEnscrmqKPMXpnYSHwhfn'\n  },\n  preorderSalt: 'BzQi567XVqc8wYiVHS887sJtL6MDbxLHNnp+UpTFSB0',\n  subdomainRules: { allowSubdomains: false },\n  normalizedLabel: 'user-9999',\n  normalizedParentDomainName: 'dash'\n}",
      "language": "json"
    }
  ]
}
[/block]
# What's Happening

After we initialize the Client, we request a name. The [code examples](#code) demonstrate the three ways to request a name:

1. Resolve by name. The `platform.names.resolve` method takes a single argument: a fully-qualified name (e.g., `user-9999.dash`). 
2. Resolve by record. The `platform.names.resolveByRecord` method takes two arguments: the record type (e.g., `dashUniqueIdentityId`) and the record value to resolve. 
3. Search. The `platform.names.search` method takes two arguments: the leading characters of the name to search for and the domain to search (e.g., `dash` for names in the `*.dash` domain). The search will return names that begin the with string provided in the first parameter.

After the name is retrieved, it is displayed on the console.