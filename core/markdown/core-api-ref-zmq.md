# Overview

ZeroMQ is a lightweight wrapper around TCP connections, inter-process communication, and shared-memory, providing various message-oriented semantics such as publish/subscribe, request/reply, and push/pull.

The Dash Core daemon can be configured to act as a trusted "border router", implementing the dash wire protocol and relay, making consensus decisions, maintaining the local blockchain database, broadcasting locally generated transactions into the network, and providing a queryable RPC interface to interact on a polled basis for requesting blockchain related data. However, there exists only a limited service to notify external software of events like the arrival of new blocks or transactions.

The ZeroMQ facility implements a notification interface through a set of specific notifiers. Currently there are notifiers that publish blocks and transactions. This read-only facility requires only the connection of a corresponding ZeroMQ subscriber port in receiving software; it is not authenticated nor is there any two-way protocol involvement. Therefore, subscribers should validate the received data since it may be out of date, incomplete or even invalid.

ZeroMQ sockets are self-connecting and self-healing; that is, connections made between two endpoints will be automatically restored after an outage, and either end may be freely started or stopped in any order.

Because ZeroMQ is message oriented, subscribers receive transactions and blocks all-at-once and do not need to implement any sort of buffering or reassembly.

# Available Notifications

Currently, the following notifications are supported:


| Notification | Description |
| - | - |
| zmqpubhashblock | Block hash |
| zmqpubhashchainlock | Hash of a block with a [ChainLock](core-guide-dash-features-chainlocks) |
| zmqpubhashtx | Transaction hash (TXID) |
| zmqpubhashtxlock | Hash of a transaction receiving and [InstantSend](core-guide-dash-features-instantsend) lock (TXID) |
| zmqpubhashgovernancevote | Governance vote hash |
| zmqpubhashgovernanceobject | Governance object hash |
| zmqpubhashinstantsend<br>doublespend | Hash of a transaction attempting to double-spend an InstantSend-locked input |
| zmqpubrawblock | Raw [`block`](core-ref-p2p-network-data-messages#section-block) |
| zmqpubrawchainlock | Raw [`block`](core-ref-p2p-network-data-messages#section-block) receiving a ChainLock |
| zmqpubrawchainlocksig | Raw [`block`](core-ref-p2p-network-data-messages#section-block) with ChainLock signature ([`clsig`](core-ref-p2p-network-instantsend-messages#section-clsig)) concatenated |
| zmqpubrawtx | Raw transaction ([`tx`](core-ref-transactions-raw-transaction-format))  |
| zmqpubrawtxlock | Raw InstantSend transaction ([`tx`](core-ref-transactions-raw-transaction-format))  |
| zmqpubrawtxlocksig | Raw InstantSend transaction ([`tx`](core-ref-transactions-raw-transaction-format)) with InstantSend lock signature ([`islock`](core-ref-p2p-network-instantsend-messages#section-islock)) concatenated |
| zmqpubrawgovernancevote | Raw governance vote ([`govobjvote`](core-ref-p2p-network-governance-messages#section-govobjvote)) |
| zmqpubrawgovernanceobject | Raw governance object ([`govobject`](core-ref-p2p-network-governance-messages#section-govobj)) |
| zmqpubrawinstantsend<br>doublespend | Raw transaction ([`tx`](core-ref-transactions-raw-transaction-format)) attempting to double-spend an InstantSend-locked input |

# Dash Core Configuration

ZMQ notifications can be enabled via either command line arguments or the configuration file (typically `dash.conf`).

## Command Line

```
$ dashd -zmqpubhashtx=tcp://127.0.0.1:28332 \
        -zmqpubrawtx=ipc:///tmp/dashd.tx.raw
```

## Config File

```
# ZMQ
zmqpubhashtx=tcp://0.0.0.0:28332
zmqpubrawtx=tcp://0.0.0.0:28332
```

# Usage
The socket type is PUB and the address must be a valid ZeroMQ socket address. Each PUB notification has a topic and body, where the header corresponds to the notification type. For instance, for the notification `-zmqpubhashtx` the topic is `hashtx` (no null terminator) and the body is the hexadecimal transaction hash (32 bytes).
[block:callout]
{
  "type": "info",
  "body": "The same address can be used in more than one notification."
}
[/block]

ZeroMQ endpoint specifiers for TCP (and others) are documented in the [ZeroMQ API](http://api.zeromq.org/4-0:_start).

Client side, then, the ZeroMQ subscriber socket must have the `ZMQ_SUBSCRIBE` option set to one or either of these prefixes (for instance, just `hash`); without doing so will result in no messages arriving. Please see the Dash Core repository for a [working example](https://github.com/dashpay/dash/blob/master/contrib/zmq/zmq_sub3.4.py).

# Notes

From the perspective of dashd, the ZeroMQ socket is write-only; PUB sockets don't even have a read function. Thus, there is no state introduced into dashd directly. Furthermore, no information is broadcast that wasn't already received from the public P2P network.

No authentication or authorization is done on connecting clients; it is assumed that the ZeroMQ port is exposed only to trusted entities, using other means such as firewalling.

Note that when the block chain tip changes, a reorganisation may occur and just the tip will be notified. It is up to the subscriber to retrieve the chain from the last known block to the new tip.

There are several possibilities that ZMQ notification can get lost during transmission depending on the communication type your are using. Dashd appends an up-counting sequence number to each notification which allows listeners to detect lost notifications.