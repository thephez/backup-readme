The purpose of this tutorial is to walk through the steps necessary to access a network.

# Overview

Platform services are provided via a combination of HTTP and gRPC connections to DAPI, and some connections to an Insight API. Although one could interact with DAPI by connecting to these directly, or by using [DAPI-client](https://github.com/dashevo/platform/tree/master/packages/js-dapi-client), the easiest approach is to use the [JavaScript Dash SDK](https://github.com/dashevo/platform/tree/master/packages/js-dash-sdk).

# Prerequisites
- An installation of [NodeJS](https://nodejs.org/en/download/)

> 📘 Minimum Supported Version
>
> Dash Platform only supports NodeJS v12 and higher

# Connect via Dash SDK

## 1. Install the Dash SDK

The JavaScript SDK package is available from npmjs.com and can be installed by running `npm install dash` (from the command line):

```shell
npm install dash
```

## 2. Connect to Dash Platform

Create a file with the following contents. Then run it by typing `node <file.js>` (from the command line):

> 📘 
>
> Currently the Dash SDK connects to Testnet by default.

```javascript
const Dash = require('dash');

const client = new Dash.Client();

async function connect() {
  return await client.getDAPIClient().core.getBestBlockHash();
}

connect()
  .then((d) => console.log('Connected. Best block hash:\n', d))
  .catch((e) => console.error('Something went wrong:\n', e))
  .finally(() => client.disconnect());
```

Once this returns successfully, you're ready to begin developing! For details on all SDK options and methods, please refer to the [SDK documentation](https://github.com/dashevo/platform/tree/master/packages/js-dash-sdk).

# Connect Directly to DAPI (Optional) 

> 🚧 Advanced Topic
>
> Normally, the Dash SDK, dapi-client, or another library should be used to interact with DAPI. This may be helpful for debugging in some cases, but generally is not required.

The example below demonstrates retrieving the hash of the best block hash directly from a DAPI node via command line and several languages:

```shell
curl --request POST \
  --url http://seed-1.testnet.networks.dash.org:3000/ \
  --header 'content-type: application/json' \
  --data '{"method":"getBlockHash","id":1,"jsonrpc":"2.0","params":{"height": 100 }}'
```
```python
import requests

url = "http://seed-1.testnet.networks.dash.org:3000/"

payload = "{\"method\":\"getBlockHash\",\"id\":1,\"jsonrpc\":\"2.0\",\"params\":{\"height\":100}}"
headers = {'content-type': 'application/json'}

response = requests.request("POST", url, data=payload, headers=headers)

print(response.text)
```
```ruby
require 'uri'
require 'net/http'

url = URI("http://seed-1.testnet.networks.dash.org:3000/")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Post.new(url)
request["content-type"] = 'application/json'
request.body = "{\"method\":\"getBlockHash\",\"id\":1,\"jsonrpc\":\"2.0\",\"params\":{\"height\":100}}"

response = http.request(request)
puts response.read_body
```