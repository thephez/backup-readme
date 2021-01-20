[block:callout]
{
  "type": "warning",
  "body": "Since Dash Platform is fully accessible via DAPI, running a full node is unnecessary and offers no particular benefit. Regardless, the steps below provide the necessary information for advanced users to connect.",
  "title": "Advanced Topic"
}
[/block]
# Config File

 The config file shown below may be used to connect a Dash Core node to Testnet. Testnet currently operates using [Dash Core v0.17.0.0-RC2](https://github.com/dashpay/dash/tree/v0.17.0.0-rc2). 

The second tab shows the conf file for the previously used Dash development network named Evonet as an example of how a custom devnet could be used.
[block:code]
{
  "codes": [
    {
      "code": "testnet=1\n\n# Hard-coded first node\naddnode=seed-1.testnet.networks.dash.org:19999",
      "language": "text",
      "name": "dash-testnet.conf"
    },
    {
      "code": "devnet=evonet-8\n\n[devnet]\n# The [devnet] section heading is required as of Dash Core 0.16\nport=20001\nsporkaddr=yQuAu9YAMt4yEiXBeDp3q5bKpo7jsC2eEj\n\n# Hard-coded first node\naddnode=seed-1.evonet.networks.dash.org:20001\n\nminimumdifficultyblocks=1000\nhighsubsidyblocks=500\nhighsubsidyfactor=10\n",
      "language": "text",
      "name": "dash-evonet.conf"
    }
  ]
}
[/block]
# Starting Dash Core

To start Dash Core and connect to Testnet, simply run dashd or dash-qt with the `conf` parameter set to the full path of the configuration file created above: `<path to binary> -conf=<path to configuration file>`

```shell Start dashd on Testnet
dashd -conf=/home/dash/.dashcore/dash-testnet.conf
```