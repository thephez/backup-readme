[block:callout]
{
  "type": "danger",
  "body": "RPCs that require wallet support are **not available on masternodes** for security reasons. Such RPCs are designated with a \"_Requires wallet support_\" message.",
  "title": "Wallet Support"
}
[/block]
# AbandonTransaction

*Added in Bitcoin Core 0.12.0*

The [`abandontransaction` RPC](core-api-ref-remote-procedure-calls-wallet#abandontransaction) marks an in-wallet transaction and all its in-wallet descendants as abandoned. This allows their inputs to be respent.

*Parameter #1---a transaction identifier (TXID)*

Name | Type | Presence | Description
--- | --- | --- | ---
TXID | string (hex) | Required<br>(exactly 1) | The TXID of the transaction that you want to abandon.  The TXID must be encoded as hex in RPC byte order

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | JSON `null` when the transaction and all descendants were abandoned

*Example from Dash Core 0.12.2*

Abandons the transaction on your node.

```bash
dash-cli abandontransaction fa3970c341c9f5de6ab13f128cbfec58d732e736a505fe32137ad551c799ecc4
```

Result (no output from `dash-cli` because result is set to `null`).

*See also*

* [SendRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transactions#sendrawtransaction): validates a transaction and broadcasts it to the peer-to-peer network.

# AbortRescan

The [`abortrescan` RPC](core-api-ref-remote-procedure-calls-wallet#abortrescan) Stops current wallet rescan

Stops current wallet rescan triggered e.g. by an [`importprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#importprivkey) call.

*Parameters: none*

*Result---`true` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | `true` when the command was successful or `false` if not successful.

*Example from Dash Core 0.15.0*

Abort the running wallet rescan

```bash
dash-cli -testnet abortrescan
```

Result:
```text
true
```

*See also: none*

# AddMultiSigAddress
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]

The [`addmultisigaddress` RPC](core-api-ref-remote-procedure-calls-wallet#addmultisigaddress) adds a P2SH multisig address to the wallet.

*Parameter #1---the number of signatures required*

Name | Type | Presence | Description
--- | --- | --- | ---
Required | number (int) | Required<br>(exactly 1) | The minimum (*m*) number of signatures required to spend this m-of-n multisig script

*Parameter #2---the full public keys, or addresses for known public keys*

Name | Type | Presence | Description
--- | --- | --- | ---
Keys Or Addresses | array | Required<br>(exactly 1) | An array of strings with each string being a public key or address
→<br>Key Or Address | string | Required<br>(1 or more) | A public key against which signatures will be checked.  Alternatively, this may be a P2PKH address belonging to the wallet---the corresponding public key will be substituted.  There must be at least as many keys as specified by the Required parameter, and there may be more keys

*Parameter #3---label*

Name | Type | Presence | Description
--- | --- | --- | ---
Label | string | Optional<br>(0 or 1) | A label to assign the addresses to.

*Result---P2SH address and hex-encoded redeem script*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object describing the multisig address
→<br>`address` | string (base58) | Required<br>(exactly 1) | The P2SH address for this multisig redeem script
→<br>`redeemScript` | string (hex) | Required<br>(exactly 1) | The multisig redeem script encoded as hex

*Example from Dash Core 0.17.0*

Adding a 1-of-2 P2SH multisig address with the label "test label" by combining one P2PKH address and one full public key:

```bash
dash-cli -testnet addmultisigaddress 1 '''
  [
    "ySxkBWzPwMrZLAY9ZPitMnSwf4NSUBPbiH",
    "02594523b004e82849a66b3da096b1e680bf2ed5f7d03a3443c027aa5777bb6223"
  ]
''' \
 'test label'
```

Result:

```json
{
  "address": "8jYUv8hJcbSUPbwYmzp1XMPU6SXoic3hwi",
  "redeemScript": "512103283a224c2c014d1d0ef82b00470b6b277d71e227c0e2394f9baade5d666e57d32102594523b004e82849a66b3da096b1e680bf2ed5f7d03a3443c027aa5777bb622352ae"
}
```

(New P2SH multisig address also stored in wallet.)

*See also*

* [CreateMultiSig](/docs/core-api-ref-remote-procedure-calls-util#createmultisig): creates a P2SH multi-signature address.
* [DecodeScript](/docs/core-api-ref-remote-procedure-calls-raw-transactions#decodescript): decodes a hex-encoded P2SH redeem script.

# BackupWallet
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]

The [`backupwallet` RPC](core-api-ref-remote-procedure-calls-wallet#backupwallet) safely copies `wallet.dat` to the specified file, which can be a directory or a path with filename.

*Parameter #1---destination directory or filename*

Name | Type | Presence | Description
--- | --- | --- | ---
Destination | string | Required<br>(exactly 1) | A filename or directory name.  If a filename, it will be created or overwritten.  If a directory name, the file `wallet.dat` will be created or overwritten within that directory

*Result---`null` or error*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | Always `null` whether success or failure.  The JSON-RPC error and message fields will be set if a failure occurred

*Example from Dash Core 0.12.2*

```bash
dash-cli -testnet backupwallet /tmp/backup.dat
```

*See also*

* [DumpWallet](core-api-ref-remote-procedure-calls-wallet#dumpwallet): creates or overwrites a file with all wallet keys in a human-readable format.
* [ImportWallet](core-api-ref-remote-procedure-calls-wallet#importwallet): imports private keys from a file in wallet dump file format (see the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#dumpwallet)). These keys will be added to the keys currently in the wallet.  This call may need to rescan all or parts of the block chain for transactions affecting the newly-added keys, which may take several minutes.

# CreateWallet
[block:callout]
{
  "type": "success",
  "body": "Added in Dash Core 0.17.0"
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]
The [`createwallet` RPC](core-api-ref-remote-procedure-calls-wallet#createwallet) creates and loads a new wallet.

*Parameter #1---wallet name*

Name | Type | Presence | Description
--- | --- | --- | ---
`wallet_name` | string | Required<br>(exactly 1) | The name for the new wallet. If this is a path, the wallet will be created at the path location.

*Parameter #2---disable private keys*

Name | Type | Presence | Description
--- | --- | --- | ---
`disable_private_keys` | bool | Optional<br>(0 or 1) | Disable the possibility of private keys. Only watchonlys are possible in this mode.

*Parameter #3---blank*

Name | Type | Presence | Description
--- | --- | --- | ---
`blank` | bool | Optional<br>(0 or 1) | Create a blank wallet. A blank wallet has no keys or HD seed. Use [`upgradetohd`](#upgradetohd) to add an HD seed.

*Parameter #4---passphrase*

Name | Type | Presence | Description
--- | --- | --- | ---
`passphrase` | string | Optional<br>(0 or 1) | Encrypt the wallet with this passphrase.

*Result---wallet name and any warnings*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object containing information about wallet creation
→<br>`name` | string | Required<br>(exactly 1) | The wallet name if created successfully. If the wallet was created using a full path, the `wallet_name` will be the full path
→<br>`warning` | string | Required<br>(exactly 1) | Warning message if wallet was not loaded cleanly.

*Example from Dash Core 0.17.0*

```bash
dash-cli -testnet createwallet new-wallet
```

Result:

```json
{
  "name": "new-wallet",
  "warning": ""
}
```
[block:callout]
{
  "type": "info",
  "body": "In the example above, a new directory named `new-wallet` was created in the current data directory (`~/.dashcore/testnet3/`). This new directory contains the wallet.dat file and other related wallet files for the new wallet."
}
[/block]
*See also*

* [LoadWallet](core-api-ref-remote-procedure-calls-wallet#loadwallet): loads a wallet from a wallet file or directory.

# DumpHDInfo

The [`dumphdinfo` RPC](core-api-ref-remote-procedure-calls-wallet#dumphdinfo) returns an object containing sensitive private info about this HD wallet

*Parameters: none*

*Result---HD wallet information*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | An object containing sensitive private info about this HD wallet.
→ <br>`hdseed` | string | Required<br>(exactly 1) | The BIP-32 HD seed (in hex)
→ <br>`mnemonic` | string | Required<br>(exactly 1) | The BIP-39 mnemonic for this HD wallet (English words)
→ <br>`mnemonicpassphrase` | string | Required<br>(exactly 1) | The BIP-39 mnemonic passphrase for this HD wallet (may be empty)

*Example from Dash Core 0.12.2*

```bash
dash-cli -testnet dumphdinfo
```

Result (truncated for security reasons):
```json
{
  "hdseed": "20c63c3fb298ebd52de3 ...",
  "mnemonic": "cost circle shiver ...",
  "mnemonicpassphrase": ""
}
```

*See also: none*

# DumpPrivKey
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**). Requires an unlocked wallet or an unencrypted wallet."
}
[/block]

The [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#dumpprivkey) returns the wallet-import-format (WIP) private key corresponding to an address. (But does not remove it from the wallet.)

*Parameter #1---the address corresponding to the private key to get*

Name | Type | Presence | Description
--- | --- | --- | ---
P2PKH Address | string (base58) | Required<br>(exactly 1) | The P2PKH address corresponding to the private key you want returned.  Must be the address corresponding to a private key in this wallet

*Result---the private key*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (base58) | Required<br>(exactly 1) | The private key encoded as base58check using wallet import format

*Example from Dash Core 0.12.2*

```bash
dash-cli -testnet dumpprivkey ycBuREgSskHHkWLxDa9A5WppCki6PfFycL
```

Result:

```text
cQZZ4awQvcXXyES3CmUJqSgeTobQm9t9nyUr337kvUtsWsnvvMyw
```

*See also*

* [ImportPrivKey](core-api-ref-remote-procedure-calls-wallet#importprivkey): adds a private key to your wallet. The key should be formatted in the wallet import format created by the [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#dumpprivkey).
* [DumpWallet](core-api-ref-remote-procedure-calls-wallet#dumpwallet): creates or overwrites a file with all wallet keys in a human-readable format.

# DumpWallet
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**). Requires an unlocked wallet or an unencrypted wallet."
}
[/block]

The [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#dumpwallet) creates or overwrites a file with all wallet keys in a human-readable format.

*Parameter #1---a filename*

Name | Type | Presence | Description
--- | --- | --- | ---
Filename | string | Required<br>(exactly 1) | The filename with path (either absolute or relative to Dash Core) into which the wallet dump will be placed.  An existing file with that name will be overwritten. 

*Result---information about exported wallet*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object describing dumped wallet file
→<br>`dashcoreversion` | string | Required<br>(exactly 1) | Dash Core build details
→<br>`lastblockheight` | int | Required<br>(exactly 1) | Height of the most recent block received
→<br>`lastblockhash` | string (hex) | Required<br>(exactly 1) | Hash of the most recent block received
→<br>`lastblocktime` | string | Required<br>(exactly 1) | Timestamp of the most recent block received
→<br>`keys` | int | Required<br>(exactly 1) | Number of keys dumped
→<br>`filename` | string | Required<br>(exactly 1) | Name of the file the wallet was dumped to
→<br>`warning` | string | Required<br>(exactly 1) | Warning to not share the file due to it containing the private keys

*Example from Dash Core 0.13.0*

Create a wallet dump and then print its first 10 lines.

```bash
dash-cli -testnet dumpwallet /tmp/dump.txt
head /tmp/dump.txt
```

Results:

```json
{
  "dashcoreversion": "v0.17.0.0",
  "lastblockheight": 250186,
  "lastblockhash": "0000000000a82fb1890de5da4740d0671910a436fe6fc4503a3e553adef073b4",
  "lastblocktime": "2018-10-23T12:50:44Z",
  "keys": 8135,
  "file": "/tmp/dump.txt",
  "warning": "/tmp/dump.txt file contains all private keys from this wallet. Do not share it with anyone!"
}
```

Results (the first 10 lines of the file):

```bash
>>>>>>>># Wallet dump created by Dash Core v0.13.0.0
>>>>>>>># * Created on 2020-12-09T18:40:52Z
>>>>>>>># * Best block at time of backup was 405635 (000000b2304f57eefd42cdd943e7736d479468beb08049b8f88d11ebc7cf6f02),
>>>>>>>>#   mined on 2020-12-09T18:40:23Z

cQZZ4awQvcXXyES3CmUJqSgeTobQm9t9nyUr337kvUtsWsnvvMyw 2018-12-14T17:24:37Z change=1 # addr=ycBuREgSskHHkWLxDa9A5WppCki6PfFycL
cTBRPnJoPjEMh67v1zes437v8Po5bFLDWKgEudTJMhVaLs1ZVGJe 2018-12-14T17:24:37Z change=1 # addr=yNsWkgPLN1u7p5dfWYnasYdgirU2J3tjUj
cRkkwrFnQUrih3QiT87sNy1AxyfjzqVYSyVYuL3qnJcSiQfE4QJa 2018-12-14T17:24:37Z change=1 # addr=yRkHzRbRKn8gBp5826mbaBvxLuBBNDVQg3
cQM7KoqQjHCCTrDhnfBEY1vpW9W65zRvaQeTb41UbFb6WX8Q8UkQ 2018-12-14T17:24:37Z change=1 # addr=yVEdefApUYiDLHApvvWCK5afTtJeQada8Y
cTGSKYaQTQabnjNSwCqpjYXiucVujTXiwp9dzmJV9cNAiayAJusi 2018-12-14T17:24:37Z change=1 # addr=ybQYgp21ZyZK8JuMLb2CVwG4TaWrXVXD5M
```

*See also*

* [BackupWallet](core-api-ref-remote-procedure-calls-wallet#backupwallet): safely copies `wallet.dat` to the specified file, which can be a directory or a path with filename.
* [ImportWallet](core-api-ref-remote-procedure-calls-wallet#importwallet): imports private keys from a file in wallet dump file format (see the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#dumpwallet)). These keys will be added to the keys currently in the wallet.  This call may need to rescan all or parts of the block chain for transactions affecting the newly-added keys, which may take several minutes.

# EncryptWallet
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]

The [`encryptwallet` RPC](core-api-ref-remote-procedure-calls-wallet#encryptwallet) encrypts the wallet with a passphrase.  This is only to enable encryption for the first time. After encryption is enabled, you will need to enter the passphrase to use private keys.
[block:callout]
{
  "type": "warning",
  "body": "**Warning:** if using this RPC on the command line, remember that your shell probably saves your command lines (including the value of the passphrase parameter). In addition, there is no RPC to completely disable encryption. If you want to return to an unencrypted wallet, you must create a new wallet and restore your data from a backup made with the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#dumpwallet)."
}
[/block]
*Parameter #1---a passphrase*

Name | Type | Presence | Description
--- | --- | --- | ---
Passphrase | string | Required<br>(exactly 1) | The passphrase to use for the encrypted wallet.  Must be at least one character

*Result---a notice (with program shutdown)*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | A notice that the server is stopping and that you need to make a new backup.  The wallet is now encrypted

*Example from Dash Core 0.12.2*

```bash
dash-cli -testnet encryptwallet "test"
```

Result:

```text
Wallet encrypted; Dash Core server stopping, restart to run with encrypted wallet.
The keypool has been flushed and a new HD seed was generated (if you are using
HD). You need to make a new backup.

```

*See also*

* [WalletPassphrase](core-api-ref-remote-procedure-calls-wallet#walletpassphrase): stores the wallet decryption key in memory for the indicated number of seconds. Issuing the `walletpassphrase` command while the wallet is already unlocked will set a new unlock time that overrides the old one.
* [WalletLock](core-api-ref-remote-procedure-calls-wallet#walletlock): removes the wallet encryption key from memory, locking the wallet. After calling this method, you will need to call `walletpassphrase` again before being able to call any methods which require the wallet to be unlocked.
* [WalletPassphraseChange](core-api-ref-remote-procedure-calls-wallet#walletpassphrasechange): changes the wallet passphrase from 'old passphrase' to 'new passphrase'.

# GetAddressInfo
[block:callout]
{
  "type": "success",
  "body": "Added in Dash Core 0.17.0"
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]
The [`getaddressinfo` RPC](core-api-ref-remote-procedure-calls-wallet#getaddressinfo) returns information about the given Dash address. Note: Some information requires the address to be in the wallet.

*Parameter #1---a P2PKH or P2SH address*

Name | Type | Presence | Description
--- | --- | --- | ---
Address | string (base58) | Required<br>(exactly 1) | The P2PKH or P2SH address encoded in base58check format

*Result---returns information about the address*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | Information about the address
→<br>`address` | string (base58) | Required<br>(exactly 1) | The Dash address given as parameter
→<br>`scriptPubKey` | string (hex) | Required<br>(exactly 1) | The hex encoded scriptPubKey generated by the address  
→<br>`ismine` | bool | Required<br>(exactly 1) | Set to `true` if the address belongs to the wallet; set to false if it does not.  Only returned if wallet support enabled
→<br>`iswatchonly` | bool | Required<br>(exactly 1) | Set to `true` if the address is watch-only.  Otherwise set to `false`.  Only returned if address is in the wallet
→<br>`solvable` | bool | Required<br>(exactly 1) | Whether we know how to spend coins sent to this address, ignoring the possible lack of private keys
→<br>`desc` | bool | Optional<br>(0 or 1) | A descriptor for spending coins sent to this address (only when solvable)
→<br>`isscript` | bool | Required<br>(exactly 1) | Set to `true` if a P2SH address; otherwise set to `false`.  Only returned if the address is in the wallet
→<br>`ischange` | bool | Required<br>(exactly 1) | Set to `true` if the address was used for change output.
→<br>`script` | string | Optional<br>(0 or 1) | Only returned for P2SH addresses belonging to this wallet. This is the type of script:<br>• `pubkey` for a P2PK script inside P2SH<br>• `pubkeyhash` for a P2PKH script inside P2SH<br>• `multisig` for a multisig script inside P2SH<br>• `nonstandard` for unknown scripts
→<br>`hex` | string (hex) | Optional<br>(0 or 1) | Only returned for P2SH addresses belonging to this wallet.  This is the redeem script encoded as hex
→<br>`pubkeys` | array | Optional<br>(0 or 1) | Array of pubkeys associated with the known redeemscript (only if `script` is "multisig")
→ →<br>Pubkey | string | Optional<br>(0 or more) | A public key
→<br>`sigsrequired` | number (int) | Optional<br>(0 or 1) | Only returned for multisig P2SH addresses belonging to the wallet.  The number of signatures required by this script
→<br>`pubkey` | string (hex) | Optional<br>(0 or 1) | The public key corresponding to this address.  Only returned if the address is a P2PKH address in the wallet
→<br>`embedded` | object | Optional<br>(0 or 1) | Information about the address embedded in P2SH, if relevant and known. It includes all getaddressinfo output fields for the embedded address, excluding metadata (`timestamp`, `hdkeypath`) and relation to the wallet (`ismine`, `iswatchonly`, `account`).
→<br>`iscompressed` | bool | Optional<br>(0 or 1) | Set to `true` if a compressed public key or set to `false` if an uncompressed public key.  Only returned if the address is a P2PKH address in the wallet
→<br>`label` | string | Optional<br>(0 or 1) | The label associated with the address, `""` is the default account
→<br>`account` | string | Optional<br>(0 or 1) | **This field will be removed in V0.18. To see this deprecated field, start dashd with `-deprecatedrpc=accounts`.** The account associated with the address, `""` is the default account.
→<br>`timestamp` | number (int) | Optional<br>(0 or 1) | The creation time of the key if available in seconds since epoch (Jan 1 1970 GMT)
→<br>`hdkeypath` | string | Optional<br>(0 or 1) | The HD keypath if the key is HD and available  
→<br>`hdchainid` | string (hash160) | Optional<br>(0 or 1) | The ID of the HD chain
→<br>`labels` | array | Optional<br>(0 or 1) | Array of labels associated with the address
→→<br>Label Data | object | Optional<br>(0 or 1) | JSON object containing label data
→→→<br>`name` | string | Optional<br>(0 or 1) | The label
→→→<br>`purpose` | string | Optional<br>(0 or 1) | Purpose of address (`send` for sending address, `receive` for receiving address

*Example from Dash Core 0.18.0*

Get info for the following P2PKH address from the wallet:

``` bash
dash-cli getaddressinfo yYvsn6vdnkeq9VG1JbkfqKbjv3gUmFmnny
```

Result:

``` json
{
  "address": "yYvsn6vdnkeq9VG1JbkfqKbjv3gUmFmnny",
  "scriptPubKey": "76a9148a54e0c51084f0e5819a66bb1c4d01191f5caa3888ac",
  "ismine": true,
  "solvable": true,
  "desc": "pkh([8a54e0c5]0214889c34100d00aca6e7cbfe0fa72d83c28857585740bff5f3db6b37e51d9aaa)#wyvgzv2k",
  "iswatchonly": false,
  "isscript": false,
  "pubkey": "0214889c34100d00aca6e7cbfe0fa72d83c28857585740bff5f3db6b37e51d9aaa",
  "iscompressed": true,
  "ischange": true,
  "timestamp": 1612374776,
  "hdkeypath": "m/44'/1'/0'/1/137",
  "hdchainid": "a1f28726e62a7766153dac6c90242b712dbb98f4d457b7fb46e09136461dca7e",
  "labels": [
  ]
}
```

Get info for the following P2SH multisig address from the wallet:

``` bash
dash-cli -testnet getaddressinfo 8uJLxDxk2gEMbidF5vT8XLS2UCgQmVcroW
```

Result:

``` json
{
  "address": "8uJLxDxk2gEMbidF5vT8XLS2UCgQmVcroW",
  "scriptPubKey": "a914a33155e490d146e656a9bac2cbee9c625ef42f0a87",
  "ismine": false,
  "solvable": false,
  "iswatchonly": false,
  "isscript": true,
  "ischange": false,
  "labels": [
  ]
}
```

*See also*

* [ImportAddress](core-api-ref-remote-procedure-calls-wallet#importaddress): adds an address or pubkey script to the wallet without the associated private key, allowing you to watch for transactions affecting that address or pubkey script without being able to spend any of its outputs.
* [GetNewAddress](core-api-ref-remote-procedure-calls-wallet#getnewaddress): returns a new Dash address for receiving payments. If an account is specified, payments received with the address will be credited to that account.
* [ValidateAddress](/docs/core-api-ref-remote-procedure-calls-util#validateaddress): returns information about the given Dash address.

# GetAddressesByLabel

[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]

The [`getaddressesbylabel` RPC](core-api-ref-remote-procedure-calls-wallet#getaddressesbylabel) returns a list of every address assigned to a particular label.

*Parameter #1---the label name*

Name | Type | Presence | Description
--- | --- | --- | ---
Label | string | Required<br>(exactly 1) | The name of the label associated with the addresses to get.  To get addresses from the default account, pass an empty string (`""`).

*Result---a list of addresses*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | A JSON object containing all addresses belonging to the specified label as keys.
→Address | object | Optional<br>(1 or more) | A JSON object with information about a P2PKH or P2SH address belonging to the label
→→`purpose` | string | Optional<br>(1 or more) | Purpose of address (`send` for sending address, `receive` for receiving address)

*Example from Dash Core 0.17.0*

Get the addresses assigned to the label "doc test":

``` bash
dash-cli -testnet getaddressesbylabel "doc test"
```

Result:

``` json
{
  "yacJKd6tRz2JSn8Wfp9GKgCbuowAEBivrA": {
    "purpose": "receive"
  }
}
```

*See also*

* [GetAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getaccount): returns the name of the account associated with the given address.
* [GetBalance](core-api-ref-remote-procedure-calls-wallet#getbalance): gets the balance in decimal dash across all accounts or for a particular account.

# GetBalance
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]

The [`getbalance` RPC](core-api-ref-remote-procedure-calls-wallet#getbalance) gets the total _available balance_ in Dash. The _available balance_ is what the wallet considers currently spendable, and is thus affected by options which limit spendability such as `-spendzeroconfchange`.

*Parameter #1---unused parameter*

Name | Type | Presence | Description
--- | --- | --- | ---
Unused | string | Optional<br>(0 or 1) | **Deprecated: (previously account) will be removed in a later version of Dash Core**<br><br>Remains for backward compatibility. Must be excluded or set to `"*"`.

*Parameter #2---the minimum number of confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations an externally-generated transaction must have before it is counted towards the balance.  Transactions generated by this node are counted immediately.  Typically, externally-generated transactions are payments to this wallet and transactions generated by this node are payments to other wallets.  Use `0` to count unconfirmed transactions.  Default is `1`

*Parameter #3---whether to add the balance from transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(exactly 1) | Add the balance from InstantSend locked transactions

*Parameter #4---whether to include watch-only addresses*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Watch-Only | bool | Optional<br>(0 or 1) | If set to `true`, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.  If set to `false` (the default), treat watch-only addresses as if they didn't belong to this wallet

*Result---the balance in Dash*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (dash) | Required<br>(exactly 1) | The balance of the account (or all accounts) in dash

*Examples from Dash Core 0.17.0*

Get the balance, including transactions with at least three confirmations and those spent to watch-only addresses. Do not include InstantSend locked transactions.

```bash
dash-cli -testnet getbalance "*" 3 false true
```

Result:

```json
0.00000000
```

Get the balance, including transactions with at least three confirmations and those spent to watch-only addresses. Include the balance from InstantSend locked transactions.

```bash
dash-cli -testnet getbalance "" 3 true true
```

Result:

```json
1.00000000
```

*See also*

* [ListAccounts](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#listaccounts): lists accounts and their balances.
* [GetReceivedByAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getreceivedbyaccount): returns the total amount received by addresses in a particular account from transactions with the specified number of confirmations.  It does not count coinbase transactions.
* [GetReceivedByAddress](core-api-ref-remote-procedure-calls-wallet#getreceivedbyaddress): returns the total amount received by the specified address in transactions with the specified number of confirmations. It does not count coinbase transactions.

# GetNewAddress
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]

The [`getnewaddress` RPC](core-api-ref-remote-procedure-calls-wallet#getnewaddress) returns a new Dash address for receiving payments. If `label` is specified, the address is added to the address book so payments received with the address will be associated with `label`.

*Parameter #1---an account name*

Name | Type | Presence | Description
--- | --- | --- | ---
`label` | string | Optional<br>(0 or 1) | The label name for the address to be linked to. If not provided, the default label `""` is used. It can also be set to the empty string `""` to represent the default label. The label does not need to exist, it will be created if there is no label by the given name.

*Result---a dash address never previously returned*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (base58) | Required<br>(exactly 1) | The new Dash address.  

*Example from Dash Core 0.17.0*

Create a new address in the "doc test" account:

```bash
dash-cli -testnet getnewaddress "doc test"
```

Result:

```text
yPuNTqCGzXtU3eEV5jHvhhJkzEPyJLmVkb
```

*See also*

* [GetAccountAddress](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getaccountaddress): returns the current Dash address for receiving payments to this account. If the account doesn't exist, it creates both the account and a new address for receiving payment.  Once a payment has been received to an address, future calls to this RPC for the same account will return a different address.
* [GetRawChangeAddress](core-api-ref-remote-procedure-calls-wallet#getrawchangeaddress): returns a new Dash address for receiving change. This is for use with raw transactions, not normal use.
* [GetBalance](core-api-ref-remote-procedure-calls-wallet#getbalance): gets the balance in decimal dash across all accounts or for a particular account.

# GetRawChangeAddress
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]

The [`getrawchangeaddress` RPC](core-api-ref-remote-procedure-calls-wallet#getrawchangeaddress) returns a new Dash address for receiving change. This is for use with raw transactions, not normal use.

*Parameters: none*

*Result---a P2PKH address which can be used in raw transactions*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (base58) | Required<br>(exactly 1) | A P2PKH address which has not previously been returned by this RPC.  The address will be removed from the keypool but not marked as a receiving address, so RPCs such as the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#dumpwallet) will show it as a change address.  The address may already have been part of the keypool, so other RPCs such as the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#dumpwallet) may have disclosed it previously.  If the wallet is unlocked, its keypool will also be filled to its max (by default, 100 unused keys).  If the wallet is locked and its keypool is empty, this RPC will fail

*Example from Dash Core 0.12.2*

```bash
dash-cli -testnet getrawchangeaddress
```

Result:

```text
yXBr9BiJmugTzHPgByDmvjJMAkvhTmXVJ8
```

*See also*

* [GetNewAddress](core-api-ref-remote-procedure-calls-wallet#getnewaddress): returns a new Dash address for receiving payments. If an account is specified, payments received with the address will be credited to that account.
* [GetAccountAddress](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getaccountaddress): returns the current Dash address for receiving payments to this account. If the account doesn't exist, it creates both the account and a new address for receiving payment.  Once a payment has been received to an address, future calls to this RPC for the same account will return a different address.

# GetReceivedByAddress
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]
![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg) Note: This RPC only returns a balance for addresses contained in the local wallet.

The [`getreceivedbyaddress` RPC](core-api-ref-remote-procedure-calls-wallet#getreceivedbyaddress) returns the total amount received by the specified address in transactions with the specified number of confirmations. It does not count coinbase transactions.

*Parameter #1---the address*

Name | Type | Presence | Description
--- | --- | --- | ---
Address | string | Required<br>(exactly 1) | __Only works for addresses contained in the local wallet__<br><br>The address whose transactions should be tallied

*Parameter #2---the minimum number of confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations an externally-generated transaction must have before it is counted towards the balance.  Transactions generated by this node are counted immediately.  Typically, externally-generated transactions are payments to this wallet and transactions generated by this node are payments to other wallets.  Use `0` to count unconfirmed transactions.  Default is `1`

*Parameter #3---whether to include transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(exactly 1) | Add the balance from InstantSend locked transactions

*Result---the amount of dash received*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (dash) | Required<br>(exactly 1) | The amount of dash received by the address, excluding coinbase transactions.  May be `0`

*Example from Dash Core 0.13.0*

Get the dash received for a particular address, only counting
transactions with six or more confirmations (ignore InstantSend locked transactions):

```bash
dash-cli -testnet getreceivedbyaddress yYoCWcjbykWsQJ7MVJrTMeQd8TZe5N4Q7g 6
```

Result:

```json
0.00000000
```

Get the dash received for a particular address, only counting
transactions with six or more confirmations (include InstantSend locked transactions):

```bash
dash-cli -testnet getreceivedbyaddress yYoCWcjbykWsQJ7MVJrTMeQd8TZe5N4Q7g 6 true
```

Result:

```json
0.30000000
```

*See also*

* [GetReceivedByAccount](core-api-ref-remote-procedure-calls-wallet-deprecated#getreceivedbyaccount): returns the total amount received by addresses in a particular account from transactions with the specified number of confirmations.  It does not count coinbase transactions.
* [GetAddressesByAccount](core-api-ref-remote-procedure-calls-wallet-deprecated#getaddressesbyaccount): returns a list of every address assigned to a particular account.
* [ListAccounts](core-api-ref-remote-procedure-calls-wallet-deprecated#listaccounts): lists accounts and their balances.

# GetReceivedByLabel

[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]

The [`getreceivedbylabel` RPC](core-api-ref-remote-procedure-calls-wallet#getreceivedbylabel) returns the total amount received by addresses with <label> in transactions with specified minimum number of confirmations.

*Parameter #1---the label name*

Name | Type | Presence | Description
--- | --- | --- | ---
Label | string | Required<br>(exactly 1) | The selected label, may be the default label using `""`.

*Parameter #2---the minimum number of confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations a transaction must have before it is counted towards the balance. Use `0` to count unconfirmed transactions.  Default is `1`

*Parameter #3---whether to include transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(exactly 1) | Add the balance from InstantSend locked transactions (default=false)

*Result---the number of DASH received*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (dash) | Required<br>(exactly 1) | The total amount of DASH received for this label.  May be `0`

*Example from Dash Core 0.17.0*

Get the DASH received by the "doc test" label with six or more confirmations:

``` bash
dash-cli -testnet getreceivedbylabel "doc test" 6
```

Result:

``` json
0.30000000
```

*See also*

* [GetReceivedByAddress](core-api-ref-remote-procedure-calls-wallet#getreceivedbyaddress): returns the total amount received by the specified address in transactions with the specified number of confirmations. It does not count coinbase transactions.
* [GetAddressesByLabel](core-api-ref-remote-procedure-calls-wallet#getaddressesbylabel): returns a list of every address assigned to a particular label.
* [ListLabels](core-api-ref-remote-procedure-calls-wallet#listlabels): lists labels.

# GetTransaction
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]

The [`gettransaction` RPC](core-api-ref-remote-procedure-calls-wallet#gettransaction) gets detailed information about an in-wallet transaction.

*Parameter #1---a transaction identifier (TXID)*

Name | Type | Presence | Description
--- | --- | --- | ---
TXID | string (hex) | Required<br>(exactly 1) | The TXID of the transaction to get details about.  The TXID must be encoded as hex in RPC byte order

*Parameter #2---whether to include watch-only addresses in details and calculations*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Watch-Only | bool | Optional<br>(0 or 1) | If set to `true`, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.  If set to `false` (the default), treat watch-only addresses as if they didn't belong to this wallet

*Result---a description of the transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object describing how the transaction affects the wallet
→<br>`amount` | number (dash) | Required<br>(exactly 1) | A positive number of dash if this transaction increased the total wallet balance; a negative number of dash if this transaction decreased the total wallet balance, or `0` if the transaction had no net effect on wallet balance
→<br>`fee` | number (dash) | Optional<br>(0 or 1) | If an outgoing transaction, this is the fee paid by the transaction reported as negative dash
→ <br>`confirmations` | number (int) | Required<br>(exactly 1) | The number of confirmations the transaction has received.  Will be `0` for unconfirmed and `-1` for conflicted
→ <br>`instantlock` | bool | Required<br>(exactly 1) | **Always `false` if [lite mode](core-guide-dash-features#lite-mode) is enabled**<br><br>Current transaction lock state (InstantSend and/or ChainLock)
→ <br>`instantlock_internal` | bool | Required<br>(exactly 1) | **Always `false` if [lite mode](core-guide-dash-features#lite-mode) is enabled**<br><br>Current InstantSend transaction lock state
→ <br>`chainlock` | bool | Required<br>(exactly 1) | *Added in Dash Core 0.14.0*<br><br>**Always `false` if [lite mode](core-guide-dash-features#lite-mode) is enabled**<br><br>If set to `true`, this transaction is in a block that is locked (not susceptible to a chain re-org)  
→ <br>`generated` | bool | Optional<br>(0 or 1) | Set to `true` if the transaction is a coinbase.  Not returned for regular transactions
→ <br>`blockhash` | string (hex) | Optional<br>(0 or 1) | The hash of the block on the local best block chain which includes this transaction, encoded as hex in RPC byte order.  Only returned for confirmed transactions
→ <br>`blockindex` | number (int) | Optional<br>(0 or 1) | The index of the transaction in the block that includes it.  Only returned for confirmed transactions
→ <br>`blocktime` | number (int) | Optional<br>(0 or 1) | The block header time (Unix epoch time) of the block on the local best block chain which includes this transaction.  Only returned for confirmed transactions
→ <br>`txid` | string (hex) | Required<br>(exactly 1) | The TXID of the transaction, encoded as hex in RPC byte order
→ <br>`walletconflicts` | array | Required<br>(exactly 1) | An array containing the TXIDs of other transactions that spend the same inputs (UTXOs) as this transaction.  Array may be empty
→ →<br>TXID | string (hex) | Optional<br>(0 or more) | The TXID of a conflicting transaction, encoded as hex in RPC byte order
→ <br>`time` | number (int) | Required<br>(exactly 1) | A Unix epoch time when the transaction was added to the wallet
→ <br>`timereceived` | number (int) | Required<br>(exactly 1) | A Unix epoch time when the transaction was detected by the local node, or the time of the block on the local best block chain that included the transaction
→ <br>`abandoned` | bool | Optional<br>(0 or 1) | `true` if the transaction has been abandoned (inputs are respendable). Only available for the 'send' category of transactions.
→ <br>`comment` | string | Optional<br>(0 or 1) | For transaction originating with this wallet, a locally-stored comment added to the transaction.  Only returned if a comment was added
→ <br>`to` | string | Optional<br>(0 or 1) | For transaction originating with this wallet, a locally-stored comment added to the transaction identifying who the transaction was sent to.  Only returned if a comment-to was added
→<br>`DS` | bool | Optional<br>(0 or 1) | Set to 1 if a CoinJoin transaction
→<br>`details` | array | Required<br>(exactly 1) | An array containing one object for each input or output in the transaction which affected the wallet
→ → <br>`involvesWatchonly` | bool | Optional<br>(0 or 1) | Set to `true` if the input or output involves a watch-only address.  Otherwise not returned
→ →<br>`account` | string | Required<br>(exactly 1) | **This field will be removed in V0.18. To see this deprecated field, start dashd with `-deprecatedrpc=accounts`.**<br><br>The account which the payment was credited to or debited from.  May be an empty string (\\") for the default account"
→ →<br>`address` | string (base58) | Optional<br>(0 or 1) | If an output, the address paid (may be someone else's address not belonging to this wallet).  If an input, the address paid in the previous output.  May be empty if the address is unknown, such as when paying to a non-standard pubkey script
→ →<br>`category` | string | Required<br>(exactly 1) | Set to one of the following values:<br>• `send` if sending payment normally<br>• `coinjoin` if sending CoinJoin payment<br>• `receive` if this wallet received payment in a regular transaction<br>• `generate` if a matured and spendable coinbase<br>• `immature` if a coinbase that is not spendable yet<br>• `orphan` if a coinbase from a block that's not in the local best block chain
→ →<br>`amount` | number (dash) | Required<br>(exactly 1) | A negative dash amount if sending payment; a positive dash amount if receiving payment (including coinbases)
→ →<br>`label` | string | Optional<br>(0 or 1) | An optional comment for the address/transaction
→ →<br>`vout` | number (int) | Required<br>(exactly 1) | For an output, the output index (vout) for this output in this transaction.  For an input, the output index for the output being spent in its transaction.  Because inputs list the output indexes from previous transactions, more than one entry in the details array may have the same output index
→ →<br>`fee` | number (dash) | Optional<br>(0 or 1) | If sending payment, the fee paid as a negative dash value.  May be `0`.  Not returned if receiving payment
→ →<br>`abandoned` | bool | Optional<br>(0 or 1) | *Added in Bitcoin Core 0.12.1*<br><br>Indicates if a transaction is was abandoned:<br>• `true` if it was abandoned (inputs are respendable)<br>• `false`  if it was not abandoned<br>Only returned by *send* category payments
→<br>`hex` | string (hex) | Required<br>(exactly 1) | The transaction in serialized transaction format

*Example from Dash Core 0.17.0*

```bash
dash-cli -testnet gettransaction \
  c099c882745ad150e9b2a55ef5818683c7ef597e1e5fc20856c67eabc3778ccc
```

Result:
```json
{
  "amount": -50.00000000,
  "fee": -0.00030000,
  "confirmations": 359811,
  "instantlock": true,
  "instantlock_internal": false,
  "chainlock": true,
  "blockhash": "00000a01007be2912c3123085534b58d341cb5e5980b967e8dcc021089487a1e",
  "blockindex": 1,
  "blocktime": 1553290594,
  "txid": "c099c882745ad150e9b2a55ef5818683c7ef597e1e5fc20856c67eabc3778ccc",
  "walletconflicts": [
  ],
  "time": 1553290584,
  "timereceived": 1553290584,
  "details": [
    {
      "address": "ycCsAUKsjdmoP4qAXiS1cjYA4ixM48zJWe",
      "category": "send",
      "amount": -50.00000000,
      "label": "Electrum",
      "vout": 1,
      "fee": -0.00030000,
      "abandoned": false
    }
  ],
  "hex": "0200000003aac865dba0e98fe32533df6bc3eaac160d04bb02966584fb61fc8d7788e09537010000006a47304402202d537257f23ab42b3e14f2ab533f39bb4586aa1b29a1f833f718a59493c8a601022019c6c156c20e66ef256519592b3c977b64d417c94aea4dca20cf18522a138993012103c67d86944315838aea7ec80d390b5d09b91b62483370d4979da5ccf7a7df77a9feffffff47833a270d2e2bac47bc5dc0df576c3a68b01bedbc89692060ac4113a6f9cb67010000006a4730440220442c19a913b10edc533bf63310f5294d6d91eec0eb9c510a3c6b0f33333f27320220501d5093ecdf603b8af9734e21d5de4710c8500309bfa4acdda243a294442b2c012103c67d86944315838aea7ec80d390b5d09b91b62483370d4979da5ccf7a7df77a9feffffffdcfd2d0fb30d79ffeadab8832e65be2310b67043ff3d74deac9a9cb825acda67000000006b483045022100cae8c025d3bec82903f356a5ec38d78a141447b6562e3aceac901f5fcc6f8567022076407835937514d6690c81c0c3b97f92d2b0ae9749249affaf539ead825692f4012102d6be44ab930ff67f084fbaf47a38b539b8d5da65c010952a972c9e524b6009dffeffffff0204fe2b00000000001976a914e3b0093477c2f629430d0a7b5813fe8b0153b0fd88ac00f2052a010000001976a914ae4365dedb1836ba215b9149602e0787a23376d288ac42010100"
}
```

*See also*

* [GetRawTransaction](core-api-ref-remote-procedure-calls-raw-transactions#getrawtransaction): gets a hex-encoded serialized transaction or a JSON object describing the transaction. By default, Dash Core only stores complete transaction data for UTXOs and your own transactions, so the RPC may fail on historic transactions unless you use the non-default `txindex=1` in your Dash Core startup settings.

# GetUnconfirmedBalance
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]

The [`getunconfirmedbalance` RPC](core-api-ref-remote-procedure-calls-wallet#getunconfirmedbalance) returns the wallet's total unconfirmed balance.

*Parameters: none*

*Result---the balance of unconfirmed transactions paying this wallet*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (dash) | Required<br>(exactly 1) | The total number of dash paid to this wallet in unconfirmed transactions

*Example from Dash Core 0.12.2*

```bash
dash-cli -testnet getunconfirmedbalance
```

Result (no unconfirmed incoming payments):

```json
0.00000000
```

*See also*

* [GetBalance](core-api-ref-remote-procedure-calls-wallet#getbalance): gets the balance in decimal dash across all accounts or for a particular account.

# GetWalletInfo
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]

The [`getwalletinfo` RPC](core-api-ref-remote-procedure-calls-wallet#getwalletinfo) provides information about the wallet.

*Parameters: none*

*Result---information about the wallet*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object describing the wallet
→<br>`walletname` | string | Required<br>(exactly 1) | The name of the wallet
→<br>`walletversion` | number (int) | Required<br>(exactly 1) | The version number of the wallet
→<br>`balance` | number (dash) | Required<br>(exactly 1) | The total confirmed balance of the wallet.  The same as returned by the [`getbalance` RPC](core-api-ref-remote-procedure-calls-wallet#getbalance) with default parameters
→<br>`coinjoin_balance` | number (dash) | Required<br>(exactly 1) | *Added in Dash Core 0.12.3*<br><br>The total CoinJoin balance of the wallet
→<br>`unconfirmed_balance` | number (dash) | Required<br>(exactly 1) | The total unconfirmed balance of the wallet.  The same as returned by the [`getunconfirmedbalance` RPC](core-api-ref-remote-procedure-calls-wallet#getunconfirmedbalance) with default parameters
→<br>`immature_balance` | number (dash) | Required<br>(exactly 1) | The total immature balance of the wallet.  This includes mining/masternode rewards that cannot be spent yet
→<br>`txcount` | number (int) | Required<br>(exactly 1) | The total number of transactions in the wallet (both spends and receives)
→<br>`timefirstkey` | number (int) | Required<br>(exactly 1) | **Added in Dash Core 0.17.0**<br><br>The timestamp (seconds since Unix epoch) of the oldest known key in the wallet
→<br>`keypoololdest` | number (int) | Required<br>(exactly 1) | The date as Unix epoch time when the oldest key in the wallet key pool was created; useful for only scanning blocks created since this date for transactions
→<br>`keypoolsize` | number (int) | Required<br>(exactly 1) | The number of keys in the wallet keypool
→<br>`keypoolsize_hd_internal` | number (int) | Optional<br>(0 or 1) | How many new keys are pre-generated for internal use (used for change outputs, only appears if the wallet is using this feature, otherwise external keys are used)
→<br>`keys_left` | number (int) | Required<br>(exactly 1) | The number of unused keys left since the last automatic backup
→<br>`unlocked_until` | number (int) | Optional<br>(0 or 1) | Only returned if the wallet was encrypted with the [`encryptwallet` RPC](core-api-ref-remote-procedure-calls-wallet#encryptwallet). A Unix epoch date when the wallet will be locked, or `0` if the wallet is currently locked
→<br>`paytxfee` | number (int) | Required<br>(exactly 1) | The transaction fee configuration, set in DASH/kB
→<br>`hdchainid` | string (hash) | Optional<br>(0 or 1) | The ID of the HD chain
→<br>`hdaccountcount` | number (int) | Optional<br>(0 or 1) | How many accounts of the HD chain are in this wallet
→ →<br>`hdaccountcountindex` | number (int) | Optional<br>(0 or 1) | The index of the account
→ →<br>`hdexternalkeyindex` | number (int) | Optional<br>(0 or 1) | Current external child key index
→ →<br>`hdinternalkeyindex` | number (int) | Optional<br>(0 or 1) | Current internal child key index
→<br>`scanning` | object | Required<br>(exactly 1) | _Added in Dash Core 0.16.1__<br><br>JSON object containing current scanning details (false (0) if no scan is in progress)
→ →<br>`duration` | number (int) | Optional<br>(0 or 1) | Elapsed seconds since scan start
→ →<br>`progress` | number (int) | Optional<br>(0 or 1) | Scanning progress percentage 0.0 to 1.0


*Example from Dash Core 0.17.0*

```bash
dash-cli -testnet getwalletinfo
```

Result:

```json
{
  "walletname": "",
  "walletversion": 61000,
  "balance": 188838.10331689,
  "coinjoin_balance": 2007.81107791,
  "unconfirmed_balance": 0.00000000,
  "immature_balance": 104.46428575,
  "txcount": 29137,
  "timefirstkey": 1544808277,
  "keypoololdest": 1601323587,
  "keypoolsize": 1000,
  "keys_left": 1000,
  "paytxfee": 0.00000000,
  "scanning": false
}
```

*See also*

* [ListTransactions](core-api-ref-remote-procedure-calls-wallet#listtransactions): returns the most recent transactions that affect the wallet.

# ImportAddress
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]

The [`importaddress` RPC](core-api-ref-remote-procedure-calls-wallet#importaddress) adds an address or pubkey script to the wallet without the associated private key, allowing you to watch for transactions affecting that address or pubkey script without being able to spend any of its outputs.

*Parameter #1---the address or pubkey script to watch*

Name | Type | Presence | Description
--- | --- | --- | ---
Address or Script | string (base58 or hex) | Required<br>(exactly 1) | Either a P2PKH or P2SH address encoded in base58check, or a pubkey script encoded as hex

*Parameter #2---The account into which to place the address or pubkey script*

Name | Type | Presence | Description
--- | --- | --- | ---
Label | string | Optional<br>(0 or 1) | An optional label.  Default is an empty string(\\")"

*Parameter #3---whether to rescan the block chain*

Name | Type | Presence | Description
--- | --- | --- | ---
Rescan | bool | Optional<br>(0 or 1) | Set to `true` (the default) to rescan the entire local block database for transactions affecting any address or pubkey script in the wallet (including transaction affecting the newly-added address or pubkey script).  Set to `false` to not rescan the block database (rescanning can be performed at any time by restarting Dash Core with the `-rescan` command-line argument).  Rescanning may take several minutes.

*Parameter #4---whether to rescan the block chain*

Name | Type | Presence | Description
--- | --- | --- | ---
P2SH | bool | Optional<br>(0 or 1) | Add the P2SH version of the script as well

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | If the address or pubkey script is added to the wallet (or is already part of the wallet), JSON `null` will be returned

*Example from Dash Core 0.12.2*

Add an address, rescanning the local block database for any transactions
matching it.

```bash
dash-cli -testnet importaddress \
  yg89Yt5Tjzs9nRpX3wJCuvr7KuQvgkvmeC "watch-only test" true
```

Result:

(No output; success.)

Show that the address has been added:

```bash
dash-cli -testnet getaccount yg89Yt5Tjzs9nRpX3wJCuvr7KuQvgkvmeC
```

Result:

```text
watch-only test
```

*See also*

* [ImportPrivKey](core-api-ref-remote-procedure-calls-wallet#importprivkey): adds a private key to your wallet. The key should be formatted in the wallet import format created by the [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#dumpprivkey).
* [ListReceivedByAddress](core-api-ref-remote-procedure-calls-wallet#listreceivedbyaddress): lists the total number of dash received by each address.

# ImportElectrumWallet

The [`importelectrumwallet` RPC](core-api-ref-remote-procedure-calls-wallet#importelectrumwallet) imports keys from an Electrum wallet export file (.csv or .json)

*Parameter #1---file name*

Name | Type | Presence | Description
--- | --- | --- | ---
File Name | string | Required<br>(exactly 1) | The Electrum wallet export file (should be in csv or json format)

*Parameter #2---index*

Name | Type | Presence | Description
--- | --- | --- | ---
Index | number (int) | Optional<br>(0 or 1) | Rescan the wallet for transactions starting from this block index (default: 0)

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | If the Electrum keys are imported successfully, JSON `null` will be returned

*Example from Dash Core 0.12.2*

```bash
dash-cli importelectrumwallet /tmp/electrum-export.csv
```

(Success: no result displayed.)

*See also: none*

# ImportMulti
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**).   Wallet must be unlocked."
}
[/block]

*Added in Dash Core 0.12.3 / Bitcoin Core 0.14.0*

The [`importmulti` RPC](core-api-ref-remote-procedure-calls-wallet#importmulti) imports addresses or scripts (with private keys, public keys, or P2SH redeem scripts) and optionally performs the minimum necessary rescan for all imports.

*Parameter #1---the addresses/scripts to import*

Name | Type | Presence | Description
--- | --- | --- | ---
Imports | array | Required<br>(exactly 1) | An array of JSON objects, each one being an address or script to be imported
→ Import | object | Required<br>(1 or more) | A JSON object describing a particular import
→ →<br>`scriptPubKey` | string (hex) | Optional<br>(0 or 1) | The script (string) to be imported.  Must have either this field or `address` below
→ →<br>`address` | string (base58) | Optional<br>(0 or 1) | The P2PKH or P2SH address to be imported.  Must have either this field or `scriptPubKey` above
→ →<br>`timestamp` | number (int) / string | Required<br>(exactly 1) | The creation time of the key in Unix epoch time or the string “now” to substitute the current synced block chain time. The timestamp of the oldest key will determine how far back block chain rescans need to begin. Specify `now` to bypass scanning for keys which are known to never have been used.  Specify `0` to scan the entire block chain. Blocks up to 2 hours before the earliest key creation time will be scanned
→ →<br>`redeemscript` | string | Optional<br>(0 or 1) | A redeem script. Only allowed if either the `address` field is a P2SH address or the `scriptPubKey` field is a P2SH scriptPubKey
→ →<br>`pubkeys` | array | Optional<br>(0 or 1) | Array of strings giving pubkeys that must occur in the scriptPubKey or redeemscript
→ →<br>`keys` | array | Optional<br>(0 or 1) | Array of strings giving private keys whose corresponding public keys must occur in the scriptPubKey or redeemscript
→ →<br>`internal` | bool | Optional<br>(0 or 1) | Stating whether matching outputs should be treated as change rather than incoming payments. The default is `false`
→ →<br>`watchonly` | bool | Optional<br>(0 or 1) | Stating whether matching outputs should be considered watched even when they're not spendable. This is only allowed if keys are empty. The default is `false`
→ →<br>`label` | string | Optional<br>(0 or 1) | Label to assign to the address, only allowed with `internal` set to `false`. The default is an empty string (“”)  

*Parameter #2---options regarding the import*

Name | Type | Presence | Description
--- | --- | --- | ---
Option | object | Optional<br>(0 or 1) | JSON object with options regarding the import
→ <br>`rescan` | bool | Optional<br>(0 or 1) | Set to `true` (the default) to rescan the entire local block chain for transactions affecting any imported address or script. Set to `false` to not rescan after the import. Rescanning may take a considerable amount of time and may require re-downloading blocks if using block chain pruning

*Result---execution result*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array of JSON objects, with each object describing the execution result of each import
→ Result | object | Required<br>(1 or more) | A JSON object describing the execution result of an imported address or script
→ → <br>`success` | string | Required<br>(exactly 1) | Displays `true` if the import has been successful or `false` if it failed
→ → <br>`error` | string : object | Optional<br>(0 or 1) | A JSON object containing details about the error. Only displayed if the import fails
→ → → <br>`code` | number (int) | Optional<br>(0 or 1) | The error code  
→ → → <br>`message` | string | Optional<br>(0 or 1) | The error message    

*Example from Dash Core 0.12.3*

Import the address `ycCsAUKsjdmoP4qAXiS1cjYA4ixM48zJWe` (giving it a label and scanning the entire block chain) and the scriptPubKey `76a9146cf5870411edc31ba5630d61c7cddff55b884fda88ac` (giving a specific timestamp and label):

```bash
dash-cli importmulti '
  [
    {
      "scriptPubKey" : { "address": "ycCsAUKsjdmoP4qAXiS1cjYA4ixM48zJWe" },
      "timestamp" : 0,
      "label" : "Personal"
    },
    {
      "scriptPubKey" : "76a9146cf5870411edc31ba5630d61c7cddff55b884fda88ac",
      "timestamp" : 1493912405,
      "label" : "TestFailure"
    }
  ]' '{ "rescan": true }'
```

Result (scriptPubKey import failed because `internal` was not set to `true`):

```json
[
  {
    "success": true
  },
  {
    "success": false,
    "error": {
      "code": -8,
      "message": "Internal must be set for hex scriptPubKey"
    }
  }
]
```

*See also*

* [ImportPrivKey](core-api-ref-remote-procedure-calls-wallet#importprivkey): adds a private key to your wallet. The key should be formatted in the wallet import format created by the [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#dumpprivkey).
* [ImportAddress](core-api-ref-remote-procedure-calls-wallet#importaddress): adds an address or pubkey script to the wallet without the associated private key, allowing you to watch for transactions affecting that address or pubkey script without being able to spend any of its outputs.
* [ImportWallet](core-api-ref-remote-procedure-calls-wallet#importwallet): imports private keys from a file in wallet dump file format (see the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#dumpwallet)). These keys will be added to the keys currently in the wallet.  This call may need to rescan all or parts of the block chain for transactions affecting the newly-added keys, which may take several minutes.

# ImportPrivKey
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**).   Wallet must be unlocked."
}
[/block]

The [`importprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#importprivkey) adds a private key to your wallet. The key should be formatted in the wallet import format created by the [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#dumpprivkey).

*Parameter #1---the private key to import*

Name | Type | Presence | Description
--- | --- | --- | ---
Private Key | string (base58) | Required<br>(exactly 1) | The private key to import into the wallet encoded in base58check using wallet import format (WIF)

*Parameter #2---the account into which the key should be placed*

Name | Type | Presence | Description
--- | --- | --- | ---
Account | string | Optional<br>(0 or 1) | The name of an account to which transactions involving the key should be assigned.  The default is the default account, an empty string (\\")"

*Parameter #3---whether to rescan the block chain*

Name | Type | Presence | Description
--- | --- | --- | ---
Rescan | bool | Optional<br>(0 or 1) | Set to `true` (the default) to rescan the entire local block database for transactions affecting any address or pubkey script in the wallet (including transaction affecting the newly-added address for this private key).  Set to `false` to not rescan the block database (rescanning can be performed at any time by restarting Dash Core with the `-rescan` command-line argument).  Rescanning may take several minutes.  Notes: if the address for this key is already in the wallet, the block database will not be rescanned even if this parameter is set

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | If the private key is added to the wallet (or is already part of the wallet), JSON `null` will be returned

*Example from Dash Core 0.12.2*

Import the private key for the address
ycBuREgSskHHkWLxDa9A5WppCki6PfFycL, giving it a label and scanning the
entire block chain:

```bash
dash-cli -testnet importprivkey \
              cQZZ4awQvcXXyES3CmUJqSgeTobQm9t9nyUr337kvUtsWsnvvMyw \
              "test label" \
              true
```

(Success: no result displayed.)

*See also*

* [DumpPrivKey](core-api-ref-remote-procedure-calls-wallet#dumpprivkey): returns the wallet-import-format (WIP) private key corresponding to an address. (But does not remove it from the wallet.)
* [ImportAddress](core-api-ref-remote-procedure-calls-wallet#importaddress): adds an address or pubkey script to the wallet without the associated private key, allowing you to watch for transactions affecting that address or pubkey script without being able to spend any of its outputs.
* [ImportPubKey](core-api-ref-remote-procedure-calls-wallet#importpubkey): imports a public key (in hex) that can be watched as if it were in your wallet but cannot be used to spend
* [ImportWallet](core-api-ref-remote-procedure-calls-wallet#importwallet): imports private keys from a file in wallet dump file format (see the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#dumpwallet)). These keys will be added to the keys currently in the wallet.  This call may need to rescan all or parts of the block chain for transactions affecting the newly-added keys, which may take several minutes.

# ImportPrunedFunds
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]

*Added in Dash Core 0.12.3 / Bitcoin Core 0.13.0*

The [`importprunedfunds` RPC](core-api-ref-remote-procedure-calls-wallet#importprunedfunds) imports funds without the need of a rescan. Meant for use with pruned wallets. Corresponding address or script must previously be included in wallet.
The end-user is responsible to import additional transactions that subsequently spend the imported
outputs or rescan after the point in the blockchain the transaction is included.

*Parameter #1---the raw transaction to import*

Name | Type | Presence | Description
--- | --- | --- | ---
Raw Transaction | string<br>(hex) | Required<br>(exactly 1) | A raw transaction in hex funding an already-existing address in wallet

*Parameter #2---the tx out proof that cointains the transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
TX Out Proof | string<br>(hex) | Required<br>(exactly 1) | The hex output from gettxoutproof that contains the transaction

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | If the funds are added to wallet, JSON `null` will be returned

*Example from Dash Core 0.12.3*

```bash
bitcoin-cli importprunedfunds "txhex" "txoutproof"
```

(Success: no result displayed.)

*See also*

* [ImportPrivKey](core-api-ref-remote-procedure-calls-wallet#importprivkey): adds a private key to your wallet. The key should be formatted in the wallet import format created by the [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#dumpprivkey).
* [RemovePrunedFunds](core-api-ref-remote-procedure-calls-wallet#removeprunedfunds): deletes the specified transaction from the wallet. Meant for use with pruned wallets and as a companion to importprunedfunds.

# ImportPubKey

The [`importpubkey` RPC](core-api-ref-remote-procedure-calls-wallet#importpubkey) imports a public key (in hex) that can be watched as if it were in your wallet but cannot be used to spend

*Parameter #1---the public key to import*

Name | Type | Presence | Description
--- | --- | --- | ---
Public Key | string (hex) | Required<br>(exactly 1) | The public key to import

*Parameter #2---the account into which the key should be placed*

Name | Type | Presence | Description
--- | --- | --- | ---
Label | string | Optional<br>(0 or 1) | The label the key should be assigned

*Parameter #3---whether to rescan the block chain*

Name | Type | Presence | Description
--- | --- | --- | ---
Rescan | bool | Optional<br>(0 or 1) | Set to `true` (the default) to rescan the entire local block database for transactions affecting any address or pubkey script in the wallet.  Set to `false` to not rescan the block database (rescanning can be performed at any time by restarting Dash Core with the `-rescan` command-line argument).  Rescanning may take several minutes.  Notes: if the address for this key is already in the wallet, the block database will not be rescanned even if this parameter is set

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | If the public key is added to the wallet (or is already part of the wallet), JSON `null` will be returned

*Example from Dash Core 0.12.2*

Import the public key for the address, giving it a label and scanning the
entire block chain:

```bash
dash-cli -testnet importpubkey \
    0210c1349657c1253d3d64d1b31d3500b09335bf12b8df061666e216f550a43249 \
    "test label" \
    true
```

(Success: no result displayed.)

*See also:*

* [ImportAddress](core-api-ref-remote-procedure-calls-wallet#importaddress): adds an address or pubkey script to the wallet without the associated private key, allowing you to watch for transactions affecting that address or pubkey script without being able to spend any of its outputs.
* [ImportPrivKey](core-api-ref-remote-procedure-calls-wallet#importprivkey): adds a private key to your wallet. The key should be formatted in the wallet import format created by the [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#dumpprivkey).
* [ImportWallet](core-api-ref-remote-procedure-calls-wallet#importwallet): imports private keys from a file in wallet dump file format (see the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#dumpwallet)). These keys will be added to the keys currently in the wallet.  This call may need to rescan all or parts of the block chain for transactions affecting the newly-added keys, which may take several minutes.

# ImportWallet
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**). Requires an unlocked wallet or an unencrypted wallet."
}
[/block]

The [`importwallet` RPC](core-api-ref-remote-procedure-calls-wallet#importwallet) imports private keys from a file in wallet dump file format (see the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#dumpwallet)). These keys will be added to the keys currently in the wallet.  This call may need to rescan all or parts of the block chain for transactions affecting the newly-added keys, which may take several minutes.

*Parameter #1---the file to import*

Name | Type | Presence | Description
--- | --- | --- | ---
Filename | string | Required<br>(exactly 1) | The file to import.  The path is relative to Dash Core's working directory

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | If all the keys in the file are added to the wallet (or are already part of the wallet), JSON `null` will be returned

*Example from Dash Core 0.12.2*

Import the file shown in the example subsection of the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#dumpwallet).

```bash
dash-cli -testnet importwallet /tmp/dump.txt
```

(Success: no result displayed.)

*See also*

* [DumpWallet](core-api-ref-remote-procedure-calls-wallet#dumpwallet): creates or overwrites a file with all wallet keys in a human-readable format.
* [ImportPrivKey](core-api-ref-remote-procedure-calls-wallet#importprivkey): adds a private key to your wallet. The key should be formatted in the wallet import format created by the [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#dumpprivkey).

# KeyPoolRefill
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**). Requires an unlocked wallet or an unencrypted wallet."
}
[/block]

The [`keypoolrefill` RPC](core-api-ref-remote-procedure-calls-wallet#keypoolrefill) fills the cache of unused pre-generated keys (the keypool).

*Parameter #1---the new keypool size*

Name | Type | Presence | Description
--- | --- | --- | ---
Key Pool Size | number (int) | Optional<br>(0 or 1) | The new size of the keypool; if the number of keys in the keypool is less than this number, new keys will be generated.  Default is `1000`.  The value `0` also equals the default.  The value specified is for this call only---the default keypool size is not changed

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | If the keypool is successfully filled, JSON `null` will be returned

*Example from Dash Core 0.12.2*

Generate one extra key than the default:

```bash
dash-cli -testnet keypoolrefill 1001
```

(No result shown: success.)

*See also*

* [GetNewAddress](core-api-ref-remote-procedure-calls-wallet#getnewaddress): returns a new Dash address for receiving payments. If an account is specified, payments received with the address will be credited to that account.
* [GetAccountAddress](core-api-ref-remote-procedure-calls-wallet-deprecated#getaccountaddress): returns the current Dash address for receiving payments to this account. If the account doesn't exist, it creates both the account and a new address for receiving payment.  Once a payment has been received to an address, future calls to this RPC for the same account will return a different address.
* [GetWalletInfo](core-api-ref-remote-procedure-calls-wallet#getwalletinfo): provides information about the wallet.

# ListAddressBalances

The [`listaddressbalances` RPC](core-api-ref-remote-procedure-calls-wallet#listaddressbalances) lists addresses of this wallet and their balances

*Parameter #1---Minimum Amount*

Name | Type | Presence | Description
--- | --- | --- | ---
Minimum Amount | numeric (int) | Optional<br>(0 or 1) | Minimum balance in DASH an address should have to be shown in the list (default=0)

*Result---an object containing the addresses and their balances*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object containing key/value pairs corresponding to the addresses with balances > the minimum amount.  May be empty
→<br>Address/Amount | string (base58):<br>number (DASH) | Optional<br>(1 or more) | A key/value pair with a base58check-encoded string containing the address as the key, and an amount of DASH as the value

*Example from Dash Core 0.12.3*

```bash
dash-cli -testnet listaddressbalances 25
```

Result:
```json
{
  "yMQtQkcMBXrAZyqTGZeg7tQHzmbypGEP4w": 299.99990000,
  "yRyfgrHm4f5A8GGvqpqTFvbCrCQHJm1L4V": 99.13570000,
  "ybePwhPzUbiWzFhkgxPgP6iHnTLTyFH6sU": 123.45600000,
  "ycCdPQnjNEVRgrQY8j6mxEx9h7oaQpo5Ge": 500.00000000
}
```

*See also:*

* [ListAddressGroupings](core-api-ref-remote-procedure-calls-wallet#listaddressgroupings): lists groups of addresses that may have had their common ownership made public by common use as inputs in the same transaction or from being used as change from a previous transaction.

# ListAddressGroupings
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]

The [`listaddressgroupings` RPC](core-api-ref-remote-procedure-calls-wallet#listaddressgroupings) lists groups of addresses that may have had their common ownership made public by common use as inputs in the same transaction or from being used as change from a previous transaction.

*Parameters: none*

*Result---an array of arrays describing the groupings*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array containing the groupings.  May be empty
→<br>Groupings | array | Optional<br>(0 or more) | An array containing arrays of addresses which can be associated with each other
→ →<br>Address Details | array | Required<br>(1 or more) | An array containing information about a particular address
→ → →<br>Address | string (base58) | Required<br>(exactly 1) | The address in base58check format
→ → →<br>Amount | number (bitcoins) | Required<br>(exactly 1) | The amount in DASH
→ → →<br>Label | string | Optional<br>(0 or 1) | *Replaced "Account" in Dash Core 0.17.0*<br>The label the address belongs to, if any.

*Example from Dash Core 0.17.0*

```bash
dash-cli -testnet listaddressgroupings
```

Result (edited to only three results):

```json
[
  [
    [
      "yacJKd6tRz2JSn8Wfp9GKgCbuowAEBivrA",
      10.00000000,
      "Doc test"
    ]
  ],
  [
    [
      "ye5XTjKqXyrVizEzky255NXUsaNSPC2W7k",
      5.00000000,
      ""
    ]
  ]
]
```

*See also*

* [GetAddressesByAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getaddressesbyaccount): returns a list of every address assigned to a particular account.
* [GetTransaction](core-api-ref-remote-procedure-calls-wallet#gettransaction): gets detailed information about an in-wallet transaction.
* [ListAddressBalances](core-api-ref-remote-procedure-calls-wallet#listaddressbalances): lists addresses of this wallet and their balances

# ListLabels

[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]

The [`listlabels` RPC](core-api-ref-remote-procedure-calls-wallet#listlabels) returns the list of all labels, or labels that are assigned to addresses with a specific purpose.

*Parameter #1---purpose*

Name | Type | Presence | Description
--- | --- | --- | ---
Purpose | string | Optional<br>(0 or 1) | Address purpose to list labels for (`send`, `receive`). An empty string is the same as not providing this argument.

*Result---a list of labels*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | A JSON array containing label names.  Must include, at the very least, the default account (`""`).
→<br>Label | string | Required<br>(1 or more) | The name of a label.

*Example from Dash Core 0.17.0*

Display labels used for receiving.

``` bash
dash-cli -testnet listlabels "receive"
```

Result:

``` json
[
  "",
  "Doc test"
]
```

*See also*

* [GetAddressesByLabel](core-api-ref-remote-procedure-calls-wallet#getaddressesbylabel): returns the list of addresses assigned the specified label.
* [ListReceivedByLabel](core-api-ref-remote-procedure-calls-wallet#listreceivedbylabel): lists the total number of Dash received by each label.

# ListLockUnspent
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]

The [`listlockunspent` RPC](core-api-ref-remote-procedure-calls-wallet#listlockunspent) returns a list of temporarily unspendable (locked) outputs.

*Parameters: none*

*Result---an array of locked outputs*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array containing all locked outputs.  May be empty
→<br>Output | object | Optional<br>(1 or more) | An object describing a particular locked output
→ →<br>`txid` | string (hex) | Required<br>(exactly 1) | The TXID of the transaction containing the locked output, encoded as hex in RPC byte order
→ →<br>`vout` | number (int) | Required<br>(exactly 1) | The output index number (vout) of the locked output within the transaction.  Output index `0` is the first output within the transaction

*Example from Dash Core 0.12.2*

```bash
dash-cli -testnet listlockunspent
```

Result:

```json
[
  {
    "txid": "d3d57ec5e4168b7145e911d019e9713563c1f2db5b2d6885739ea887feca4c87",
    "vout": 0
  }
]
```

*See also*

* [LockUnspent](core-api-ref-remote-procedure-calls-wallet#lockunspent): temporarily locks or unlocks specified transaction outputs. A locked transaction output will not be chosen by automatic coin selection when spending dash. Locks are stored in memory only, so nodes start with zero locked outputs and the locked output list is always cleared when a node stops or fails.

# ListReceivedByAddress
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]

The [`listreceivedbyaddress` RPC](core-api-ref-remote-procedure-calls-wallet#listreceivedbyaddress) lists the total number of dash received by each address.

*Parameter #1---the minimum number of confirmations a transaction must have to be counted*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations an externally-generated transaction must have before it is counted towards the balance.  Transactions generated by this node are counted immediately.  Typically, externally-generated transactions are payments to this wallet and transactions generated by this node are payments to other wallets.  Use `0` to count unconfirmed transactions.  Default is `1`

*Parameter #2---whether to include transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(exactly 1) | Add the balance from InstantSend locked transactions

*Parameter #3---whether to include empty accounts*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Empty | bool | Optional<br>(0 or 1) | Set to `true` to display accounts which have never received a payment.  Set to `false` (the default) to only include accounts which have received a payment.  Any account which has received a payment will be displayed even if its current balance is `0`

*Parameter #4---whether to include watch-only addresses in results*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Watch-Only | bool | Optional<br>(0 or 1) | If set to `true`, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.  If set to `false` (the default), treat watch-only addresses as if they didn't belong to this wallet

*Parameter #5---limit returned information to a specific address*

**_Added in Dash Core 0.17.0_**

Name | Type | Presence | Description
--- | --- | --- | ---
Address Filter | string | Optional<br>(0 or 1) | If present, only return information for this address

*Result---addresses, account names, balances, and minimum confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array containing objects each describing a particular address
→<br>Address | object | Optional<br>(0 or more) | An object describing an address
→ →<br>`involvesWatchonly` | bool | Optional<br>(0 or 1) | Set to `true` if this address is a watch-only address which has received a spendable payment (that is, a payment with at least the specified number of confirmations and which is not an immature coinbase).  Otherwise not returned
→ →<br>`address` | string (base58) | Required<br>(exactly 1) | The address being described encoded in base58check
→ →<br>`account` | string | Required<br>(exactly 1) | *Deprecated: will be removed in a later version of Dash Core*<br><br>The account the address belongs to.  May be the default account, an empty string (\\")"
→ →<br>`amount` | number (dash) | Required<br>(exactly 1) | The total amount the address has received in dash
→ →<br>`confirmations` | number (int) | Required<br>(exactly 1) | The number of confirmations of the latest transaction to the address.  May be `0` for unconfirmed
→ →<br>`label` | string | Required<br>(exactly 1) | The account the address belongs to.  May be the default account, an empty string (\\")"    
→ →<br>`txids` | array | Required<br>(exactly 1) | An array of TXIDs belonging to transactions that pay the address
→ → →<br>TXID | string | Optional<br>(0 or more) | The TXID of a transaction paying the address, encoded as hex in RPC byte order

*Example from Dash Core 0.13.0*

List addresses with balances confirmed by at least six blocks, including
watch-only addresses. Also include the balance from InstantSend locked transactions:

```bash
dash-cli -testnet listreceivedbyaddress 6 true false true
```

Result (edit to show only two entries):

```json
[
  {
    "address": "yV3ZTfwyfUmpspncMSitiwzh7EvqSGrqZA",
    "account": "",
    "amount": 1000.00000000,
    "confirmations": 26779,
    "label": "",
    "txids": [
      "0456aaf51a8df21dd47c2a06ede046a5bf7403bcb95d14d1d71b178c189fb933"
    ]
  },
  {
    "involvesWatchonly" : true,
    "address": "yfoR9uM3rcDfUc7AEfUNm5BjVYGFw7uQ9w",
    "account": "Watching",
    "amount": 1877.78476068,
    "confirmations": 26876,
    "label": "Watching",
    "txids": [
      "cd64114c803a2a243cb6ce4eb5c98e60cd2c688be8e900b3b957fe520cf42601",
      "83d3f7f31926908962e336341b1895d5f734f9d7149bdb35f0381cc78019bd83"
    ]
  }
]
```

*See also*

* [ListReceivedByAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#listreceivedbyaccount): lists the total number of dash received by each account.
* [GetReceivedByAddress](core-api-ref-remote-procedure-calls-wallet#getreceivedbyaddress): returns the total amount received by the specified address in transactions with the specified number of confirmations. It does not count coinbase transactions.
* [GetReceivedByAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getreceivedbyaccount): returns the total amount received by addresses in a particular account from transactions with the specified number of confirmations.  It does not count coinbase transactions.

# ListReceivedByLabel
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]
The [`listreceivedbylabel` RPC](core-api-ref-remote-procedure-calls-wallet#listreceivedbylabel) lists the total number of Dash received by each label.

*Parameter #1---the minimum number of confirmations a transaction must have to be counted*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations a  transaction must have before it is counted towards the balance. Use `0` to count unconfirmed transactions.  Default is `1`

*Parameter #2---whether to include transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(exactly 1) | Add the balance from InstantSend locked transactions

*Parameter #3---whether to include empty accounts*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Empty | bool | Optional<br>(0 or 1) | Set to `true` to display accounts which have never received a payment.  Set to `false` (the default) to only include accounts which have received a payment.  Any account which has received a payment will be displayed even if its current balance is `0`

*Parameter #4---whether to include watch-only addresses in results*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Watch-Only | bool | Optional<br>(0 or 1) | If set to `true`, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.  If set to `false` (the default), treat watch-only addresses as if they didn't belong to this wallet

*Result---account names, balances, and minimum confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array containing objects each describing an account.  At the very least, the default account (\\") will be included"
→<br>Label | object | Required<br>(1 or more) | An object describing a label
→ →<br>`involvesWatchonly` | bool | Optional<br>(0 or 1) | Set to `true` if the balance of this account includes a watch-only address which has received a spendable payment (that is, a payment with at least the specified number of confirmations and which is not an immature coinbase).  Otherwise not returned
→ →<br>`account` | string | Required<br>(exactly 1) | *Deprecated*<br>Backwards compatible alias for label
→ →<br>`amount` | number (dash) | Required<br>(exactly 1) | The total amount received by this account in dash
→ →<br>`confirmations` | number (int) | Required<br>(exactly 1) | The number of confirmations received by the last transaction received by this account.  May be `0`
→ →<br>`label` | string | Optional<br>(0 or 1) | The label of the receiving address. The default label is `""`.

*Example from Dash Core 0.17.0*

Get the balances for all non-empty accounts, including transactions which have been confirmed at least six times and InstantSend locked transactions:

``` bash
dash-cli -testnet listreceivedbylabel 6 true false true
```

Result:

``` json
[
  {
    "account": "",
    "amount": 5.00000000,
    "confirmations": 33,
    "label": ""
  },
  {
    "account": "Doc test",
    "amount": 10.00000000,
    "confirmations": 47,
    "label": "Doc test"
  }
]
```

*See also*

* [ListReceivedByAddress](core-api-ref-remote-procedure-calls-wallet#listreceivedbyaddress): lists the total number of dash received by each address.
* [GetReceivedByAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getreceivedbyaccount): returns the total amount received by addresses in a particular account from transactions with the specified number of confirmations.  It does not count coinbase transactions.
* [GetReceivedByAddress](core-api-ref-remote-procedure-calls-wallet#getreceivedbyaddress): returns the total amount received by the specified address in transactions with the specified number of confirmations. It does not count coinbase transactions.

# ListSinceBlock
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]

The [`listsinceblock` RPC](core-api-ref-remote-procedure-calls-wallet#listsinceblock) gets all transactions affecting the wallet which have occurred since a particular block, plus the header hash of a block at a particular depth.

*Parameter #1---a block header hash*

Name | Type | Presence | Description
--- | --- | --- | ---
Header Hash | string (hex) | Optional<br>(0 or 1) | The hash of a block header encoded as hex in RPC byte order.  All transactions affecting the wallet which are not in that block or any earlier block will be returned, including unconfirmed transactions.  Default is the hash of the genesis block, so all transactions affecting the wallet are returned by default

*Parameter #2---the target confirmations for the lastblock field*

Name | Type | Presence | Description
--- | --- | --- | ---
Target Confirmations | number (int) | Optional<br>(0 or 1) | Sets the lastblock field of the results to the header hash of a block with this many confirmations.  This does not affect which transactions are returned.  Default is `1`, so the hash of the most recent block on the local best block chain is returned

*Parameter #3---whether to include watch-only addresses in details and calculations*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Watch-Only | bool | Optional<br>(0 or 1) | If set to `true`, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.  If set to `false` (the default), treat watch-only addresses as if they didn't belong to this wallet

*Parameter #4---include_removed*

Name | Type | Presence | Description
--- | --- | --- | ---
include_removed | bool | Optional<br>Default=`true` | Show transactions that were removed due to a reorg in the \removed\" array (not guaranteed to work on pruned nodes)"

**Result**

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object containing an array of transactions and the lastblock field
→<br>`transactions` | array | Required<br>(exactly 1) | An array of objects each describing a particular **payment** to or from this wallet.  The objects in this array do not describe an actual transactions, so more than one object in this array may come from the same transaction.  This array may be empty
→ →<br>Payment | object | Optional<br>(0 or more) | An payment which did not appear in the specified block or an earlier block
→ <br>`involvesWatchonly` | bool | Optional<br>(0 or 1) | Set to `true` if the payment involves a watch-only address.  Otherwise not returned
→ <br>`account` | string | Required<br>(exactly 1) | **This field will be removed in V0.18. To see this deprecated field, start dashd with `-deprecatedrpc=accounts`.**<br><br>The account which the payment was credited to or debited from.  May be an empty string (\\") for the default account"
→ <br>`address` | string (base58) | Optional<br>(0 or 1) | The address paid in this payment, which may be someone else's address not belonging to this wallet.  May be empty if the address is unknown, such as when paying to a non-standard pubkey script
→ <br>`category` | string | Required<br>(exactly 1) | Set to one of the following values:<br>• `send` if sending payment normally<br>• `coinjoin` if sending CoinJoin payment<br>• `receive` if this wallet received payment in a regular transaction<br>• `generate` if a matured and spendable coinbase<br>• `immature` if a coinbase that is not spendable yet<br>• `orphan` if a coinbase from a block that's not in the local best block chain
→ <br>`amount` | number (dash) | Required<br>(exactly 1) | A negative dash amount if sending payment; a positive dash amount if receiving payment (including coinbases)
→ <br>`vout` | number (int) | Required<br>(exactly 1) | For an output, the output index (vout) for this output in this transaction.  For an input, the output index for the output being spent in its transaction.  Because inputs list the output indexes from previous transactions, more than one entry in the details array may have the same output index
→ <br>`fee` | number (dash) | Optional<br>(0 or 1) | If sending payment, the fee paid as a negative dash value.  May be `0`. Not returned if receiving payment
→ <br>`confirmations` | number (int) | Required<br>(exactly 1) | The number of confirmations the transaction has received.  Will be `0` for unconfirmed and `-1` for conflicted
→ <br>`instantlock` | bool | Required<br>(exactly 1) | **Always `false` if [lite mode](core-guide-dash-features#lite-mode) is enabled**<br><br>Current transaction lock state (InstantSend and/or ChainLock)
→ <br>`instantlock_internal` | bool | Required<br>(exactly 1) | **Always `false` if [lite mode](core-guide-dash-features#lite-mode) is enabled**<br><br>Current InstantSend transaction lock state
→ <br>`chainlock` | bool | Required<br>(exactly 1) | *Added in Dash Core 0.14.0*<br><br>**Always `false` if [lite mode](core-guide-dash-features#lite-mode) is enabled**<br><br>If set to `true`, this transaction is in a block that is locked (not susceptible to a chain re-org)  
→ <br>`generated` | bool | Optional<br>(0 or 1) | Set to `true` if the transaction is a coinbase.  Not returned for regular transactions
→ <br>`blockhash` | string (hex) | Optional<br>(0 or 1) | The hash of the block on the local best block chain which includes this transaction, encoded as hex in RPC byte order.  Only returned for confirmed transactions
→ <br>`blockindex` | number (int) | Optional<br>(0 or 1) | The index of the transaction in the block that includes it.  Only returned for confirmed transactions
→ <br>`blocktime` | number (int) | Optional<br>(0 or 1) | The block header time (Unix epoch time) of the block on the local best block chain which includes this transaction.  Only returned for confirmed transactions
→ <br>`txid` | string (hex) | Required<br>(exactly 1) | The TXID of the transaction, encoded as hex in RPC byte order
→ <br>`walletconflicts` | array | Required<br>(exactly 1) | An array containing the TXIDs of other transactions that spend the same inputs (UTXOs) as this transaction.  Array may be empty
→ →<br>TXID | string (hex) | Optional<br>(0 or more) | The TXID of a conflicting transaction, encoded as hex in RPC byte order
→ <br>`time` | number (int) | Required<br>(exactly 1) | A Unix epoch time when the transaction was added to the wallet
→ <br>`timereceived` | number (int) | Required<br>(exactly 1) | A Unix epoch time when the transaction was detected by the local node, or the time of the block on the local best block chain that included the transaction
→ <br>`abandoned` | bool | Optional<br>(0 or 1) | `true` if the transaction has been abandoned (inputs are respendable). Only available for the 'send' category of transactions.
→ <br>`comment` | string | Optional<br>(0 or 1) | For transaction originating with this wallet, a locally-stored comment added to the transaction.  Only returned if a comment was added
→ <br>`to` | string | Optional<br>(0 or 1) | For transaction originating with this wallet, a locally-stored comment added to the transaction identifying who the transaction was sent to.  Only returned if a comment-to was added
→<br>`removed` | array | Optional<br>(0 or 1) | Structure is the same as `transactions`. Only present if `include_removed` is `true`.<br>_Note_: transactions that were re-added in the active chain will appear as-is in this array, and may thus have a positive confirmation count.
→<br>`lastblock` | string (hex) | Required<br>(exactly 1) | The header hash of the block with the number of confirmations specified in the *target confirmations* parameter, encoded as hex in RPC byte order

*Example from Dash Core 0.17.0*

Get all transactions since a particular block (including watch-only transactions) and the header hash of the sixth most recent block.

```bash
dash-cli -testnet listsinceblock \
              0000000001fc119ea77e0c67783227fb9d55386125179ea5597109d311af2337 \
              6 true true
```

Result (edited to show only two payments):

```json
{
  "transactions": [
    {
      "address": "yYYFByVqsqCNfJ6J8i7rWZuW7a6paphxTM",
      "category": "generate",
      "amount": 11.25000784,
      "label": "Mining",
      "vout": 0,
      "confirmations": 217129,
      "instantlock": true,
      "instantlock_internal": false,
      "chainlock": true,
      "generated": true,
      "blockhash": "00000d6fab6ee232096d3c33c813684e249e6a23e5e42b25122fd3a60249fec4",
      "blockindex": 0,
      "blocktime": 1573452255,
      "txid": "f1ebc584d6aa9c3750ed3bff8e794c190f7046ac86386b6e61c05886c3bd0200",
      "walletconflicts": [
      ],
      "time": 1573452255,
      "timereceived": 1573478830
    },
    {
      "address": "yPAaweTvCYXaoUwxMFJ31sTdKiARUBZYhd",
      "category": "send",
      "amount": -10.00010000,
      "vout": 0,
      "fee": 10.00010000,
      "confirmations": 36635,
      "instantlock": true,
      "instantlock_internal": false,
      "chainlock": true,
      "blockhash": "0000068eee543783408586f3895afe45839ed1bf6f586566cd56f0449fd1f5ce",
      "blockindex": 11,
      "blocktime": 1601325843,
      "txid": "3b8d8ff1d539391c8dfb176bda2c012c08924ac0fbbc098c890412e22af80800",
      "walletconflicts": [
      ],
      "time": 1601325735,
      "timereceived": 1601325735,
      "abandoned": false
    }
  ],
  "lastblock": "000001f419feac4ce2ec47190b5d380ce137f35f3a3e13b322ff89428fe75f2c"
}
```

*See also*

* [ListReceivedByAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#listreceivedbyaccount): lists the total number of dash received by each account.
* [ListReceivedByAddress](core-api-ref-remote-procedure-calls-wallet#listreceivedbyaddress): lists the total number of dash received by each address.

# ListTransactions
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]

The [`listtransactions` RPC](core-api-ref-remote-procedure-calls-wallet#listtransactions) returns the most recent transactions that affect the wallet. If a label name is provided, this will return only incoming transactions paying to addresses with the specified label.

*Parameter #1---a label name*

Name | Type | Presence | Description
--- | --- | --- | ---
Label | string | Optional<br>(0 or 1) | If set, should be a valid label name to return only incoming transactions with the specified label, or `"*"` to disable filtering and return all transactions.

*Parameter #2---the number of transactions to get*

Name | Type | Presence | Description
--- | --- | --- | ---
Count | number (int) | Optional<br>(0 or 1) | The number of the most recent transactions to list.  Default is `10`

*Parameter #3---the number of transactions to skip*

Name | Type | Presence | Description
--- | --- | --- | ---
Skip | number (int) | Optional<br>(0 or 1) | The number of the most recent transactions which should not be returned.  Allows for pagination of results.  Default is `0`

*Parameter #4---whether to include watch-only addresses in details and calculations*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Watch-Only | bool | Optional<br>(0 or 1) | If set to `true`, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.  If set to `false` (the default), treat watch-only addresses as if they didn't belong to this wallet

*Result---payment details*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array containing objects, with each object describing a **payment** or internal accounting entry (not a transaction).  More than one object in this array may come from a single transaction.  Array may be empty
→<br>Payment | object | Optional<br>(0 or more) | A payment or internal accounting entry
→ →<br>`address` | string (base58) | Optional<br>(0 or 1) | The address paid in this payment, which may be someone else's address not belonging to this wallet.  May be empty if the address is unknown, such as when paying to a non-standard pubkey script or if this is in the *move* category
→ →<br>`category` | string | Required<br>(exactly 1) | Set to one of the following values:<br>• `send` if sending payment<br>• `receive` if this wallet received payment in a regular transaction<br>• `generate` if a matured and spendable coinbase<br>• `immature` if a coinbase that is not spendable yet<br>• `orphan` if a coinbase from a block that's not in the local best block chain<br>• `move` if an off-block-chain move made with the [`move` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#move)
→ →<br>`amount` | number (dash) | Required<br>(exactly 1) | A negative dash amount if sending payment; a positive dash amount if receiving payment (including coinbases)
→ →<br>`label` | string | Optional<br>(0 or 1) | A comment for the address/transaction  
→ →<br>`vout` | number (int) | Optional<br>(0 or 1) | For an output, the output index (vout) for this output in this transaction.  For an input, the output index for the output being spent in its transaction.  Because inputs list the output indexes from previous transactions, more than one entry in the details array may have the same output index.  Not returned for *move* category payments
→ →<br>`fee` | number (dash) | Optional<br>(0 or 1) | If sending payment, the fee paid as a negative dash value.  May be `0`. Not returned if receiving payment or for *move* category payments
→ →<br>`confirmations` | number (int) | Optional<br>(0 or 1) | The number of confirmations the transaction has received.  Will be `0` for unconfirmed and `-1` for conflicted.  Not returned for *move* category payments
→<br>`instantlock` | bool | Required<br>(exactly 1) | **Always `false` if [lite mode](core-guide-dash-features#lite-mode) is enabled**<br><br>Current transaction lock state (InstantSend and/or ChainLock)  
→<br>`instantlock_internal` | bool | Required<br>(exactly 1) | **Always `false` if [lite mode](core-guide-dash-features#lite-mode) is enabled**<br><br>Current InstantSend transaction lock state
<br>`chainlock` | bool | Required<br>(exactly 1) | *Added in Dash Core 0.14.0*<br><br>**Always `false` if [lite mode](core-guide-dash-features#lite-mode) is enabled**<br><br>If set to `true`, this transaction is in a block that is locked (not susceptible to a chain re-org)
→ →<br>`generated` | bool | Optional<br>(0 or 1) | Set to `true` if the transaction is a coinbase.  Not returned for regular transactions or *move* category payments
→ →<br>`trusted` | bool | Optional<br>(0 or 1) | Indicates whether we consider the outputs of this unconfirmed transaction safe to spend.  Only returned for unconfirmed transactions
→ →<br>`blockhash` | string (hex) | Optional<br>(0 or 1) | The hash of the block on the local best block chain which includes this transaction, encoded as hex in RPC byte order.  Only returned for confirmed transactions
→ →<br>`blockindex` | number (int) | Optional<br>(0 or 1) | The index of the transaction in the block that includes it.  Only returned for confirmed transactions
→ →<br>`blocktime` | number (int) | Optional<br>(0 or 1) | The block header time (Unix epoch time) of the block on the local best block chain which includes this transaction.  Only returned for confirmed transactions
→ →<br>`txid` | string (hex) | Optional<br>(0 or 1) | The TXID of the transaction, encoded as hex in RPC byte order.  Not returned for *move* category payments
→ →<br>`walletconflicts` | array | Optional<br>(0 or 1) | An array containing the TXIDs of other transactions that spend the same inputs (UTXOs) as this transaction.  Array may be empty.  Not returned for *move* category payments
→ → →<br>TXID | string (hex) | Optional<br>(0 or more) | The TXID of a conflicting transaction, encoded as hex in RPC byte order
→ →<br>`time` | number (int) | Required<br>(exactly 1) | A Unix epoch time when the transaction was added to the wallet
→ →<br>`timereceived` | number (int) | Optional<br>(0 or 1) | A Unix epoch time when the transaction was detected by the local node, or the time of the block on the local best block chain that included the transaction.  Not returned for *move* category payments
→ →<br>`comment` | string | Optional<br>(0 or 1) | For transaction originating with this wallet, a locally-stored comment added to the transaction.  Only returned in regular payments if a comment was added.  Always returned in *move* category payments.  May be an empty string
→ →<br>`to` | string | Optional<br>(0 or 1) | For transaction originating with this wallet, a locally-stored comment added to the transaction identifying who the transaction was sent to.  Only returned if a comment-to was added.  Never returned by *move* category payments.  May be an empty string
→ →<br>`otheraccount` | string | Optional<br>(0 or 1) | This is the account the dash were moved from or moved to, as indicated by a negative or positive *amount* field in this payment.  Only returned by *move* category payments
→ →<br>`abandoned` | bool | Optional<br>(0 or 1) | *Added in Bitcoin Core 0.12.1*<br><br>Indicates if a transaction is was abandoned:<br>• `true` if it was abandoned (inputs are respendable)<br>• `false`  if it was not abandoned<br>Only returned by *send* category payments

*Example from Dash Core 0.17.0*

List the most recent transaction including watch-only addresses.

```bash
dash-cli listtransactions "*" 1 0 true
```

Result:

```json
[
  {
    "address": "ySjAeJ7CmHZrkXaxv54X9ZnPZqAgTSabhK",
    "category": "send",
    "amount": -0.50000000,
    "label": "",
    "vout": 0,
    "fee": -0.00000226,
    "confirmations": 14,
    "instantlock": true,
    "instantlock_internal": false,
    "chainlock": true,
    "blockhash": "000000a37134cb52e7310f651131a6c168e7c0ef87f84260b7811c202e011f0f",
    "blockindex": 2,
    "blocktime": 1610645212,
    "txid": "06697c9f9c713473ba00b3d96118546b88d1da89adaae89ea6b760fea23ebad0",
    "walletconflicts": [
    ],
    "time": 1610645190,
    "timereceived": 1610645190,
    "abandoned": false
  }
]
```

*See also*

* [GetTransaction](core-api-ref-remote-procedure-calls-wallet#gettransaction): gets detailed information about an in-wallet transaction.
* [ListSinceBlock](core-api-ref-remote-procedure-calls-wallet#listsinceblock): gets all transactions affecting the wallet which have occurred since a particular block, plus the header hash of a block at a particular depth.

# ListUnspent
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]

The [`listunspent` RPC](core-api-ref-remote-procedure-calls-wallet#listunspent) returns an array of unspent transaction outputs belonging to this wallet. **Note:** as of Bitcoin
Core 0.10.0, outputs affecting watch-only addresses will be returned; see
the *spendable* field in the results described below.

*Parameter #1---the minimum number of confirmations an output must have*

Name | Type | Presence | Description
--- | --- | --- | ---
Minimum Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations the transaction containing an output must have in order to be returned.  Use `0` to return outputs from unconfirmed transactions. Default is `1`

*Parameter #2---the maximum number of confirmations an output may have*

Name | Type | Presence | Description
--- | --- | --- | ---
Maximum Confirmations | number (int) | Optional<br>(0 or 1) | The maximum number of confirmations the transaction containing an output may have in order to be returned.  Default is `9999999` (~10 million)

*Parameter #3---the addresses an output must pay*

Name | Type | Presence | Description
--- | --- | --- | ---
Addresses | array | Optional<br>(0 or 1) | If present, only outputs which pay an address in this array will be returned
→<br>Address | string (base58) | Required<br>(1 or more) | A P2PKH or P2SH address

*Parameter #4---include unsafe outputs*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Unsafe | bool | Optional<br>(false or true) | Include outputs that are not safe to spend . See description of `safe` attribute below.  Default is `true`

*Parameter #5---query options*

Name | Type | Presence | Description
--- | --- | --- | ---
Query Options | json | Optional | JSON with query options. Available options:<br> - `minimumAmount`: Minimum value of each UTXO in DASH<br> - `maximumAmount`: Maximum value of each UTXO in DASH<br> - `maximumCount`: Maximum number of UTXOs<br> - `minimumSumAmount`: Minimum sum value of all UTXOs in DASH<br> - `cointType`: Filter coinTypes as follows:<br>0 = `ALL_COINS`, <br>1 = `ONLY_FULLY_MIXED`, <br>2 = `ONLY_READY_TO_MIX`, <br>3 = `ONLY_NONDENOMINATED`, <br>4 = `ONLY_MASTERNODE_COLLATERAL`, <br>5 = `ONLY_COINJOIN_COLLATERAL`

*Result---the list of unspent outputs*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array of objects each describing an unspent output.  May be empty
→<br>Unspent Output | object | Optional<br>(0 or more) | An object describing a particular unspent output belonging to this wallet
→ →<br>`txid` | string (hex) | Required<br>(exactly 1) | The TXID of the transaction containing the output, encoded as hex in RPC byte order
→ →<br>`vout` | number (int) | Required<br>(exactly 1) | The output index number (vout) of the output within its containing transaction
→ →<br>`address` | string (base58) | Optional<br>(0 or 1) | The P2PKH or P2SH address the output paid.  Only returned for P2PKH or P2SH output scripts
→ →<br>`account` | string | Optional<br>(0 or 1) | **This field will be removed in V0.18. To see this deprecated field, start dashd with `-deprecatedrpc=accounts`.**<br><br>If the address returned belongs to an account, this is the account.  Otherwise not returned
→ →<br>`scriptPubKey` | string (hex) | Required<br>(exactly 1) | The output script paid, encoded as hex
→ →<br>`redeemScript` | string (hex) | Optional<br>(0 or 1) | If the output is a P2SH whose script belongs to this wallet, this is the redeem script
→ →<br>`amount` | number (int) | Required<br>(exactly 1) | The amount paid to the output in dash
→ →<br>`confirmations` | number (int) | Required<br>(exactly 1) | The number of confirmations received for the transaction containing this output
→ →<br>`spendable` | bool | Required<br>(exactly 1) | Set to `true` if the private key or keys needed to spend this output are part of the wallet.  Set to `false` if not (such as for watch-only addresses)
→ →<br>`solvable` | bool | Required<br>(exactly 1) | *Added in Bitcoin Core 0.13.0*<br><br>Set to `true` if the wallet knows how to spend this output.  Set to `false` if the wallet does not know how to spend the output.  It is ignored if the private keys are available
→ →<br>`desc` | string | Optional<br>(0 or 1) | A descriptor for spending this output
→ →<br>`coinjoin_rounds` | number (int) | Required<br>(exactly 1) | The number of rounds
→ →<br>`safe` | bool | Required<br>(exactly 1) | *Added in Bitcoin Core 0.15.0*<br><br>Whether this output is considered safe to spend. Unconfirmed transactions from outside keys are considered unsafe and are not eligible for spending by `fundrawtransaction` and `sendtoaddress`.

*Example from Dash Core 0.18.0*

Get all outputs confirmed at least 6 times for a particular address:

```bash
dash-cli -testnet listunspent 6 99999999 '''
  [
    "yYvsn6vdnkeq9VG1JbkfqKbjv3gUmFmnny"
  ]
'''
```

Result:

```json
[
  {
    "txid": "8181ac201ef2ff896f666589e98924f7cbbc885f1c42c766629f677e37cafc47",
    "vout": 0,
    "address": "yYvsn6vdnkeq9VG1JbkfqKbjv3gUmFmnny",
    "scriptPubKey": "76a9148a54e0c51084f0e5819a66bb1c4d01191f5caa3888ac",
    "amount": 0.01274051,
    "confirmations": 16007,
    "spendable": true,
    "solvable": true,
    "desc": "pkh([8a54e0c5]0214889c34100d00aca6e7cbfe0fa72d83c28857585740bff5f3db6b37e51d9aaa)#wyvgzv2k",
    "safe": true,
    "coinjoin_rounds": -2
  }
]
```

Get all outputs for a particular address that have at least 1 confirmation and a maximum value of 10:

```shell
dash-cli -testnet listunspent 1 9999999 '''
  [
    "yYvsn6vdnkeq9VG1JbkfqKbjv3gUmFmnny"
  ]
  ''' true '''
  {
    "maximumAmount": "10"
  }
  '''
```

Result:

```json
[
  {
    "txid": "8181ac201ef2ff896f666589e98924f7cbbc885f1c42c766629f677e37cafc47",
    "vout": 0,
    "address": "yYvsn6vdnkeq9VG1JbkfqKbjv3gUmFmnny",
    "scriptPubKey": "76a9148a54e0c51084f0e5819a66bb1c4d01191f5caa3888ac",
    "amount": 0.01274051,
    "confirmations": 16008,
    "spendable": true,
    "solvable": true,
    "desc": "pkh([8a54e0c5]0214889c34100d00aca6e7cbfe0fa72d83c28857585740bff5f3db6b37e51d9aaa)#wyvgzv2k",
    "safe": true,
    "coinjoin_rounds": -2
  }
]
```

*See also*

* [ListTransactions](core-api-ref-remote-procedure-calls-wallet#listtransactions): returns the most recent transactions that affect the wallet.
* [LockUnspent](core-api-ref-remote-procedure-calls-wallet#lockunspent): temporarily locks or unlocks specified transaction outputs. A locked transaction output will not be chosen by automatic coin selection when spending dash. Locks are stored in memory only, so nodes start with zero locked outputs and the locked output list is always cleared when a node stops or fails.

# ListWallets

The [`listwallets` RPC](core-api-ref-remote-procedure-calls-wallet#listwallets) returns a list of currently loaded wallets.

For full information on the wallet, use the [`getwalletinfo` RPC](core-api-ref-remote-procedure-calls-wallet#getwalletinfo).

*Parameters: none*

*Result*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array of strings containing a list of currently loaded wallet files
→<br>Wallet | string | Required<br>(0 or more) | The name of a wallet file

*Example from Dash Core 0.15.0*

```bash
dash-cli -testnet listwallets
```

Result:
```json
[
  "wallet.dat"
]
```

*See also: none*

# ListWalletDir

The [`listwalletdir` RPC](core-api-ref-remote-procedure-calls-wallet#listwalletdir) returns a list of wallets in the wallet directory.

For full information on the wallet, use the [`getwalletinfo` RPC](core-api-ref-remote-procedure-calls-wallet#getwalletinfo).

*Parameters: none*

*Result*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | A JSON array of objects containing a list of wallets in the wallet directory.
→<br>name | string | Required<br>(0 or more) | The wallet name

*Example from Dash Core 0.18.0*

```bash
dash-cli -testnet listwalletdir
```

Result:
```json
{
  "wallets": [
    {
      "name": "demo"
    }
  ]
}
```

*See also*

* [GetWalletInfo](core-api-ref-remote-procedure-calls-wallet#getwalletinfo): provides information about the wallet.

# LoadWallet
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]
The [`loadwallet` RPC](core-api-ref-remote-procedure-calls-wallet#loadwallet) loads a wallet from a wallet file or directory. Note that all wallet command-line options used when starting dashd will be applied to the new wallet (eg -zapwallettxes, upgradewallet, rescan, etc).

*Parameter #1---wallet name*

Name | Type | Presence | Description
--- | --- | --- | ---
Filename | string | Required<br>(exactly 1) | The wallet directory or .dat file. The wallet can be specified as file/directory basename (which must be located in the `walletdir` directory), or as an absolute path to a file/directory.

*Result---operation status*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object containing the wallet name or warning message related to the operation
→<br>`name` | string | Required | The wallet name if loaded successfully
→<br>`warning` | string | Required | Warning message if wallet was not loaded cleanly
*Example from Dash Core 0.16.0*

```bash
dash-cli -testnet loadwallet wallet-test.dat
```

Result:

```json
{
  "name": "wallet-test.dat",
  "warning": ""
}
```

*See also*

* [CreateWallet](core-api-ref-remote-procedure-calls-wallet#createwallet): creates and loads a new wallet.
* [UnloadWallet](core-api-ref-remote-procedure-calls-wallet#unloadwallet): unloads the specified wallet.

# LockUnspent
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]

The [`lockunspent` RPC](core-api-ref-remote-procedure-calls-wallet#lockunspent) temporarily locks or unlocks specified transaction outputs. A locked transaction output will not be chosen by automatic coin selection when spending dash. Locks are stored in memory only, so nodes start with zero locked outputs and the locked output list is always cleared when a node stops or fails.

*Parameter #1---whether to lock or unlock the outputs*

Name | Type | Presence | Description
--- | --- | --- | ---
Unlock | bool | Required<br>(exactly 1) | Set to `false` to lock the outputs specified in the following parameter.  Set to `true` to unlock the outputs specified.  If this is the only argument specified and it is set to `true`, all outputs will be unlocked; if it is the only argument and is set to `false`, there will be no change

*Parameter #2---the outputs to lock or unlock*

Name | Type | Presence | Description
--- | --- | --- | ---
Outputs | array | Optional<br>(0 or 1) | An array of outputs to lock or unlock
→<br>Output | object | Required<br>(1 or more) | An object describing a particular output
→ →<br>`txid` | string | Required<br>(exactly 1) | The TXID of the transaction containing the output to lock or unlock, encoded as hex in internal byte order
→ →<br>`vout` | number (int) | Required<br>(exactly 1) | The output index number (vout) of the output to lock or unlock.  The first output in a transaction has an index of `0`

*Result---`true` if successful*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | bool | Required<br>(exactly 1) | Set to `true` if the outputs were successfully locked or unlocked.  If the outputs were already locked or unlocked, it will also return `true`

*Example from Dash Core 0.12.2*

Lock two outputs:

```bash
dash-cli -testnet lockunspent false '''
  [
    {
      "txid": "d3d57ec5e4168b7145e911d019e9713563c1f2db5b2d6885739ea887feca4c87",
      "vout": 0
    },
    {
      "txid": "607897611b2f7c5b23297b2a352a8d6f4383f8d0782585f93220082d361f8db9",
      "vout": 1
    }
  ]
'''
```

Result:

```json
true
```

Verify the outputs have been locked:

```bash
dash-cli -testnet listlockunspent
```

Result

```json
[
  {
    "txid": "d3d57ec5e4168b7145e911d019e9713563c1f2db5b2d6885739ea887feca4c87",
    "vout": 0
  },
  {
    "txid": "607897611b2f7c5b23297b2a352a8d6f4383f8d0782585f93220082d361f8db9",
    "vout": 1
  }
]
```

Unlock one of the above outputs:

```bash
dash-cli -testnet lockunspent true '''
[
  {
    "txid": "607897611b2f7c5b23297b2a352a8d6f4383f8d0782585f93220082d361f8db9",
    "vout": 1
  }
]
'''
```

Result:

```json
true
```

Verify the output has been unlocked:

```bash
dash-cli -testnet listlockunspent
```

Result:

```json
[
  {
    "txid": "d3d57ec5e4168b7145e911d019e9713563c1f2db5b2d6885739ea887feca4c87",
    "vout": 0
  }
]
```

*See also*

* [ListLockUnspent](core-api-ref-remote-procedure-calls-wallet#listlockunspent): returns a list of temporarily unspendable (locked) outputs.
* [ListUnspent](core-api-ref-remote-procedure-calls-wallet#listunspent): returns an array of unspent transaction outputs belonging to this wallet.

# RemovePrunedFunds
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]

*Added in Dash Core 0.12.3 / Bitcoin Core 0.13.0*

The [`removeprunedfunds` RPC](core-api-ref-remote-procedure-calls-wallet#removeprunedfunds) deletes the specified transaction from the wallet. Meant for use with pruned wallets and as a companion to importprunedfunds. This will effect wallet balances.

*Parameter #1---the raw transaction to import*

Name | Type | Presence | Description
--- | --- | --- | ---
TXID | string<br>(hex) | Required<br>(exactly 1) | The hex-encoded id of the transaction you are removing

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | If the funds are removed from the wallet, JSON `null` will be returned

*Example from Dash Core 0.12.3*

```bash
dash-cli removeprunedfunds bb7daff525b83fa6a847ab50bf7f8f14d6\
22761a19f69157b362ef3f25bda687
```

(Success: no result displayed.)

*See also*

* [ImportPrivKey](core-api-ref-remote-procedure-calls-wallet#importprivkey): adds a private key to your wallet. The key should be formatted in the wallet import format created by the [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#dumpprivkey).
* [ImportPrunedFunds](core-api-ref-remote-procedure-calls-wallet#importprunedfunds): imports funds without the need of a rescan. Meant for use with pruned wallets.


# RescanBlockChain

The [`rescanblockchain` RPC](core-api-ref-remote-procedure-calls-wallet#rescanblockchain) rescans the local blockchain for wallet related transactions.

*Parameter #1---the start block height*

Name | Type | Presence | Description
--- | --- | --- | ---
Start Height | integer | Optional<br>(0 or 1) | The block height where the rescan should start

*Parameter #2---the stop block height*

Name | Type | Presence | Description
--- | --- | --- | ---
Stop Height | integer | Optional<br>(0 or 1) | The last block height that should be scanned

*Result---`null` or start/end height details if parameters provided*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object containing the start/end heights depending on the range of blocks scanned
→<br>`start_height` | integer | Optional<br>(0 or 1) | The block height where the rescan has started. If omitted, rescan started from the genesis block
→<br>`stop_height` | integer | Optional<br>(0 or 1) | The height of the last rescanned block. If omitted, rescan stopped at the chain tip

*Example from Dash Core 0.16.0*

```bash
dash-cli rescanblockchain
```

Result:
```json
{
  "start_height": 293600,
  "stop_height": 293602
}
```

*See also*

* [AbortRescan](core-api-ref-remote-procedure-calls-wallet#abortrescan): stops current wallet rescan.

# ScanTXOutset
[block:callout]
{
  "type": "danger",
  "body": "Experimental warning: this call may be removed or changed in future releases."
}
[/block]
The [`scantxoutset` RPC](core-api-ref-remote-procedure-calls-wallet#scantxoutset) scans the unspent transaction output set for entries that match certain output descriptors.

*Parameter #1---action*

Name | Type | Presence | Description
--- | --- | --- | ---
Action | string | Required<br>(exactly 1) | The action to execute:<br> - "start" for starting a scan,<br> - "abort" for aborting the current scan (returns true when abort was successful),<br> - "status" for progress report (in %) of the current scan

*Parameter #2---scan objects*

Name | Type | Presence | Description
--- | --- | --- | ---
Scan Objects | array | Required<br>(exactly 1) | An array of scan objects. Every scan object is either a string descriptor or an object.
→<br>descriptor | string: object | Required<br>(1 or more) | An output descriptor; an object with output descriptor and metadata.<br>Examples of output descriptors are:<br> - `addr(<address>)`: Outputs whose scriptPubKey corresponds to the specified address (does not include P2PK)<br> - `raw(<hex script>)`: Outputs whose scriptPubKey equals the specified hex scripts<br> - `combo(<pubkey>)`: P2PK and P2PKH outputs for the given pubkey<br> - `pkh(<pubkey>)`: P2PKH outputs for the given pubkey<br> - `sh(multi(<n>,<pubkey>,<pubkey>,...))`: P2SH-multisig outputs for the given threshold and pubkeys
→ →<br>desc | string | Required<br>(exactly 1) | An output descriptor
→ →<br>range | number (int) | Optional<br>(0 or 1) | The child index HD that chains should be explored (default: 1000)

In the above, <pubkey> either refers to a fixed public key in hexadecimal notation, or to an xpub/xprv optionally followed by one or more path elements separated by "/", and optionally ending in "/*" (unhardened), or "/*'" or "/*h" (hardened) to specify all unhardened or hardened child keys. In the latter case, a range needs to be specified by below if different from 1000.

*Result---The unspent and total amount*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object containing the the unspent and total amounts.
→<br>`unspents` | array | Required<br>(exactly 1) | An array containing unspent output objects
→ →<br>Unspent output | array | Required<br>(1 or more) | An object containing unspent output information
→ → →<br>`txid` | string (hex) | Required<br>(exactly 1) | The TXID of the transaction the output appeared in.  The TXID must be encoded in hex in RPC byte order
→ → →<br>`vout` | number (int) | Required<br>(exactly 1) | The index number of the output (vout) as it appeared in its transaction, with the first output being 0
→ → →<br>`scriptPubKey` | string (hex) | Required<br>(exactly 1) | The output's pubkey script encoded as hex
→ → →<br>`desc` | string | Required<br>(exactly 1) | A specialized descriptor for the matched scriptPubKey
→ → →<br>`amount` | number (int) | Required<br>(exactly 1) | The total amount in DASH of the unspent output
→ → →<br>`height` | number (int) | Required<br>(exactly 1) | The height of the unspent transaction output
→<br>`total_amount` | numeric | Required<br>(exactly 1) | The total amount of all found unspent outputs in DASH

*Example from Dash Core 0.18.0*

```bash
dash-cli -testnet scantxoutset start '["addr(yWjoZBvnUKWhpKMbBkVVnnMD8Bzno9j6tQ)"]'
```

Result:
```json
{
  "success": true,
  "searched_items": 572864,
  "unspents": [
    {
      "txid": "571028a9a2f69c5eec75dbae10c8724b8afd44530fac97936ae6676a9c61e03c",
      "vout": 0,
      "scriptPubKey": "76a914724c86a5dc23ecac05474d9be3ac76a6aa4bcb4488ac",
      "desc": "addr(yWjoZBvnUKWhpKMbBkVVnnMD8Bzno9j6tQ)#sycxjztu",
      "amount": 1.00000000,
      "height": 494777
    },
    {
      "txid": "3e76165230a3ff5bb8df0a9e278caa81f9a653c2b7a075f8dc16e56103c8f68e",
      "vout": 0,
      "scriptPubKey": "76a914724c86a5dc23ecac05474d9be3ac76a6aa4bcb4488ac",
      "desc": "addr(yWjoZBvnUKWhpKMbBkVVnnMD8Bzno9j6tQ)#sycxjztu",
      "amount": 7.76020488,
      "height": 494777
    }
  ],
  "total_amount": 8.76020488
}
```

*See also*

* [ListUnspent](/docs/core-api-ref-remote-procedure-calls-wallet#listunspent): returns an array of unspent transaction outputs belonging to this wallet.

# SendMany
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**). Requires an unlocked wallet or an unencrypted wallet."
}
[/block]

The [`sendmany` RPC](core-api-ref-remote-procedure-calls-wallet#sendmany) creates and broadcasts a transaction which sends outputs to multiple addresses.

*Parameter #1---unused parameter*
[block:callout]
{
  "type": "warning",
  "body": "Note that this parameter has been removed in v0.17. To use this RPC with a \"fromaccount\" parameter, restart dashd with `-deprecatedrpc=accounts`."
}
[/block]
Name | Type | Presence | Description
--- | --- | --- | ---
Unused | string | Required<br>(exactly 1) | **Deprecated: (previously account) will be removed in a later version of Dash Core**<br><br>Must be set to `""` for backwards compatibility. 

*Parameter #2---the addresses and amounts to pay*

Name | Type | Presence | Description
--- | --- | --- | ---
Outputs | object | Required<br>(exactly 1) | An object containing key/value pairs corresponding to the addresses and amounts to pay
→<br>Address/Amount | string (base58) : number (dash) | Required<br>(1 or more) | A key/value pair with a base58check-encoded string containing the P2PKH or P2SH address to pay as the key, and an amount of dash to pay as the value

*Parameter #3---minimum confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | *Deprecated and ignored since Dash Core 0.18.0*

*Parameter #4--whether to add the balance from transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(0 or 1) | *Deprecated and ignored since Dash Core 0.18.0*

*Parameter #5---a comment*

Name | Type | Presence | Description
--- | --- | --- | ---
Comment | string | Optional<br>(0 or 1) | A locally-stored (not broadcast) comment assigned to this transaction.  Default is no comment

*Parameter #6---automatic fee subtraction*

Name | Type | Presence | Description
--- | --- | --- | ---
Subtract Fee From Amount | array | Optional<br>(0 or 1) | An array of addresses.  The fee will be equally divided by as many addresses as are entries in this array and subtracted from each address.  If this array is empty or not provided, the fee will be paid by the sender
→<br>Address | string (base58) | Optional (0 or more) | An address previously listed as one of the recipients.

*Parameter #7---use InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
Use InstantSend | bool | Optional<br>(0 or 1) | *Deprecated and ignored since Dash Core 0.15.0*

*Parameter #8---use CoinJoin*

Name | Type | Presence | Description
--- | --- | --- | ---
Use CoinJoin | bool | Optional<br>(0 or 1) | If set to `true`, use CoinJoin funds only (default: false).

*Parameter #9---confirmation target*

Name | Type | Presence | Description
--- | --- | --- | ---
`conf_target` | number (int) | Optional<br>(0 or 1) | Confirmation target (in blocks)

*Parameter #10---fee estimate mode*

Name | Type | Presence | Description
--- | --- | --- | ---
`estimate_mode` | string | Optional<br>(0 or 1) |  The fee estimate mode, must be one of:<br>`UNSET`<br>`ECONOMICAL`<br>`CONSERVATIVE`<br>Default: `UNSET`

*Result---a TXID of the sent transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | The TXID of the sent transaction, encoded as hex in RPC byte order

*Example from Dash Core 0.17.0*

Send 0.1 DASH to the first address and 0.2 DASH to the second address, with a comment of "Example Transaction".

```bash
dash-cli -testnet sendmany \
  "" \
  '''
    {
      "ySutkc49Khpz1HQN8AfWNitVBLwqtyaxvv": 0.1,
      "yhQrX8CZTTfSjKmaq5h7DgSShyEsumCRBi": 0.2
    } ''' \
  6       \
  false   \
  "Example Transaction"
```

Result:

```text
a7c0194a005a220b9bfeb5fdd12d5b90979c10f53de4f8a48a1495aa198a6b95
```

*Example from Dash Core 0.12.2 (CoinJoin)*

Send 0.1 DASH to the first address and 0.2 DASH to the second address using CoinJoin, with a comment of "Example Transaction".

```bash
dash-cli -testnet sendmany \
  "" \
  '''
    {
      "ySutkc49Khpz1HQN8AfWNitVBLwqtyaxvv": 0.1,
      "yhQrX8CZTTfSjKmaq5h7DgSShyEsumCRBi": 0.2
    } ''' \
  6       \
  false   \
  "Example Transaction"
  '''
    [""]
  '''    \
  false  \
  true
```

Result:

```text
43337c8e4f3b21bedad7765fa851a6e855e4bb04f60d6b3e4c091ed21ffc5753
```

*See also*

* [SendFrom](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#sendfrom): spends an amount from a local account to a dash address.
* [SendToAddress](core-api-ref-remote-procedure-calls-wallet#sendtoaddress): spends an amount to a given address.
* [Move](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#move): moves a specified amount from one account in your wallet to another using an off-block-chain transaction.

# SendToAddress
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**). Requires an unlocked wallet or an unencrypted wallet."
}
[/block]

The [`sendtoaddress` RPC](core-api-ref-remote-procedure-calls-wallet#sendtoaddress) spends an amount to a given address.

*Parameter #1---to address*

Name | Type | Presence | Description
--- | --- | --- | ---
To Address | string | Required<br>(exactly 1) | A P2PKH or P2SH address to which the dash should be sent

*Parameter #2---amount to spend*

Name | Type | Presence | Description
--- | --- | --- | ---
Amount | number (dash) | Required<br>(exactly 1) | The amount to spent in dash

*Parameter #3---a comment*

Name | Type | Presence | Description
--- | --- | --- | ---
Comment | string | Optional<br>(0 or 1) | A locally-stored (not broadcast) comment assigned to this transaction.  Default is no comment

*Parameter #4---a comment about who the payment was sent to*

Name | Type | Presence | Description
--- | --- | --- | ---
Comment To | string | Optional<br>(0 or 1) | A locally-stored (not broadcast) comment assigned to this transaction.  Meant to be used for describing who the payment was sent to. Default is no comment

*Parameter #5---automatic fee subtraction*

Name | Type | Presence | Description
--- | --- | --- | ---
Subtract Fee From Amount | boolean | Optional<br>(0 or 1) | The fee will be deducted from the amount being sent. The recipient will receive less dash than you enter in the amount field. Default is `false`

*Parameter #6---use InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
Use InstantSend | bool | Optional<br>(0 or 1) | *Deprecated and ignored since Dash Core 0.15.0*

*Parameter #7---use CoinJoin*

Name | Type | Presence | Description
--- | --- | --- | ---
Use CoinJoin | bool | Optional<br>(0 or 1) | If set to `true`, use CoinJoin funds only (default: false).

*Parameter #8---confirmation target*

Name | Type | Presence | Description
--- | --- | --- | ---
`conf_target` | number (int) | Optional<br>(0 or 1) | Confirmation target (in blocks)

*Parameter #9---fee estimate mode*

Name | Type | Presence | Description
--- | --- | --- | ---
`estimate_mode` | string | Optional<br>(0 or 1) |  The fee estimate mode, must be one of:<br>`UNSET`<br>`ECONOMICAL`<br>`CONSERVATIVE`<br>Default: `UNSET`

*Result---a TXID of the sent transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | The TXID of the sent transaction, encoded as hex in RPC byte order

*Example from Dash Core 0.12.2*

Spend 0.1 dash to the address below with the comment "sendtoaddress
example" and the comment-to "Nemo From Example.com":

```bash
dash-cli -testnet sendtoaddress ySutkc49Khpz1HQN8AfWNitVBLwqtyaxvv \
  1.0 "sendtoaddress example" "Nemo From Example.com"
```

Result:

```text
70e2029d363f0110fe8a0aa2ba7bd771a579453135568b2aa559b2cb30f875aa
```

*Example from Dash Core 0.12.2 (InstantSend)*

Spend 0.1 dash via InstantSend to the address below with the comment "sendtoaddress
example" and the comment-to "Nemo From Example.com":

```bash
dash-cli -testnet sendtoaddress ySutkc49Khpz1HQN8AfWNitVBLwqtyaxvv \
  1.0 "sendtoaddress example" "Nemo From Example.com" false true
```

Result:

```text
af002b9c931b5efb5b2852df3d65efd48c3b9ac2ba0ef8a4cf97b894f3ff08c2
```

*Example from Dash Core 0.12.2 (CoinJoin)*

Spend 0.1 dash via CoinJoin to the address below with the comment "sendtoaddress
example" and the comment-to "Nemo From Example.com":

```bash
dash-cli -testnet sendtoaddress ySutkc49Khpz1HQN8AfWNitVBLwqtyaxvv \
  1.0 "sendtoaddress example" "Nemo From Example.com" false false true
```

Result:

```text
949833bc49e0643f63e2afed1704ccccf005a93067a4e46165b06ace42544694
```

*Example from Dash Core 0.12.2 (InstantSend + CoinJoin)*

Spend 0.1 dash via InstantSend and CoinJoin to the address below with the
comment "sendtoaddressexample" and the comment-to "Nemo From Example.com":

```bash
dash-cli -testnet sendtoaddress ySutkc49Khpz1HQN8AfWNitVBLwqtyaxvv \
  1.008 "sendtoaddress example" "Nemo From Example.com" false true true
```

Result:

```text
ba4bbe29fa06b67d6f3f3a73e381627e66abe22e217ce329aefad41ea72c3922
```

*See also*

* [SendFrom](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#sendfrom): spends an amount from a local account to a dash address.
* [SendMany](core-api-ref-remote-procedure-calls-wallet#sendmany): creates and broadcasts a transaction which sends outputs to multiple addresses.
* [Move](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#move): moves a specified amount from one account in your wallet to another using an off-block-chain transaction.

# SetLabel
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]
The [`setlabel` RPC](core-api-ref-remote-procedure-calls-wallet#setlabel) sets the label associated with the given address.

*Parameter #1---a Dash address*

Name | Type | Presence | Description
--- | --- | --- | ---
Address | string (base58) | Required<br>(exactly 1) | The P2PKH or P2SH Dash address to be associated with a label.

*Parameter #2---a label*

Name | Type | Presence | Description
--- | --- | --- | ---
Label | string | Required<br>(exactly 1) | The label to assign to the address.

*Result---`null` if successful*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | Set to JSON `null` if the address was successfully placed in the account

*Example from Dash Core 0.17.0*

Assign the "doc test" label to the provided address.

``` bash
dash-cli -testnet setlabel yMTFRnrfJ4NpnYVeidDNHVwT7uuNsVjevq "doc test"
```

(Success: no result displayed.)

*See also*

* [ListLabels](core-api-ref-remote-procedure-calls-wallet#listlabels): returns the list of all labels, or labels that are assigned to addresses with a specific purpose.
* [GetAddressesByLabel](core-api-ref-remote-procedure-calls-wallet#getaddressesbylabel): returns the list of addresses assigned the specified label.

<span id="setprivatesendamount"></span>
# SetCoinJoinAmount

The [`setcoinjoinamount` RPC](core-api-ref-remote-procedure-calls-wallet#setcoinjoinamount) sets the amount of DASH to be processed with CoinJoin (previously named `setprivatesendamount` prior to Dash Core 0.17.0)

*Parameter #1---amount to process*

Name | Type | Presence | Description
--- | --- | --- | ---
Amount | int | Required<br>(exactly 1) | The number of DASH to process (minimum: 2, maximum: 21,000,000)

*Result---`null` on success*

*Example from Dash Core 0.13.0*

```bash
dash-cli -testnet setcoinjoinamount 2000
```

(Success: no result displayed.)

*See also:*

* [SetCoinJoinRounds](core-api-ref-remote-procedure-calls-wallet#setcoinjoinrounds): sets the number of rounds to use

<span id="setprivatesendrounds"></span>
# SetCoinJoinRounds

The [`setcoinjoinrounds` RPC](core-api-ref-remote-procedure-calls-wallet#setcoinjoinrounds) sets the number of rounds to use (previously named `setprivatesendrounds` prior to Dash Core 0.17.0)

*Parameter #1---number of rounds to use*

Name | Type | Presence | Description
--- | --- | --- | ---
Rounds | int | Required<br>(exactly 1) | The number of rounds to use (minimum: 1, maximum: 16)

*Result---`null` on success*

*Example from Dash Core 0.13.0*

```bash
dash-cli -testnet setcoinjoinrounds 4
```

(Success: no result displayed.)

*See also:*

* [SetCoinJoinAmount](core-api-ref-remote-procedure-calls-wallet#setcoinjoinamount): sets the amount of DASH to be processed with CoinJoin

# SetTxFee
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]

The [`settxfee` RPC](core-api-ref-remote-procedure-calls-wallet#settxfee) sets the transaction fee per kilobyte paid by transactions created by this wallet.

*Parameter #1---the transaction fee amount per kilobyte*

Name | Type | Presence | Description
--- | --- | --- | ---
Transaction Fee Per Kilobyte | number (dash) | Required<br>(exactly 1) | The transaction fee to pay, in dash, for each kilobyte of transaction data.  Be careful setting the fee too low---your transactions may not be relayed or included in blocks

*Result: `true` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | bool (true) | Required<br>(exactly 1) | Set to `true` if the fee was successfully set

*Example from Dash Core 0.12.2*

Set the transaction fee per kilobyte to 10,000 duffs.

```bash
dash-cli -testnet settxfee 0.00010000
```

Result:

```json
true
```

*See also*

* [GetWalletInfo](core-api-ref-remote-procedure-calls-wallet#getwalletinfo): provides information about the wallet.
* [GetNetworkInfo](/docs/core-api-ref-remote-procedure-calls-network#getnetworkinfo): returns information about the node's connection to the network.

# SignMessage
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**). Requires an unlocked wallet or an unencrypted wallet."
}
[/block]

The [`signmessage` RPC](core-api-ref-remote-procedure-calls-wallet#signmessage) signs a message with the private key of an address.

*Parameter #1---the address corresponding to the private key to sign with*

Name | Type | Presence | Description
--- | --- | --- | ---
Address | string (base58) | Required<br>(exactly 1) | A P2PKH address whose private key belongs to this wallet

*Parameter #2---the message to sign*

Name | Type | Presence | Description
--- | --- | --- | ---
Message | string | Required<br>(exactly 1) | The message to sign

*Result---the message signature*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (base64) | Required<br>(exactly 1) | The signature of the message, encoded in base64.

*Example from Dash Core 0.12.2*

Sign a the message "Hello, World!" using the following address:

```bash
dash-cli -testnet signmessage yNpezfFDfoikDuT1f4iK75AiLp2YLPsGAb "Hello, World!"
```

Result:

```text
H4XULzfHCf16In2ECk9Ta9QxQPq639zQto2JA3OLlo3JbUdrClvJ89+A1z+Z9POd6l8LJhn1jGpQYF8mX4jkQvE=
```

*See also*

* [VerifyMessage](/docs/core-api-ref-remote-procedure-calls-util#verifymessage): verifies a signed message.

# SignRawTransactionWithWallet
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**). Requires an unlocked wallet."
}
[/block]
The [`signrawtransactionwithwallet` RPC](#signrawtransactionwithwallet) signs a transaction in the serialized transaction format using private keys stored in the wallet.

*Parameter #1---the transaction to sign*

Name | Type | Presence | Description
--- | --- | --- | ---
Transaction | string (hex) | Required<br>(exactly 1) | The transaction to sign as a serialized transaction

*Parameter #2---unspent transaction output details*

Name | Type | Presence | Description
--- | --- | --- | ---
Dependencies | array | Optional<br>(0 or 1) | The previous outputs being spent by this transaction
→<br>Output | object | Optional<br>(0 or more) | An output being spent
→ →<br>`txid` | string (hex) | Required<br>(exactly 1) | The TXID of the transaction the output appeared in.  The TXID must be encoded in hex in RPC byte order
→ →<br>`vout` | number (int) | Required<br>(exactly 1) | The index number of the output (vout) as it appeared in its transaction, with the first output being 0
→ →<br>`scriptPubKey` | string (hex) | Required<br>(exactly 1) | The output's pubkey script encoded as hex
→ →<br>`redeemScript` | string (hex) | Optional<br>(0 or 1) | If the pubkey script was a script hash, this must be the corresponding redeem script
→ →<br>`amount` | numeric | Required<br>(exactly 1) | The amount of Dash spent

*Parameter #3---signature hash type*

Name | Type | Presence | Description
--- | --- | --- | ---
SigHash | string | Optional<br>(0 or 1) | The type of signature hash to use for all of the signatures performed.  (You must use separate calls to the [`signrawtransactionwithwallet` RPC](#signrawtransactionwithwallet) if you want to use different signature hash types for different signatures.  The allowed values are: `ALL`, `NONE`, `SINGLE`, `ALL|ANYONECANPAY`, `NONE|ANYONECANPAY`, and `SINGLE|ANYONECANPAY`

*Result---the transaction with any signatures made*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | The results of the signature
→<br>`hex` | string (hex) | Required<br>(exactly 1) | The resulting serialized transaction encoded as hex with any signatures made inserted.  If no signatures were made, this will be the same transaction provided in parameter #1
→<br>`complete` | bool | Required<br>(exactly 1) | The value `true` if transaction is fully signed; the value `false` if more signatures are required

*Example from Dash Core 0.17.0*

Sign the hex generated from the `createrawtransaction` RPC:

``` bash
dash-cli -testnet signrawtransactionwithwallet 020000000121f39228a11ddf19\
7ac3658e93bd264d0afd927f0cdfc7caeb760537e529c94a0100000000ffffffff0180969\
800000000001976a914fe64a96d6660e30c433e1189082466a95bdf9ceb88ac00000000
```

Result:

``` json
{
  "hex": "020000000121f39228a11ddf197ac3658e93bd264d0afd927f0cdfc7caeb760537e529c94a010000006b483045022100811c5679ef097b0e5a338fc3cd05ee50e1802680ea8a172d0fd3a81da3c1fc2002204804b18a44e888ac1ee9b6a7cbadc211ecdc30f8c889938c95125206e39554220121025d81ce6581e547dd34194385352053abb17f0246768d75443b25ded5e37d594fffffffff0180969800000000001976a914fe64a96d6660e30c433e1189082466a95bdf9ceb88ac00000000",
  "complete": true
}
```

*See also*

* [CombineRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transactions#combinerawtransaction): combine multiple partially signed transactions into one transaction.
* [CreateRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transactions#createrawtransaction): creates an unsigned serialized transaction that spends a previous output to a new output with a P2PKH or P2SH address. The transaction is not stored in the wallet or transmitted to the network.
* [DecodeRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transactions#decoderawtransaction): decodes a serialized transaction hex string into a JSON object describing the transaction.
* [SendRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transactions#sendrawtransaction): validates a transaction and broadcasts it to the peer-to-peer network.
* [SignRawTransactionWithKey](/docs/core-api-ref-remote-procedure-calls-raw-transactions#signrawtransactionwithkey): signs inputs for a transaction in the serialized transaction format using private keys provided in the call.

# UnloadWallet
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)."
}
[/block]
The [`unloadwallet` RPC](core-api-ref-remote-procedure-calls-wallet#unloadwallet) unloads the wallet referenced by the request endpoint otherwise unloads the wallet specified in the argument. Specifying the wallet name on a wallet endpoint is invalid.

*Parameter #1---wallet name*

Name | Type | Presence | Description
--- | --- | --- | ---
Filename | string | Required<br>(exactly 1) | The name of the wallet to unload.

*Result---null on success*

*Example from Dash Core 0.17.0*

```bash
dash-cli -testnet unloadwallet wallet-test.dat
```

Result:

```shell
null
```

*See also*

* [LoadWallet](core-api-ref-remote-procedure-calls-wallet#loadwallet): loads a wallet from a wallet file or directory.

# UpgradeToHD

[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]

The [`upgrradetohd` RPC](core-api-ref-remote-procedure-calls-wallet#upgradetohd) upgrades non-HD wallets to HD.

*Parameters*

Name | Type | Presence | Description
--- | --- | --- | ---
Mnemonic | string | Optional<br>(0 or 1) | Mnemonic as defined in BIP39 to use for the new HD wallet. Use an empty string `""` to generate a new random mnemonic.
Mnemonic Passphrase | string | Optional<br>(0 or 1) | Optional mnemonic passphrase as defined in [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki#From_mnemonic_to_seed)
Wallet Passphrase | string | Optional<br>(0 or 1) | If your wallet is encrypted you must have your wallet passphrase here. If your wallet is not encrypted, specifying wallet passphrase will trigger wallet encryption.

*Result---`true` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | `true` when the command was successful or error message if not successful.

*Example from Dash Core 0.17.0*

Upgrade wallet to HD without specifying any optional parameters:

```bash
dash-cli -testnet upgradetohd
```

```bash
true
```

*See also*

* [DumpHDInfo](core-api-ref-remote-procedure-calls-wallet#dumphdinfo):  returns an object containing sensitive private info about this HD wallet

# WalletCreateFundedPSBT

The [`walletcreatefundedpsbt` RPC](core-api-ref-remote-procedure-calls-wallet#walletcreatefundedpsbt) creates and funds a transaction in the Partially Signed Transaction (PST) format. Inputs will be added if supplied inputs are not enough.

Implements the Creator and Updater roles.

*Parameter #1---Inputs*

Name | Type | Presence | Description
--- | --- | --- | ---
Inputs | array | Required<br>(exactly 1) | An array of objects, each one to be used as an input to the transaction
→ Input | object | Required<br>(1 or more) | An object describing a particular input
→ →<br>`txid` | string (hex) | Required<br>(exactly 1) | The TXID of the outpoint to be spent encoded as hex in RPC byte order
→ →<br>`vout` | number (int) | Required<br>(exactly 1) | The output index number (vout) of the outpoint to be spent; the first output in a transaction is index `0`
→ →<br>`Sequence` | number (int) | Optional<br>(0 or 1) | The sequence number to use for the input

*Parameter #2---Outputs*

Name | Type | Presence | Description
--- | --- | --- | ---
Outputs | array | Required<br>(exactly 1) | A JSON array with outputs as key-value pairs
→ Output | object | Required<br>(1 or more) | An object describing a particular output
→ →<br>Address | string: number (Dash) | Optional<br>(0 or 1) | A key-value pair. The key (string) is the Dash address, the value (float or string) is the amount in DASH
→ →<br>Data | `data`: string (hex) | Optional<br>(0 or 1) | A key-value pair. The key must be `data`, the value is hex encoded data

*Parameter #3---locktime*

Name | Type | Presence | Description
--- | --- | --- | ---
Locktime | numeric (int) | Optional<br>(0 or 1) | Indicates the earliest time a transaction can be added to the block chain (default=`0`). Non-0 value also locktime-activates inputs.

*Parameter #4---Additional options*

Name | Type | Presence | Description
--- | --- | --- | ---
Options | Object | Optional<br>(0 or 1) | Additional options
→ <br>`changeAddress` | string (hex) | Optional<br>(0 or 1) | The dash address to receive the change (default=pool address)
→ <br>`changePosition` | numeric (int) | Optional<br>(0 or 1) | The index of the change output (default=random)
→ <br>`includeWatching` | bool | Optional<br>(0 or 1) | Also select inputs which are watch only (default=`false`)
→ <br>`lockUnspents` | bool | Optional<br>(0 or 1) | Lock selected unspent outputs (default=`false`)
→ <br>`feeRate` | numeric or string | Optional<br>(0 or 1) | Set a specific fee rate in DASH/kB
→ <br>`subtractFeeFromOutputs` | array | Optional<br>(0 or 1) | A json array of integers. The fee will be equally deducted from the amount of each specified output. The outputs are specified by their zero-based index, before any change output is added. Those recipients will receive less Dash than you enter in their corresponding amount field. If no outputs are specified here, the sender pays the fee.
→ →<br>Output index | numeric (int) | Optional<br>(0 or more) | An output index number (vout) from which the fee should be subtracted. If multiple vouts are provided, the total fee will be divided by the number of vouts listed and each vout will have that amount subtracted from it.
→ <br>`conf_target` | numeric (int) | Optional<br>(0 or 1) | Confirmation target (in blocks)
→ <br>`estimate_mode` | numeric (int) | Optional<br>(0 or 1) | The fee estimate mode, must be one of:<br>`UNSET` (default),<br>`ECONOMICAL`,<br>`CONSERVATIVE`

*Parameter #5---bip32derivs*

Name | Type | Presence | Description
--- | --- | --- | ---
`bip32derivs` | bool | Optional<br>(exactly 0 or 1) | If true, includes the BIP32 derivation paths for public keys if we know them

*Result---information about the created transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object including information about the created transaction
→ <br>`psbt` | string (base64) | Required<br>(Exactly 1) | The resulting raw transaction (base64-encoded string)
→ <br>`fee` | numeric (bitcoins) | Required<br>(Exactly 1) | Fee in DASH the resulting transaction pays
→ <br>`changepos` | numeric (int) | Required<br>(Exactly 1) | The position of the added change output, or `-1` if no change output was added

*Example from Dash Core 0.18.0*

```bash
dash-cli -testnet walletcreatefundedpsbt "[{\"txid\":\"2662c87e1761ed5f4e98a0640b2608114d86f282824a51bd624985d236c71178\",\"vout\":0}]" "[{\"data\":\"00010203\"}]"
```

Result:
```json
{
  "psbt": "cHNidP8BAGQCAAAAAXgRxzbShUlivVFKgoLyhk0RCCYLZKCYTl/tYRd+yGImAAAAAAD/////AgAAAAAAAAAABmoEAAECA6PmxgkAAAAAGXapFFNPqpebN9gMkzsFJWixaDCZ3S8OiKwAAAAAAAEA4QIAAAABlIu3UCzRFVGowPY3H7RvS5g6QGc71ZYEFXIrcS+NfEIBAAAAakcwRAIgT+T8SIyVXyhsUshI7HlQtA7EduG0NMat1oa0dL3eCakCIGIi0pH9naNBQIqopHIPWmlZmXcVod34GH51J3tr/K5+ASEDxn2GlEMVg4rqfsgNOQtdCbkbYkgzcNSXnaXM96ffd6n+////ArTnxgkAAAAAGXapFM78RkkEwDgUwBkG4ZfcdZp0XkfuiKwAypo7AAAAABl2qRQ++by+kvd8j63QVm7qf/jUfyK94IisVUgIAAAAAA==",
  "fee": 0.00000273,
  "changepos": 1
}
```

# WalletLock
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**). Requires an unlocked wallet."
}
[/block]

The [`walletlock` RPC](core-api-ref-remote-procedure-calls-wallet#walletlock) removes the wallet encryption key from memory, locking the wallet. After calling this method, you will need to call `walletpassphrase` again before being able to call any methods which require the wallet to be unlocked.

*Parameters: none*

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | Always set to JSON `null`

*Example from Dash Core 0.12.2*

```bash
dash-cli -testnet walletlock
```

(Success: nothing printed.)

*See also*

* [EncryptWallet](core-api-ref-remote-procedure-calls-wallet#encryptwallet): encrypts the wallet with a passphrase.  This is only to enable encryption for the first time. After encryption is enabled, you will need to enter the passphrase to use private keys.
* [WalletPassphrase](core-api-ref-remote-procedure-calls-wallet#walletpassphrase): stores the wallet decryption key in memory for the indicated number of seconds. Issuing the `walletpassphrase` command while the wallet is already unlocked will set a new unlock time that overrides the old one.
* [WalletPassphraseChange](core-api-ref-remote-procedure-calls-wallet#walletpassphrasechange): changes the wallet passphrase from 'old passphrase' to 'new passphrase'.

# WalletPassphrase
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**). Requires an encrypted wallet."
}
[/block]

The [`walletpassphrase` RPC](core-api-ref-remote-procedure-calls-wallet#walletpassphrase) stores the wallet decryption key in memory for the indicated number of seconds. Issuing the `walletpassphrase` command while the wallet is already unlocked will set a new unlock time that overrides the old one.

[block:callout]
{
  "type": "warning",
  "body": "**Warning:** if using this RPC on the command line, remember that your shell probably saves your command lines (including the value of the passphrase parameter)."
}
[/block]

*Parameter #1---the passphrase*

Name | Type | Presence | Description
--- | --- | --- | ---
Passphrase | string | Required<br>(exactly 1) | The passphrase that unlocks the wallet

*Parameter #2---the number of seconds to leave the wallet unlocked*

Name | Type | Presence | Description
--- | --- | --- | ---
Seconds | number (int) | Required<br>(exactly 1) | The number of seconds after which the decryption key will be automatically deleted from memory

*Parameter #3---unlock for mixing processing only*

Name | Type | Presence | Description
--- | --- | --- | ---
Mixing Only | bool | Optional<br>(0 or 1) | If `true`, the wallet will be locked for sending functions but unlocked for mixing transactions (default: false)

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | Always set to JSON `null`

*Example from Dash Core 0.12.2*

Unlock the wallet for 10 minutes (the passphrase is "test"):

```bash
dash-cli -testnet walletpassphrase test 600
```

(Success: no result printed.)

Unlock the wallet for CoinJoin processing transactions only for 10 minutes (the passphrase is "test"):

```bash
dash-cli -testnet walletpassphrase test 600 true
```

(Success: no result printed.)

*See also*

* [EncryptWallet](core-api-ref-remote-procedure-calls-wallet#encryptwallet): encrypts the wallet with a passphrase.  This is only to enable encryption for the first time. After encryption is enabled, you will need to enter the passphrase to use private keys.
* [WalletPassphraseChange](core-api-ref-remote-procedure-calls-wallet#walletpassphrasechange): changes the wallet passphrase from 'old passphrase' to 'new passphrase'.
* [WalletLock](core-api-ref-remote-procedure-calls-wallet#walletlock): removes the wallet encryption key from memory, locking the wallet. After calling this method, you will need to call `walletpassphrase` again before being able to call any methods which require the wallet to be unlocked.

# WalletPassphraseChange
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**). Requires an encrypted wallet."
}
[/block]

The [`walletpassphrasechange` RPC](core-api-ref-remote-procedure-calls-wallet#walletpassphrasechange) changes the wallet passphrase from 'old passphrase' to 'new passphrase'.

[block:callout]
{
  "type": "warning",
  "body": "**Warning:** if using this RPC on the command line, remember that your shell probably saves your command lines (including the value of the passphrase parameter)."
}
[/block]

*Parameter #1---the current passphrase*

Name | Type | Presence | Description
--- | --- | --- | ---
Current Passphrase | string | Required<br>(exactly 1) | The current wallet passphrase

*Parameter #2---the new passphrase*

Name | Type | Presence | Description
--- | --- | --- | ---
New Passphrase | string | Required<br>(exactly 1) | The new passphrase for the wallet

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | Always set to JSON `null`

*Example from Dash Core 0.12.2*

Change the wallet passphrase from "test" to "example":

```bash
dash-cli -testnet walletpassphrasechange "test" "example"
```

(Success: no result printed.)

*See also*

* [EncryptWallet](core-api-ref-remote-procedure-calls-wallet#encryptwallet): encrypts the wallet with a passphrase.  This is only to enable encryption for the first time. After encryption is enabled, you will need to enter the passphrase to use private keys.
* [WalletLock](core-api-ref-remote-procedure-calls-wallet#walletlock): removes the wallet encryption key from memory, locking the wallet. After calling this method, you will need to call `walletpassphrase` again before being able to call any methods which require the wallet to be unlocked.
* [WalletPassphrase](core-api-ref-remote-procedure-calls-wallet#walletpassphrase): stores the wallet decryption key in memory for the indicated number of seconds. Issuing the `walletpassphrase` command while the wallet is already unlocked will set a new unlock time that overrides the old one.

# WalletProcessPSBT

The [`walletprocesspsbt` RPC](core-api-ref-remote-procedure-calls-wallet#walletprocesspsbt) updates a PSBT with input information from a wallet and then allows the signing of inputs.

*Parameter #1---PSBT*

Name | Type | Presence | Description
--- | --- | --- | ---
`psbt` | string | Required<br>(exactly 1) | The transaction base64 string

*Parameter #2---Sign Transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`sign` | bool | Optional<br>(exactly 0 or 1) | Sign the transaction when updating (default = `true`)

*Parameter #3---Signature Hash Type*

Name | Type | Presence | Description
--- | --- | --- | ---
`sighashtype` | string | Optional<br>(exactly 0 or 1) | he signature hash type to sign with if not specified by the PSBT. Must be one of the following (default = ALL):<br> - ALL<br> - NONE<br> - SINGLE<br> - ALL\|ANYONECANPAY<br> - NONE\|ANYONECANPAY<br> - SINGLE\|ANYONECANPAY

*Parameter #4---bip32derivs*

Name | Type | Presence | Description
--- | --- | --- | ---
bip32 | bool | Optional<br>(exactly 0 or 1) | If true, includes the BIP 32 derivation paths for public keys if known (default = `false`).

*Result---the processed wallet*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | The results of the signature
→<br>`psbt` | string | Required<br>(exactly 1) | The base64-encoded partially signed transaction
→<br>`complete` | bool | Required<br>(exactly 1) | If the transaction has a complete set of signatures

*Example from Dash Core 0.18.0*

Change the wallet passphrase from "test" to "example":

```bash
dash-cli walletprocesspsbt "cHNidP8BAEICAAAAAXgRxzbShUlivVFKgoLyhk0RCCYLZKCYTl/tYRd+yGImAAAAAAD/////AQAAAAAAAAAABmoEAAECAwAAAAAAAAA="
```

Result:
```json
{
  "psbt": "cHNidP8BAEICAAAAAXgRxzbShUlivVFKgoLyhk0RCCYLZKCYTl/tYRd+yGImAAAAAAD/////AQAAAAAAAAAABmoEAAECAwAAAAAAAQDhAgAAAAGUi7dQLNEVUajA9jcftG9LmDpAZzvVlgQVcitxL418QgEAAABqRzBEAiBP5PxIjJVfKGxSyEjseVC0DsR24bQ0xq3WhrR0vd4JqQIgYiLSkf2do0FAiqikcg9aaVmZdxWh3fgYfnUne2v8rn4BIQPGfYaUQxWDiup+yA05C10JuRtiSDNw1Jedpcz3p993qf7///8CtOfGCQAAAAAZdqkUzvxGSQTAOBTAGQbhl9x1mnReR+6IrADKmjsAAAAAGXapFD75vL6S93yPrdBWbup/+NR/Ir3giKxVSAgAAQdqRzBEAiAF1fgBDg2M/WAeYTYzCkEiSSrDVzcYoe8wwrw/MbdgOQIgJzoYBQ9hAm6jqk2cLFitUd1/iL1ku8w9unadjNfsCdoBIQJn2pETmk8U2X6veADqnny5/6j8Iy7Oizij0SeJHm9x6AAA",
  "complete": true
}
```

*See also*

* [CreatePSBT](/docs/core-api-ref-remote-procedure-calls-raw-transactions#createpsbt): creates a transaction in the Partially Signed Transaction (PST) format.
* [CombinePSBT](/docs/core-api-ref-remote-procedure-calls-raw-transactions#combinepsbt): combine multiple partially-signed Dash transactions into one transaction.
* [ConvertToPSBT](/docs/core-api-ref-remote-procedure-calls-raw-transactions#converttopsbt): converts a network serialized transaction to a PSBT.
* [DecodePSBT](/docs/core-api-ref-remote-procedure-calls-raw-transactions#decodepsbt): returns a JSON object representing the serialized, base64-encoded partially signed Dash transaction.
* [FinalizePSBT](/docs/core-api-ref-remote-procedure-calls-raw-transactions#finalizepsbt): finalizes the inputs of a PSBT.
* [WalletCreateFundedPSBT](/docs/core-api-ref-remote-procedure-calls-wallet#walletcreatefundedpsbt): creates and funds a transaction in the Partially Signed Transaction (PST) format.