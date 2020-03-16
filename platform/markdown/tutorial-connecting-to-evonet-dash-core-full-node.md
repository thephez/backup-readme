[block:callout]
{
  "type": "warning",
  "body": "Since Dash Platform is fully accessible via DAPI, running a full node is unnecessary and offers no particular benefit. Regardless, the steps below provide the necessary information for advanced users to connect.",
  "title": "Advanced Topic"
}
[/block]
# Config File

 The config file shown below may be used to connect a Dash Core node to Evonet. Evonet currently operates using [Dash Core v0.14.0.5](https://github.com/dashpay/dash/releases/tag/v0.14.0.5).
[block:code]
{
  "codes": [
    {
      "code": "devnet=evonet\nport=20001\nsporkaddr=yQuAu9YAMt4yEiXBeDp3q5bKpo7jsC2eEj\n\n# Hard-coded first node\naddnode=evonet.thephez.com:20001\n\nminimumdifficultyblocks=1000\nhighsubsidyblocks=500\nhighsubsidyfactor=10\n",
      "language": "text",
      "name": "dash-evonet.conf"
    }
  ]
}
[/block]
# Starting Dash Core

To start Dash Core and connect to Evonet, simply run dashd or dash-qt with the `conf` parameter set to the configuration file created above: `<path to binary> -conf=<path to configuration file>`
[block:code]
{
  "codes": [
    {
      "code": "dashd -conf=/home/dash/.dashcore/dash-evonet.conf",
      "language": "shell",
      "name": "Start dashd on Evonet"
    }
  ]
}
[/block]