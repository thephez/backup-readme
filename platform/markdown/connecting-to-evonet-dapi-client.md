The purpose of this tutorial is to walk through the steps necessary to access the network.

# Overview
EvoNet is the initial development network provided for experimentation and evaluation of Dash Platform features.

Platform services are provided via a combination of HTTP and gRPC connections to DAPI, and some connections to an Insight API. Although one could interact with DAPI by connecting to these directly, it's easier to use [DAPI-client](https://github.com/dashevo/platform/tree/master/packages/js-dapi-client).

# Connect via DAPI-client

## 1. Install dapi-client
The JavaScript dapi-client package is available from [npmjs.com](https://www.npmjs.com/package/@dashevo/dapi-client) and can be installed by running `npm install`:

```shell
npm install @dashevo/dapi-client
```
## 2. Verify a Successful Response

### a. Create `index.js` and add the following code:

```js
const DAPIClient = require('@dashevo/dapi-client');

let client = new DAPIClient({
  seeds: [{
    service: '18.236.131.253:3000',
    port: 3000
  }],
});

const start = async () => {
  let height = await client.getBestBlockHash();
  console.log('Best block hash:', height);
};

start();
```

### b. Run `index.js`

```shell
node index.js
```

The best block hash will be retrieved and printed to the console.

```text
Best block hash: 
  00000038fd1a1f6f691889d986a563372f3f4887320ad27b635caf4fcc9768c1
```

Once this returns successfully, you're ready to begin developing!