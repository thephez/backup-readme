[block:callout]
{
  "type": "warning",
  "body": "Since Dash Platform is fully accessible via DAPI, running a full node is unnecessary and offers no particular benefit. Regardless, the steps below provide the necessary information for advanced users to connect.",
  "title": "Advanced Topic"
}
[/block]
# Config File

 The config file shown below may be used to connect a Dash Core node to Testnet. Testnet currently operates using [Dash Core v0.17.0.3](https://github.com/dashpay/dash/releases/tag/v0.17.0.3).
[block:code]
{
  "codes": [
    {
      "code": "testnet=1\n\n# Hard-coded first node\naddnode=seed-1.testnet.networks.dash.org:19999",
      "language": "text",
      "name": "dash-testnet.conf"
    }
  ]
}
[/block]
# Starting Dash Core

To start Dash Core and connect to Testnet, simply run dashd or dash-qt with the `conf` parameter set to the configuration file created above: `<path to binary> -conf=<path to configuration file>`

```shell Start dashd on Testnet
dashd -conf=/home/dash/.dashcore/dash-testnet.conf
```