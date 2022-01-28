# Usage

**Start Dash Core Daemon**
```bash 
dashd [options]
```
[block:callout]
{
  "type": "warning",
  "title": "Debug Options",
  "body": "The following sections show all available options including debug options that are not normally displayed. To see only regular options, run `dashd --help`."
}
[/block]
## Options:

```text
  -?
       Print this help message and exit

  -alertnotify=<cmd>
       Execute command when a relevant alert is received or we see a really
       long fork (%s in cmd is replaced by message)

  -assumevalid=<hex>
       If this block is in the chain assume that it and its ancestors are valid
       and potentially skip their script verification (0 to verify all,
       default:
       00000000000000022f355c52417fca9b73306958f7c0832b3a7bce006ca369ef,
       testnet:
       000008b78b6aef3fd05ab78db8b76c02163e885305545144420cb08704dce538)

  -blocksdir=<dir>
       Specify blocks directory (default: <datadir>/blocks)

  -blocknotify=<cmd>
       Execute command when the best block changes (%s in cmd is replaced by
       block hash)

  -blockreconstructionextratxn=<n>
       Extra transactions to keep in memory for compact block reconstructions
       (default: 100)

  -blocksonly
       Whether to operate in a blocks only mode (default: 0)

  -conf=<file>
       Specify configuration file. Relative paths will be prefixed by datadir
       location. (default: dash.conf)

  -daemon
       Run in the background as a daemon and accept commands

  -datadir=<dir>
       Specify data directory

  -dbbatchsize
       Maximum database write batch size in bytes (default: 16777216)

  -dbcache=<n>
       Set database cache size in megabytes (4 to 16384, default: 300)

  -debuglogfile=<file>
       Specify location of debug log file. Relative paths will be prefixed by a
       net-specific datadir location. (0 to disable; default: debug.log)

  -loadblock=<file>
       Imports blocks from external blk000??.dat file on startup

  -maxmempool=<n>
       Keep the transaction memory pool below <n> megabytes (default: 300)

  -maxorphantxsize=<n>
       Maximum total size of all orphan transactions in megabytes (default: 10)

  -maxrecsigsage=<n>
       Number of seconds to keep LLMQ recovery sigs (default: 604800) 

  -mempoolexpiry=<n>
       Do not keep transactions in the mempool longer than <n> hours (default:
       336)

  -minimumchainwork=<hex>
       Minimum work assumed to exist on a valid chain in hex (default:
       0000000000000000000000000000000000000000000044f363f417890207722e,
       testnet:
       000000000000000000000000000000000000000000000000022f13324cfe06a3)

  -par=<n>
       Set the number of script verification threads (-8 to 16, 0 = auto, <0 =
       leave that many cores free, default: 0)

  -persistmempool
       Whether to save the mempool on shutdown and load on restart (default: 1)

  -pid=<file>
       Specify pid file. Relative paths will be prefixed by a net-specific
       datadir location. (default: dashd.pid)

  -prune=<n>
       Reduce storage requirements by enabling pruning (deleting) of old
       blocks. This allows the pruneblockchain RPC to be called to
       delete specific blocks, and enables automatic pruning of old
       blocks if a target size in MiB is provided. This mode is
       incompatible with -txindex, -rescan and -disablegovernance=false.
       Warning: Reverting this setting requires re-downloading the
       entire blockchain. (default: 0 = disable pruning blocks, 1 =
       allow manual pruning via RPC, >=945 = automatically prune block
       files to stay under the specified target size in MiB)

  -syncmempool
       Sync mempool from other nodes on start (default: 1)

  -sysperms
       Create new files with system default permissions, instead of umask 077
       (only effective with disabled wallet functionality)

  -version
       Print version and exit
```

## Indexing options:

```text
  -addressindex
       Maintain a full address index, used to query for the balance, txids and
       unspent outputs for addresses (default: 0)

  -reindex
       Rebuild chain state and block index from the blk*.dat files on disk

  -reindex-chainstate
       Rebuild chain state from the currently indexed blocks

  -spentindex
       Maintain a full spent index, used to query the spending txid and input
       index for an outpoint (default: 0)

  -timestampindex
       Maintain a timestamp index for block hashes, used to query blocks hashes
       by a range of timestamps (default: 0)

  -txindex
       Maintain a full transaction index, used by the getrawtransaction rpc
       call (default: 1)
```

