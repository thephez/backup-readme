[block:callout]
{
  "type": "danger",
  "body": "RPCs that require wallet support are **not available on masternodes** for security reasons. Such RPCs are designated with a \"_Requires wallet support_\" message.",
  "title": "Wallet Support"
}
[/block]

# GetAccount
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]

The [`getaccount` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#getaccount) returns the name of the account associated with the given address.
[block:callout]
{
  "type": "warning",
  "body": "**Warning:** `setaccount` will be removed in a later version of Dash Core.  Use the RPCs listed in the See Also subsection below instead."
}
[/block]

*Parameter #1---a Dash address*

Name | Type | Presence | Description
--- | --- | --- | ---
Address | string (base58) | Required<br>(exactly 1) | A P2PKH or P2SH Dash address belonging either to a specific account or the default account (\\")"

*Result---an account name*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | The name of an account, or an empty string (\\", the default account)"

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet getaccount yMTFRnrfJ4NpnYVeidDNHVwT7uuNsVjevq
```

Result:

``` text
doc test
```

*See also*

* [GetAddressesByAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getaddressesbyaccount): returns a list of every address assigned to a particular account.

# GetAccountAddress
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]

The [`getaccountaddress` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#getaccountaddress) returns the current Dash address for receiving payments to this account. If the account doesn't exist, it creates both the account and a new address for receiving payment.  Once a payment has been received to an address, future calls to this RPC for the same account will return a different address.
[block:callout]
{
  "type": "warning",
  "body": "**Warning:** `getaccountaddress` will be removed in a later version of Dash Core.  Use the RPCs listed in the See Also subsection below instead."
}
[/block]

*Parameter #1---an account name*

Name | Type | Presence | Description
--- | --- | --- | ---
Account | string | Required<br>(exactly 1) | The name of an account.  Use an empty string (\\") for the default account.  If the account doesn't exist, it will be created"

*Result---a Dash address*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (base58) | Required<br>(exactly 1) | An address, belonging to the account specified, which has not yet received any payments

*Example from Dash Core 0.12.2*

Get an address for the default account:

``` bash
dash-cli -testnet getaccountaddress ""
```

Result:

``` text
yNUQ6RzTpNj5GP5ebdRcusJ7K9JJKx6VvV
```

*See also*

* [GetNewAddress](/docs/core-api-ref-remote-procedure-calls-wallet#getnewaddress): returns a new Dash address for receiving payments. If an account is specified, payments received with the address will be credited to that account.
* [GetRawChangeAddress](/docs/core-api-ref-remote-procedure-calls-wallet#getrawchangeaddress): returns a new Dash address for receiving change. This is for use with raw transactions, not normal use.
* [GetAddressesByAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getaddressesbyaccount): returns a list of every address assigned to a particular account.

# GetAddressesByAccount
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]

The [`getaddressesbyaccount` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#getaddressesbyaccount) returns a list of every address assigned to a particular account.
[block:callout]
{
  "type": "warning",
  "body": "**Warning:** `getaddressesbyaccount` will be removed in a later version of Dash Core.  Use the RPCs listed in the See Also subsection below instead."
}
[/block]

*Parameter #1---the account name*

Name | Type | Presence | Description
--- | --- | --- | ---
Account | string | Required<br>(exactly 1) | The name of the account containing the addresses to get.  To get addresses from the default account, pass an empty string (\\")"

*Result---a list of addresses*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array containing all addresses belonging to the specified account.  If the account has no addresses, the array will be empty
Address | string (base58) | Optional<br>(1 or more) | A P2PKH or P2SH address belonging to the account

*Example from Dash Core 0.12.2*

Get the addresses assigned to the account "doc test":

``` bash
dash-cli -testnet getaddressesbyaccount "doc test"
```

Result:

``` json
[
  "yMTFRnrfJ4NpnYVeidDNHVwT7uuNsVjevq",
  "yhT2HS1SxvXkMVdAdf6RNtGPfuVFvwZi35"
]
```

*See also*

* [GetAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getaccount): returns the name of the account associated with the given address.
* [GetBalance](/docs/core-api-ref-remote-procedure-calls-wallet#getbalance): gets the balance in decimal dash across all accounts or for a particular account.

# GetReceivedByAccount
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]

The [`getreceivedbyaccount` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#getreceivedbyaccount) returns the total amount received by addresses in a particular account from transactions with the specified number of confirmations.  It does not count coinbase transactions.
[block:callout]
{
  "type": "warning",
  "body": "**Warning:** `getreceivedbyaccount` will be removed in a later version of Dash Core.  Use the RPCs listed in the See Also subsection below instead."
}
[/block]

*Parameter #1---the account name*

Name | Type | Presence | Description
--- | --- | --- | ---
Account | string | Required<br>(exactly 1) | The name of the account containing the addresses to get.  For the default account, use an empty string (\\")"

*Parameter #2---the minimum number of confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations an externally-generated transaction must have before it is counted towards the balance.  Transactions generated by this node are counted immediately.  Typically, externally-generated transactions are payments to this wallet and transactions generated by this node are payments to other wallets.  Use `0` to count unconfirmed transactions.  Default is `1`

*Parameter #3---whether to include transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(exactly 1) | Add the balance from InstantSend locked transactions (default=false)

*Result---the number of dash received*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (dash) | Required<br>(exactly 1) | The number of dash received by the account.  May be `0`

*Example from Dash Core 0.12.2*

Get the dash received by the "doc test" account with six or more
confirmations:

``` bash
dash-cli -testnet getreceivedbyaccount "doc test" 6
```

Result:

``` json
0.30000000
```

*See also*

* [GetReceivedByAddress](/docs/core-api-ref-remote-procedure-calls-wallet#getreceivedbyaddress): returns the total amount received by the specified address in transactions with the specified number of confirmations. It does not count coinbase transactions.
* [GetAddressesByAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getaddressesbyaccount): returns a list of every address assigned to a particular account.
* [ListAccounts](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#listaccounts): lists accounts and their balances.

# ListAccounts
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]

The [`listaccounts` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#listaccounts) lists accounts and their balances.
[block:callout]
{
  "type": "warning",
  "body": "**Warning:** `listaccounts` will be removed in a later version of Dash Core.  Use the RPCs listed in the See Also subsection below instead."
}
[/block]

*Parameter #1---the minimum number of confirmations a transaction must have*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations an externally-generated transaction must have before it is counted towards the balance.  Transactions generated by this node are counted immediately.  Typically, externally-generated transactions are payments to this wallet and transactions generated by this node are payments to other wallets.  Use `0` to count unconfirmed transactions.  Default is `1`

*Parameter #2--- whether to include transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(exactly 1) | Add the balance from InstantSend locked transactions

*Parameter #3---whether to include watch-only addresses in results*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Watch-Only | bool | Optional<br>(0 or 1) | If set to `true`, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.  If set to `false` (the default), treat watch-only addresses as if they didn't belong to this wallet

*Result---a list of accounts and their balances*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | A JSON array containing key/value pairs with account names and values.  Must include, at the very least, the default account (\\")"
→<br>Account : Balance | string : number (dash) | Required<br>(1 or more) | The name of an account as a string paired with the balance of the account as a number of dash.  The number of dash may be negative if the account has spent more dash than it received.  Accounts with zero balances and zero transactions will be displayed

*Example from Dash Core 0.13.0*

Display account balances with one confirmation and watch-only addresses
included. Add the balance of InstantSend locked transactions also.

``` bash
dash-cli -testnet listaccounts 1 true true
```

Result:

``` json
{
  "": -2941.30029732,
  "Watching": 8.50000000,
  "MN": 2000.25442744,
  "PS": 37.02970000,
  "Recv1": 3843.48167912,
}
```

*See also*

* [GetAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getaccount): returns the name of the account associated with the given address.
* [GetAddressesByAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getaddressesbyaccount): returns a list of every address assigned to a particular account.
* [ListReceivedByAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#listreceivedbyaccount): lists the total number of dash received by each account.

# ListReceivedByAccount
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]

The [`listreceivedbyaccount` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#listreceivedbyaccount) lists the total number of dash received by each account.
[block:callout]
{
  "type": "warning",
  "body": "**Warning:** `listreceivedbyaccount` will be removed in a later version of Dash Core.  Use the RPCs listed in the See Also subsection below instead."
}
[/block]

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

*Result---account names, balances, and minimum confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array containing objects each describing an account.  At the very least, the default account (\\") will be included"
→<br>Account | object | Required<br>(1 or more) | An object describing an account
→ →<br>`involvesWatchonly` | bool | Optional<br>(0 or 1) | Set to `true` if the balance of this account includes a watch-only address which has received a spendable payment (that is, a payment with at least the specified number of confirmations and which is not an immature coinbase).  Otherwise not returned
→ →<br>`account` | string | Required<br>(exactly 1) | The name of the account
→ →<br>`amount` | number (dash) | Required<br>(exactly 1) | The total amount received by this account in dash
→ →<br>`confirmations` | number (int) | Required<br>(exactly 1) | The number of confirmations received by the last transaction received by this account.  May be `0`
→ →<br>`label` | string | Optional<br>(0 or 1) | A comment for the address/transaction

*Example from Dash Core 0.13.0*

Get the balances for all non-empty accounts, including transactions
which have been confirmed at least six times and InstantSend locked transactions:

``` bash
dash-cli -testnet listreceivedbyaccount 6 true false true
```

Result (edited to only show the first two results):

``` json
[
    {
        "account" : "",
        "amount" : 0.19960000,
        "confirmations" : 53601
    },
    {
        "account" : "doc test",
        "amount" : 0.30000000,
        "confirmations" : 8991
    }
]
```

*See also*

* [ListReceivedByAddress](/docs/core-api-ref-remote-procedure-calls-wallet#listreceivedbyaddress): lists the total number of dash received by each address.
* [GetReceivedByAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getreceivedbyaccount): returns the total amount received by addresses in a particular account from transactions with the specified number of confirmations.  It does not count coinbase transactions.
* [GetReceivedByAddress](/docs/core-api-ref-remote-procedure-calls-wallet#getreceivedbyaddress): returns the total amount received by the specified address in transactions with the specified number of confirmations. It does not count coinbase transactions.

# Move
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]

The [`move` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#move) moves a specified amount from one account in your wallet to another using an off-block-chain transaction.
[block:callout]
{
  "type": "warning",
  "body": "**Warning:** `move` will be removed in a later version of Dash Core.  Use the RPCs listed in the See Also subsection below instead."
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "**Warning:** it's possible to move more funds than are in an account, giving the sending account a negative balance and giving the receiving account a balance that may exceed the number of dash in the wallet (or the number of dash in existence)."
}
[/block]

*Parameter #1---from account*

Name | Type | Presence | Description
--- | --- | --- | ---
From Account | string | Required<br>(exactly 1) | The name of the account to move the funds from

*Parameter #2---to account*

Name | Type | Presence | Description
--- | --- | --- | ---
To Account | string | Required<br>(exactly 1) | The name of the account to move the funds to

*Parameter #3---amount to move*

Name | Type | Presence | Description
--- | --- | --- | ---
Amount | number (dash) | Required<br>(exactly 1) | The amount of dash to move

*Parameter #4---an unused parameter*

Name | Type | Presence | Description
--- | --- | --- | ---
*Unused* | number (int) | Optional<br>(0 or 1) | This parameter is no longer used. If parameter #5 needs to be specified, this can be any integer

*Parameter #5---a comment*

Name | Type | Presence | Description
--- | --- | --- | ---
Comment | string | Optional<br>(0 or 1) | A comment to assign to this move payment

*Result---`true` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | bool | Required<br>(exactly 1) | Set to `true` if the move was successful

*Example from Dash Core 0.12.2*

Move 1 dash from "doc test" to "test1", giving the transaction the
comment "Example move":

``` bash
dash-cli -testnet move "doc test" "test1" 0.1 0 "Example move"
```

Result:

``` json
true
```

*See also*

* [ListAccounts](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#listaccounts): lists accounts and their balances.
* [SendFrom](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#sendfrom): spends an amount from a local account to a dash address.
* [SendToAddress](/docs/core-api-ref-remote-procedure-calls-wallet#sendtoaddress): spends an amount to a given address.

# SendFrom
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**). Requires an unlocked wallet or an unencrypted wallet."
}
[/block]

The [`sendfrom` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#sendfrom) spends an amount from a local account to a dash address.

[block:callout]
{
  "type": "warning",
  "body": "**Warning:** `sendfrom` will be removed in a later version of Dash Core.  Use the RPCs listed in the See Also subsection below instead."
}
[/block]

*Parameter #1---from account*

Name | Type | Presence | Description
--- | --- | --- | ---
From Account | string | Required<br>(exactly 1) | The name of the account from which the dash should be spent.  Use an empty string (\\") for the default account"

*Parameter #2---to address*

Name | Type | Presence | Description
--- | --- | --- | ---
To Address | string | Required<br>(exactly 1) | A P2PKH or P2SH address to which the dash should be sent

*Parameter #3---amount to spend*

Name | Type | Presence | Description
--- | --- | --- | ---
Amount | number (dash) | Required<br>(exactly 1) | The amount to spend in dash.  Dash Core will ensure the account has sufficient dash to pay this amount (but the transaction fee paid is not included in the calculation, so an account can spend a total of its balance plus the transaction fee)

*Parameter #4---minimum confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations an incoming transaction must have for its outputs to be credited to this account's balance. Outgoing transactions are always counted, as are move transactions made with the [`move` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#move). If an account doesn't have a balance high enough to pay for this transaction, the payment will be rejected.  Use `0` to spend unconfirmed incoming payments. Default is `1`
[block:callout]
{
  "type": "warning",
  "body": "**Warning:** if account1 receives an unconfirmed payment and transfers it to account2 with the [`move` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#move), account2 will be able to spend those dash even if this parameter is set to `1` or higher."
}
[/block]

*Parameter #5---whether to add the balance from transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(0 or 1) | If set to `true`, add the balance from InstantSend locked transactions. If set to `false` (the default), InstantSend locked transaction balances are not included.

*Parameter #6---a comment*

Name | Type | Presence | Description
--- | --- | --- | ---
Comment | string | Optional<br>(0 or 1) | A locally-stored (not broadcast) comment assigned to this transaction.  Default is no comment

*Parameter #7---a comment about who the payment was sent to*

Name | Type | Presence | Description
--- | --- | --- | ---
Comment To | string | Optional<br>(0 or 1) | A locally-stored (not broadcast) comment assigned to this transaction.  Meant to be used for describing who the payment was sent to. Default is no comment

*Result---a TXID of the sent transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | The TXID of the sent transaction, encoded as hex in RPC byte order

*Example from Dash Core 0.12.2*

Spend 0.1 dash from the account "test" to the address indicated below
using only UTXOs with at least six confirmations, giving the
transaction the comment "Example spend" and labeling the spender
"Example.com":

``` bash
dash-cli -testnet sendfrom "test" \
            yhJays6zGUFKq1KS5V5WLbyk3cwCXyGrKd \
            0.1 \
            6 \
            false \
            "Example spend" \
            "Example.com"
```

Result:

``` text
cd64b9d55c63bf247f2eca32f978e340622107b607a46c422dabcdc20c0571fe
```

*See also*

* [SendToAddress](/docs/core-api-ref-remote-procedure-calls-wallet#sendtoaddress): spends an amount to a given address.
* [SendMany](/docs/core-api-ref-remote-procedure-calls-wallet#sendmany): creates and broadcasts a transaction which sends outputs to multiple addresses.

# SetAccount
[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]

The [`setaccount` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#setaccount) puts the specified address in the given account.
[block:callout]
{
  "type": "warning",
  "body": "**Warning:** `setaccount` will be removed in a later version of Dash Core.  Use the RPCs listed in the See Also subsection below instead."
}
[/block]

*Parameter #1---a dash address*

Name | Type | Presence | Description
--- | --- | --- | ---
Address | string (base58) | Required<br>(exactly 1) | The P2PKH or P2SH address to put in the account.  Must already belong to the wallet

*Parameter #2---an account*

Name | Type | Presence | Description
--- | --- | --- | ---
Account | string | Required<br>(exactly 1) | The name of the account in which the address should be placed.  May be the default account, an empty string (\\")"

*Result---`null` if successful*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | Set to JSON `null` if the address was successfully placed in the account

*Example from Dash Core 0.12.2*

Put the address indicated below in the "doc test" account.

``` bash
dash-cli -testnet setaccount \
    yMTFRnrfJ4NpnYVeidDNHVwT7uuNsVjevq "doc test"
```

(Success: no result displayed.)

*See also*

* [GetAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getaccount): returns the name of the account associated with the given address.
* [ListAccounts](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#listaccounts): lists accounts and their balances.
* [GetAddressesByAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getaddressesbyaccount): returns a list of every address assigned to a particular account.