The purpose of this tutorial is to walk through the steps necessary to set up a masternode with Dash Platform services using an unreleased development version of the project.

# Prerequisites
- Access to a Linux system configured with a non-root user ([guide](https://docs.dash.org/en/stable/masternodes/setup.html#set-up-your-vps))
- [Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/) (v20.10.0+) and [docker-compose](https://docs.docker.com/compose/install/) (v1.25.0+) installed
- An installation of [NodeJS](https://nodejs.org/en/download/) v12+
- A copy of the [platform repository](https://github.com/dashevo/platform/)

> 📘 
>
> More comprehensive details of using the dashmate tool can be found in the [dashmate README](https://github.com/dashevo/platform/tree/master/packages/dashmate).

Clone a copy of the platform repository:

```shell
git clone -b master https://github.com/dashpay/platform.git
cd platform
```

# Local Development Network

Dashmate can be used to create a local development network (devnet) containing multiple nodes to mimic conditions and features found in testnet/mainnet settings.

## Setup

Run the following command to setup a local network:

```shell
yarn setup
```

## Operation

Once the setup completes, start/stop/restart the network via the following commands:

```shell
yarn start
yarn stop
yarn restart
```

The status of the network's nodes can be check via the group status command:

```shell
yarn exec packages/dashmate/bin/dashmate group:status
```

## Mining Dash

During development it may be necessary to obtain Dash to create and topup [identities](docs/explanation-identity). This can be done using the dashmate `wallet:mint` command. First obtain an address to fund via the [Create and Fund a Wallet](doc:tutorial-create-and-fund-a-wallet) tutorial and then mine Dash to it as shown below:

```shell Mine to provided address
# Stop the devnet first
yarn stop

# Mine 10 Dash to a provided address
yarn exec packages/dashmate/bin/dashmate wallet:mint 10 --address=<your address> --config=local_seed

# Restart the devnet
yarn start
```
```shell Mine to new address
# Stop the devnet first
yarn stop

# Mine 10 Dash to a random address/key
# The address and private key will be displayed
yarn exec packages/dashmate/bin/dashmate wallet:mint 10 --config=local_seed

# Restart the devnet
yarn start
```
Example output of `yarn exec packages/dashmate/bin/dashmate wallet:mint 10 --address=yWTXCoGjH4HDpo5cnGJcBHYFnjncf6gzuL --config=local_seed`:

```text
✔ Generate 10 dash to address
  ✔ Start Core
  ↓ Use specified address yWTXCoGjH4HDpo5cnGJcBHYFnjncf6gzuL [SKIPPED]
  ✔ Generate ≈10 dash to address yWTXCoGjH4HDpo5cnGJcBHYFnjncf6gzuL
    › Generated 172.59038279 dash
  ✔ Wait for balance to confirm
  ✔ Stop Core
```

## Using the devnet

Once the address is funded, you can begin creating identities, data contracts, etc. and experimenting with Dash Platform. The [other tutorials](tutorials-introduction) in this section will help you get started.

To make the Dash SDK connect to your local development network, set the `network` option to `'local'` and set the DPNS contract ID when configuring your client. The DPNS contract ID can be found in the `packages/platform-test-suite/.env` file.

```js
const clientOpts = {
  network: 'local',
  // Set DPNS contract ID to the one output during setup
  apps: {
    dpns: {
      contractId: 'DPNS contract ID displayed during the setup step',
    },
  },
  ...
};

const client = new Dash.Client(clientOpts);
...
```

# Remote Development Network

> 📘 Connecting to a remote development network
>
> In order to connect to a remote devnet (e.g. one run by Dash Core Group), please use one of the methods described in the [Connect to a Devnet](tutorial-connecting-to-testnet#connect-to-a-devnet) section.

For development we recommend using either a local development network created via dashmate as [described above](#local-development-network) or using Testnet. While configuring a remote development network is possible using the Dash network deployment tool, it is beyond the scope of this documentation. For details regarding this tool, please refer to the [GitHub repository](https://github.com/dashevo/dash-network-deploy).


# Testnet Masternode Setup

>❗️ Advanced Topic
>
> Running a masternode requires familiarity with Dash Platform services. Improper configuration may impact testing so please exercise caution if running a masternode.

To setup a testnet masternode, please refer to the comprehensive documentation of the process as described [here](https://docs.dash.org/en/stable/masternodes/setup-testnet.html#dashmate-installation). The following video also details how to complete the process.
[block:html]
{
  "html": "<div></div>\n\n<style></style>\n<iframe width=\"560\" height=\"315\" src=\"https://www.youtube-nocookie.com/embed/LLiMMXSAfeU?rel=0&modestbranding=1\" title=\"YouTube video player\" frameborder=\"0\" allow=\"accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture\" allowfullscreen></iframe>"
}
[/block]