## Connection options:

```text
  -addnode=<ip>
       Add a node to connect to and attempt to keep the connection open (see
       the `addnode` RPC command help for more info). This option can be
       specified multiple times to add multiple nodes.

  -allowprivatenet
       Allow RFC1918 addresses to be relayed and connected to (default: 0)

  -banscore=<n>
       Threshold for disconnecting misbehaving peers (default: 100)

  -bantime=<n>
       Number of seconds to keep misbehaving peers from reconnecting (default:
       86400)

  -bind=<addr>
       Bind to given address and always listen on it. Use [host]:port notation
       for IPv6

  -connect=<ip>
       Connect only to the specified node; -connect=0 disables automatic
       connections (the rules for this peer are the same as for
       -addnode). This option can be specified multiple times to connect
       to multiple nodes.

  -discover
       Discover own IP addresses (default: 1 when listening and no -externalip
       or -proxy)

  -dns
       Allow DNS lookups for -addnode, -seednode and -connect (default: 1)

  -dnsseed
       Query for peer addresses via DNS lookup, if low on addresses (default: 1
       unless -connect used)

  -enablebip61
       Send reject messages per BIP61 (default: 1)

  -externalip=<ip>
       Specify your own public address

  -forcednsseed
       Always query for peer addresses via DNS lookup (default: 0)

  -listen
       Accept connections from outside (default: 1 if no -proxy or -connect)

  -listenonion
       Automatically create Tor hidden service (default: 1)

  -maxconnections=<n>
       Maintain at most <n> connections to peers (temporary service connections
       excluded) (default: 125)

  -maxreceivebuffer=<n>
       Maximum per-connection receive buffer, <n>*1000 bytes (default: 5000)

  -maxsendbuffer=<n>
       Maximum per-connection send buffer, <n>*1000 bytes (default: 1000)

  -maxtimeadjustment
       Maximum allowed median peer time offset adjustment. Local perspective of
       time may be influenced by peers forward or backward by this
       amount. (default: 4200 seconds)

  -maxuploadtarget=<n>
       Tries to keep outbound traffic under the given target (in MiB per 24h),
       0 = no limit (default: 0)

  -onion=<ip:port>
       Use separate SOCKS5 proxy to reach peers via Tor hidden services
       (default: -proxy)

  -onlynet=<net>
       Make outgoing connections only through network <net> (ipv4, ipv6 or
       onion). Incoming connections are not affected by this option.
       This option can be specified multiple times to allow multiple
       networks.

  -peerbloomfilters
       Support filtering of blocks and transaction with bloom filters (default:
       1)

  -peertimeout=<n>
       Specify p2p connection timeout in seconds. This option determines the
       amount of time a peer may be inactive before the connection to it
       is dropped. (minimum: 1, default: 60)

  -permitbaremultisig
       Relay non-P2SH multisig (default: 1)

  -port=<port>
       Listen for connections on <port> (default: 9999 or testnet: 19999)

  -proxy=<ip:port>
       Connect through SOCKS5 proxy

  -proxyrandomize
       Randomize credentials for every proxy connection. This enables Tor
       stream isolation (default: 1)

  -seednode=<ip>
       Connect to a node to retrieve peer addresses, and disconnect. This
       option can be specified multiple times to connect to multiple
       nodes.

  -socketevents=<mode>
       Socket events mode, which must be one of 'select', 'poll', 'epoll' or
       'kqueue', depending on your system (default: Linux - 'epoll',
       FreeBSD/Apple - 'kqueue', Windows - 'select')

  -timeout=<n>
       Specify connection timeout in milliseconds (minimum: 1, default: 5000)

  -torcontrol=<ip>:<port>
       Tor control port to use if onion listening enabled (default:
       127.0.0.1:9051)

  -torpassword=<pass>
       Tor control port password (default: empty)

  -upnp
       Use UPnP to map the listening port (default: 0)

  -whitebind=<addr>
       Bind to given address and whitelist peers connecting to it. Use
       [host]:port notation for IPv6

  -whitelist=<IP address or network>
       Whitelist peers connecting from the given IP address (e.g. 1.2.3.4) or
       CIDR notated network (e.g. 1.2.3.0/24). Can be specified multiple
       times. Whitelisted peers cannot be DoS banned and their
       transactions are always relayed, even if they are already in the
       mempool, useful e.g. for a gateway
```

