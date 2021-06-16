The purpose of this tutorial is to walk through the steps necessary to set up a masternode with Dash Platform services.

# Prerequisites
- Access to a Linux system (an Ubuntu 20.04 LTS VPS is recommended) configured with a non-root user ([guide](https://docs.dash.org/en/stable/masternodes/setup.html#set-up-your-vps))
- [Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/) (v20.10.0+) and [docker-compose](https://docs.docker.com/compose/install/) (v1.25.0+) installed
- An installation of [NodeJS](https://nodejs.org/en/download/) v12+
- A copy of the [dashmate repository](https://github.com/dashevo/dashmate)
[block:callout]
{
  "type": "info",
  "body": "More comprehensive details of using the dashmate tool can be found in the [dashmate README](https://github.com/dashevo/dashmate/blob/master/README.md)."
}
[/block]
Clone the dashmate repository and prepare for use:
[block:code]
{
  "codes": [
    {
      "code": "git clone -b master https://github.com/dashevo/dashmate.git\ncd dashmate\nnpm ci # optional: install CLI dependencies\nsudo npm link # optional: link CLI for system-wide execution",
      "language": "shell"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Full Platform Node",
  "body": "A full node that with all Platform services can be started by simply running the setup command with the [node type setup parameter](https://github.com/dashevo/dashmate#setup-node) set to  `fullnode` and then starting the node.\n```\ndashmate setup testnet fullnode\ndashmate start\n```"
}
[/block]
# Local Development Network

Dashmate can be used to create a local development network (devnet) containing multiple nodes to mimic conditions and features found in testnet/mainnet settings.

## Setup

Run the following command to start the setup wizard, then accept the default values at each step to create a local devnet:
[block:code]
{
  "codes": [
    {
      "code": "dashmate setup local",
      "language": "shell"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Make a note of the key, identity, and contract ID information displayed during setup as they may be required in the future."
}
[/block]

## Operation

Once the setup completes, start/stop/restart the devnet via the following commands:
[block:code]
{
  "codes": [
    {
      "code": "dashmate group:start\ndashmate group:stop\ndashmate group:restart",
      "language": "shell"
    }
  ]
}
[/block]
The status of the devnet nodes can be check via the group status command:
[block:code]
{
  "codes": [
    {
      "code": "dashmate group:status",
      "language": "shell"
    }
  ]
}
[/block]
## Mining Dash

During development it may be necessary to obtain Dash to create and topup [identities](docs/explanation-identity). This can be done using the dashmate `wallet:mint` command. First obtain an address to fund via the [Create and Fund a Wallet](doc:tutorial-create-and-fund-a-wallet) tutorial and then mine Dash to it as shown below:
[block:code]
{
  "codes": [
    {
      "code": "# Stop the devnet first\ndashmate group:stop\n\n# Mine 10 Dash to a provided address\ndashmate wallet:mint 10 --address=<your address> --config=local_seed\n\n# Restart the devnet\ndashmate group:start",
      "language": "shell",
      "name": "Mine to provided address"
    },
    {
      "code": "# Stop the devnet first\ndashmate group:stop\n\n# Mine 10 Dash to a random address/key\n# The address and private key will be displayed\ndashmate wallet:mint 10 --config=local_seed\n\n# Restart the devnet\ndashmate group:start",
      "language": "shell",
      "name": "Mine to new address"
    }
  ]
}
[/block]
Example output of `dashmate wallet:mint 10 --address=yYqfdpePzn2kWtMxr9nz22HBFM7WBRmAqG --config=local_seed`:
[block:code]
{
  "codes": [
    {
      "code": "✔ Generate 10 dash to address\n  ✔ Start Core\n  ↓ Use specified address yYqfdpePzn2kWtMxr9nz22HBFM7WBRmAqG [SKIPPED]\n  ✔ Generate ≈10 dash to address yYqfdpePzn2kWtMxr9nz22HBFM7WBRmAqG\n    › Generated 172.59038279 dash\n  ✔ Wait for balance to confirm\n  ✔ Stop Core",
      "language": "text"
    }
  ]
}
[/block]
Once the address is funded, you can begin creating identities, data contracts, etc. and experimenting with Dash Platform. To make the Dash SDK connect to your local devnet, use the `dapiAddresses` option when configuring your client:
[block:code]
{
  "codes": [
    {
      "code": "const clientOpts = {\n  dapiAddresses: ['127.0.0.1:3000'],\n  ...\n};\n\nconst client = new Dash.Client(clientOpts);",
      "language": "javascript"
    }
  ]
}
[/block]

# Testnet Masternode Setup
[block:callout]
{
  "type": "danger",
  "body": "Running a masternode requires familiarity with Dash Platform services. Improper configuration may impact testing so please exercise caution if running a masternode.",
  "title": "Advanced Topic"
}
[/block]
To setup a testnet masternode, please refer to the comprehensive documentation of the process as described [here](https://docs.dash.org/en/stable/masternodes/setup-testnet.html#dashmate-installation). The following video also details how to complete the process.
[block:embed]
{
  "html": "<iframe class=\"embedly-embed\" src=\"//cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2FLLiMMXSAfeU%3Ffeature%3Doembed%26rel%3D0&display_name=YouTube&url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DLLiMMXSAfeU&image=https%3A%2F%2Fi.ytimg.com%2Fvi%2FLLiMMXSAfeU%2Fhqdefault.jpg&key=f2aa6fc3595946d0afc3d76cbbd25dc3&type=text%2Fhtml&schema=youtube-nocookie\" width=\"854\" height=\"480\" scrolling=\"no\" title=\"YouTube embed\" frameborder=\"0\" allow=\"autoplay; fullscreen\" allowfullscreen=\"true\"></iframe>",
  "url": "https://www.youtube-nocookie.com/embed/LLiMMXSAfeU?rel=0&wmode=transparent",
  "title": "Introducing Dashmate - testnet masternode setup",
  "favicon": "https://www.youtube-nocookie.com/favicon.ico",
  "image": "https://i.ytimg.com/vi/LLiMMXSAfeU/hqdefault.jpg"
}
[/block]