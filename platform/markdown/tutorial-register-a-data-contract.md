# Overview

In this tutorial we will register a data contract.
[block:callout]
{
  "type": "warning",
  "title": "EvoNet Limitation",
  "body": "Registration of third-party data contracts is not enabled on the initial phase of EvoNet; however, the DPNS contracts is registered. To register data contracts without restriction, please create a devnet."
}
[/block]
## Prerequisites
- [node.js](https://nodejs.org/en/)
- Basic familiarity with JavaScript asychronous functions using [async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
- The DashJS SDK is initialized (covered in [Connecting to EvoNet](tutorial-connecting-to-evonet))

## Steps

First we create the contract:
[block:code]
{
  "codes": [
    {
      "code": "// const sdk = DashJS.sdk( your connection options );\nconst myContract = sdk.platform.contracts.create( /* needs implementation */ );",
      "language": "javascript"
    }
  ]
}
[/block]
Then we broadcast the data contract:
[block:code]
{
  "codes": [
    {
      "code": "const txid = await sdk.platform.contracts.broadcast(myContract);",
      "language": "javascript"
    }
  ]
}
[/block]