## Wallet options:

```text
  -createwalletbackups=<n>
       Number of automatic wallet backups (default: 10)

  -disablewallet
       Do not load the wallet and disable wallet RPC calls

  -instantsendnotify=<cmd>
       Execute command when a wallet InstantSend transaction is successfully
       locked (%s in cmd is replaced by TxID)

  -keypool=<n>
       Set key pool size to <n> (default: 1000)

  -rescan=<mode>
       Rescan the block chain for missing wallet transactions on startup (1 =
       start from wallet creation time, 2 = start from genesis block)

  -salvagewallet
       Attempt to recover private keys from a corrupt wallet on startup

  -spendzeroconfchange
       Spend unconfirmed change when sending transactions (default: 1)

  -upgradewallet
       Upgrade wallet to latest format on startup

  -wallet=<path>
       Specify wallet database path. Can be specified multiple times to load
       multiple wallets. Path is interpreted relative to <walletdir> if
       it is not absolute, and will be created if it does not exist (as
       a directory containing a wallet.dat file and log files). For
       backwards compatibility this will also accept names of existing
       data files in <walletdir>.)

  -walletbroadcast
       Make the wallet broadcast transactions (default: 1)

  -walletdir=<dir>
       Specify directory to hold wallets (default: <datadir>/wallets if it
       exists, otherwise <datadir>)

  -walletnotify=<cmd>
       Execute command when a wallet transaction changes (%s in cmd is replaced
       by TxID)

  -zapwallettxes=<mode>
       Delete all wallet transactions and only recover those parts of the
       blockchain through -rescan on startup (1 = keep tx meta data e.g.
       account owner and payment request information, 2 = drop tx meta
       data)

  -walletbackupsdir=<dir>
       Specify full path to directory for automatic wallet backups (must exist)
```

## Wallet fee options:

```text
  -discardfee=<amt>
       The fee rate (in DASH/kB) that indicates your tolerance for discarding
       change by adding it to the fee (default: 0.0001). Note: An output
       is discarded if it is dust at this rate, but we will always
       discard up to the dust relay fee and a discard fee above that is
       limited by the fee estimate for the longest target

  -fallbackfee=<amt>
       A fee rate (in DASH/kB) that will be used when fee estimation has
       insufficient data (default: 0.00001)

  -mintxfee=<amt>
       Fees (in DASH/kB) smaller than this are considered zero fee for
       transaction creation (default: 0.00001)

  -paytxfee=<amt>
       Fee (in DASH/kB) to add to transactions you send (default: 0.00)

  -txconfirmtarget=<n>
       If paytxfee is not set, include enough fee so transactions begin
       confirmation on average within n blocks (default: 6)
```

## HD wallet options:

```text
  -hdseed=<hex>
       User defined seed for HD wallet (should be in hex). Only has effect
       during wallet creation/first start (default: randomly generated)

  -mnemonic=<text>
       User defined mnemonic for HD wallet (bip39). Only has effect during
       wallet creation/first start (default: randomly generated)

  -mnemonicpassphrase=<text>
       User defined mnemonic passphrase for HD wallet (BIP39). Only has effect
       during wallet creation/first start (default: empty string)

  -usehd
       Use hierarchical deterministic key generation (HD) after BIP39/BIP44.
       Only has effect during wallet creation/first start (default: 0)
```

## CoinJoin options:

```
  -coinjoinamount=<n>
       Target CoinJoin balance (2-21000000, default: 1000)

  -coinjoinautostart
       Start CoinJoin automatically (0-1, default: 0)

  -coinjoindenomsgoal=<n>
       Try to create at least N inputs of each denominated amount (10-100000,
       default: 50)

  -coinjoindenomshardcap=<n>
       Create up to N inputs of each denominated amount (10-100000, default:
       300)

  -coinjoinmultisession
       Enable multiple CoinJoin mixing sessions per block, experimental
       (0-1, default: 0)

  -coinjoinrounds=<n>
       Use N separate masternodes for each denominated input to mix funds
       (2-16, default: 4)

  -coinjoinsessions=<n>
       Use N separate masternodes in parallel to mix funds (1-10, default: 4)

  -enablecoinjoin
       Enable use of CoinJoin for funds stored in this wallet (0-1, default: 0)

```

