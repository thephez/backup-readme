# DashJS

Dash client-side JavaScript library for application development and wallet payment/signing. Uses wallet-lib, dapi-client, and dashcore-lib to expose layer-1 and layer-2 functionality. Main user is app developers.

npm: `dash`
Repository: https://github.com/dashevo/DashJS/

# dapi-client
Client library for accessing [DAPI Endpoints](reference-dapi-endpoints) . Enables interaction with Dash platform through the [DAPI](explanation-dapi) hosted on masternodes. Provides automatic masternode discovery starting from any initial masternode.

npm: `@dashevo/dapi-client`
Repository: https://github.com/dashevo/dapi-client

# dapi
A decentralized API for the Dash network. Exposes endpoints for interacting with the layer 1 blockchain and layer 2 platform services.

Repository: https://github.com/dashevo/dapi

# js-dpp
JavaScript implementation of [Dash Platform Protocol](explanation-platform-protocol). Performs validation of all data submitted to the platform.

npm: `@dashevo/dpp`
Repository: https://github.com/dashevo/js-dpp

# js-machine
JavaScript implementation of Dash Platform State Machine built as a Tendermint ABCI application.

Repository: https://github.com/dashevo/js-machine

# Supporting Repositories

## dashpay-contract
Dashpay contract documents JSON Schema

Repository: https://github.com/dashevo/dashpay-contract

## dpns-contract
DPNS contract documents JSON Schema

Repository: https://github.com/dashevo/dpns-contract

## dashcore-lib
A JavaScript Dash library

https://github.com/dashevo/dashcore-lib

## wallet-lib
An extensible JavaScript Wallet Library for Dash. Provides layer 1 SPV wallet functionality.

https://github.com/dashevo/wallet-lib

## js-dpns-client
A JavaScript client for [DPNS](explanation-dpns).

https://github.com/dashevo/js-dpns-client

## dapi-grpc
Decentralized API gRPC definition files and generated clients. Used by clients (e.g. dapi-client) to interact with DAPI endpoints.

https://github.com/dashevo/dapi-grpc

## drive-grpc
Drive gRPC definition files and generated clients. Used by js-machine to interact with Drive endpoints.

https://github.com/dashevo/drive-grpc

## drive
Decentralized application storage on the Dash network.

https://github.com/dashevo/drive

## dash-network-deploy
Tool for assisting Dash devnet network deployment and testing.

https://github.com/dashevo/dash-network-deploy

## dash-network-e2e-tests
Test suite for end-to-end testing of Dash Platform by running some real-life scenarios against a Dash Network.

https://github.com/dashevo/dash-network-e2e-tests

## js-dp-services-ctl
Tool providing a convenient JavaScript interface for configuration and interaction with Dash Platform services. Services are started in Docker containers.

https://github.com/dashevo/js-dp-services-ctl