## ZeroMQ notification options:

```
  -zmqpubhashblock=<address>
       Enable publish hash block in <address>

  -zmqpubhashgovernanceobject=<address>
       Enable publish hash of governance objects (like proposals) in <address>

  -zmqpubhashgovernancevote=<address>
       Enable publish hash of governance votes in <address>

  -zmqpubhashinstantsenddoublespend=<address>
       Enable publish transaction hashes of attempted InstantSend double spend
       in <address>

  -zmqpubhashrecoveredsig=<address>
       Enable publish message hash of recovered signatures (recovered by LLMQs)
       in <address>

  -zmqpubhashtx=<address>
       Enable publish hash transaction in <address>

  -zmqpubhashtxlock=<address>
       Enable publish hash transaction (locked via InstantSend) in <address>

  -zmqpubrawblock=<address>
       Enable publish raw block in <address>

  -zmqpubrawinstantsenddoublespend=<address>
       Enable publish raw transactions of attempted InstantSend double spend in
       <address>

  -zmqpubrawrecoveredsig=<address>
       Enable publish raw recovered signatures (recovered by LLMQs) in
       <address>

  -zmqpubrawtx=<address>
       Enable publish raw transaction in <address>

  -zmqpubrawtxlock=<address>
       Enable publish raw transaction (locked via InstantSend) in <address>
```

## Debugging/Testing options:

```
  -checkblockindex
       Do a full consistency check for mapBlockIndex, setBlockIndexCandidates,
       chainActive and mapBlocksUnlinked occasionally. (default: 0)

  -checkblocks=<n>
       How many blocks to check at startup (default: 6, 0 = all)

  -checklevel=<n>
       How thorough the block verification of -checkblocks is (0-4, default: 3)

  -checkmempool=<n>
       Run checks every <n> transactions (default: 0)

  -checkpoints
       Disable expensive verification for known chain history (default: 1)

  -deprecatedrpc=<method>
       Allows deprecated RPC method(s) to be used

  -dropmessagestest=<n>
       Randomly drop 1 of every <n> network messages

  -limitancestorcount=<n>
       Do not accept transactions if number of in-mempool ancestors is <n> or
       more (default: 25)

  -limitancestorsize=<n>
       Do not accept transactions whose size with all in-mempool ancestors
       exceeds <n> kilobytes (default: 101)

  -limitdescendantcount=<n>
       Do not accept transactions if any ancestor would have <n> or more
       in-mempool descendants (default: 25)

  -limitdescendantsize=<n>
       Do not accept transactions if any ancestor would have more than <n>
       kilobytes of in-mempool descendants (default: 101).

  -stopafterblockimport
       Stop running after importing blocks from disk (default: 0)

  -stopatheight
       Stop running after reaching the given height in the main chain (default:
       0)

  -vbparams=<deployment>:<start>:<end>(:<window>:<threshold>)
       Use given start/end times for specified version bits deployment
       (regtest-only). Specifying window and threshold is optional.

  -watchquorums=<n>
       Watch and validate quorum communication (default: 0)

  -addrmantest
       Allows to test address relay on localhost

  -debug=<category>
       Output debugging information (default: 0, supplying <category> is
       optional). If <category> is not supplied or if <category> = 1,
       output all debugging information. <category> can be: net, tor,
       mempool, http, bench, zmq, db, rpc, estimatefee, addrman,
       selectcoins, reindex, cmpctblock, rand, prune, proxy, mempoolrej,
       libevent, coindb, qt, leveldb, chainlocks, gobject, instantsend,
       keepass, llmq, llmq-dkg, llmq-sigs, mnpayments, mnsync,
       privatesend, spork, netconn.

  -debugexclude=<category>
       Exclude debugging information for a category. Can be used in conjunction
       with -debug=1 to output debug logs for all categories except one
       or more specified categories.

  -disablegovernance
       Disable governance validation (0-1, default: 0)

  -help-debug
       Show all debugging options (usage: --help -help-debug)

  -logips
       Include IP addresses in debug output (default: 0)

  -logthreadnames
       Add thread names to debug messages (default: 0)

  -logtimemicros
       Add microsecond precision to debug timestamps (default: 0)

  -maxsigcachesize=<n>
       Limit sum of signature cache and script execution cache sizes to <n> MiB
       (default: 32)

  -maxtipage=<n>
       Maximum tip age in seconds to consider node in initial block download
       (default: 21600)

  -mocktime=<n>
       Replace actual time with <n> seconds since epoch (default: 0)

  -logtimestamps
       Prepend debug output with timestamp (default: 1)

  -maxtxfee=<amt>
       Maximum total fees (in DASH) to use in a single wallet transaction or
       raw transaction; setting this too low may abort large
       transactions (default: 0.10)

  -minsporkkeys=<n>
       Overrides minimum spork signers to change spork value. Only useful for
       regtest and devnet. Using this on mainnet or testnet will ban
       you.

  -printpriority
       Log transaction fee per kB when mining blocks (default: 0)

  -printtoconsole
       Send trace/debug info to console instead of debug.log file

  -printtodebuglog
       Send trace/debug info to debug.log file (default: 1)

  -shrinkdebugfile
       Shrink debug.log file on client startup (default: 1 when no -debug)

  -sporkaddr=<dashaddress>
       Override spork address. Only useful for regtest and devnet. Using this
       on mainnet or testnet will ban you.

  -uacomment=<cmt>
       Append comment to the user agent string
```

## Chain selection options:

```
  -devnet=<name>
       Use devnet chain with provided name

  -regtest
       Enter regression test mode, which uses a special chain in which blocks
       can be solved instantly. This is intended for regression testing
       tools and app development.

  -testnet
       Use the test chain
```

## Masternode options:

```
  -masternodeblsprivkey=<hex>
       Set the masternode BLS private key and enable the client to act as a
       masternode

  -platform-user=<user>
       Set the username for the "platform user", a restricted user intended to
       be used by Dash Platform, to the specified username.
```

## Node relay options:

```
  -acceptnonstdtxn
       Relay and mine "non-standard" transactions (testnet/regtest only;
       default: 1)

  -dustrelayfee=<amt>
       Fee rate (in DASH/kB) used to defined dust, the value of an output such
       that it will cost more than its value in fees at this fee rate to
       spend it. (default: 0.00003)

  -incrementalrelayfee=<amt>
       Fee rate (in DASH/kB) used to define cost of relay, used for mempool
       limiting and BIP 125 replacement. (default: 0.00001)

  -bytespersigop
       Minimum bytes per sigop in transactions we relay and mine (default: 20)

  -datacarrier
       Relay and mine data carrier transactions (default: 1)

  -datacarriersize
       Maximum size of data in data carrier transactions we relay and mine
       (default: 83)

  -minrelaytxfee=<amt>
       Fees (in DASH/kB) smaller than this are considered zero fee for
       relaying, mining and transaction creation (default: 0.00001)

  -whitelistforcerelay
       Force relay of transactions from whitelisted peers even if they violate
       local relay policy (default: 1)

  -whitelistrelay
       Accept relayed transactions received from whitelisted peers even when
       not relaying transactions (default: 1)
```

## Block creation options:

```
  -blockmaxsize=<n>
       Set maximum block size in bytes (default: 2000000)

  -blockmintxfee=<amt>
       Set lowest fee rate (in DASH/kB) for transactions to be included in
       block creation. (default: 0.00001)

  -blockversion=<n>
       Override block version to test forking scenarios
```

## RPC server options:

```
  -rest
       Accept public REST requests (default: 0)

  -rpcallowip=<ip>
       Allow JSON-RPC connections from specified source. Valid for <ip> are a
       single IP (e.g. 1.2.3.4), a network/netmask (e.g.
       1.2.3.4/255.255.255.0) or a network/CIDR (e.g. 1.2.3.4/24). This
       option can be specified multiple times

  -rpcauth=<userpw>
       Username and hashed password for JSON-RPC connections. The field
       <userpw> comes in the format: <USERNAME>:<SALT>$<HASH>. A
       canonical python script is included in share/rpcauth. The client
       then connects normally using the
       rpcuser=<USERNAME>/rpcpassword=<PASSWORD> pair of arguments. This
       option can be specified multiple times

  -rpcbind=<addr>[:port]
       Bind to given address to listen for JSON-RPC connections. Do not expose
       the RPC server to untrusted networks such as the public internet!
       This option is ignored unless -rpcallowip is also passed. Port is
       optional and overrides -rpcport. Use [host]:port notation for
       IPv6. This option can be specified multiple times (default:
       127.0.0.1 and ::1 i.e., localhost, or if -rpcallowip has been
       specified, 0.0.0.0 and :: i.e., all addresses)

  -rpccookiefile=<loc>
       Location of the auth cookie. Relative paths will be prefixed by a
       net-specific datadir location. (default: data dir)

  -rpcpassword=<pw>
       Password for JSON-RPC connections

  -rpcport=<port>
       Listen for JSON-RPC connections on <port> (default: 9998 or testnet:
       19998)

  -rpcservertimeout=<n>
       Timeout during HTTP requests (default: 30)

  -rpcworkqueue=<n>
       Set the depth of the work queue to service RPC calls (default: 16)

  -rpcthreads=<n>
       Set the number of threads to service RPC calls (default: 4)

  -rpcuser=<user>
       Username for JSON-RPC connections

  -server
       Accept command line and JSON-RPC commands
```

## Statsd options:

```text
  -statsenabled
       Publish internal stats to statsd (default: 0)

  -statshost=<ip>
       Specify statsd host (default: 127.0.0.1)

  -statshostname=<ip>
       Specify statsd host name (default: )

  -statsport=<port>
       Specify statsd port (default: 8125)

  -statsns=<ns>
       Specify additional namespace prefix (default: )

  -statsperiod=<seconds>
       Specify the number of seconds between periodic measurements (default:
       60)
```

## Wallet debugging/testing options:
[block:callout]
{
  "type": "warning",
  "body": "These options are normally hidden and will only be shown if using the help debug option: `dashd --held -help-debug`"
}
[/block]
```text
  -dblogsize=<n>
       Flush wallet database activity from memory to disk log every <n>
       megabytes (default: 100)

  -flushwallet
       Run a thread to flush wallet periodically (default: 1)

  -privdb
       Sets the DB_PRIVATE flag in the wallet db environment (default: 1)

  -walletrejectlongchains
       Wallet will not create transactions that violate mempool chain limits
       (default: 0)
```

## Quorum Recovery Options

```text
  -llmq-data-recovery=<n>
       Enable automated quorum data recovery (default: 1)

  -llmq-qvvec-sync=<quorum_name:mode>
       Defines from which LLMQ type the masternode should sync quorum
       verification vectors. Can be used multiple times with different
       LLMQ types. Mode: 0 (sync always from all quorums of the type
       defined by "quorum_name"), 1 (sync from all quorums of the type
       defined by "quorum_name" if member of any of the quorums)
```

# Network Dependent Options

The following options can only be used for specific network types. These options are provided support development (devnet) and regression test (regtest) networks.

## Devnet Options

```text
  -minimumdifficultyblocks=<number of blocks>
       The number of blocks that can be mined with the minimum difficulty at the
       start of the devnet

  -highsubsidyblocks=<number of blocks>
       The number of blocks with a higher than normal subsidy to mine at the start
       of the devnet

  -highsubsidyfactor=<subsidy multiplication value>
       The factor to multiply the normal block subsidy by while in the
       highsubsidyblocks window

  -llmqchainlocks=<quorum name>
       Override the default LLMQ type used for ChainLocks on the devnet. 
       Allows using ChainLocks with smaller LLMQs.

  -llmqdevnetparams=<size:threshold>
       Override the default LLMQ size for the LLMQ_DEVNET quorum

  -llmqinstantsend=<quorum name>
       Override the default LLMQ type used for InstantSend on the devnet.
       Allows using InstantSend with smaller LLMQs.
```

The quorum names used in these options are:

| LLMQ Type | LLMQ Name |
| :-: | - |
| 1 | llmq50_60 |
| 2 | llmq400_60 |
| 3 | llmq400_85 |
| 4 | llmq100_67 |
| 100 | llmq_test |
| 101 | llmq_devnet |

Refer to [this table in DIP-6 - LLMQs](https://github.com/dashpay/dips/blob/master/dip-0006.md#current-llmq-types) for details on each quorum type.

## Regtest Options

```
  -llmqtestparams=<size:threshold>
              Override the default LLMQ size for the LLMQ_TEST quorum       
```