RPC Documentation
=================
AddMultiSigAddress
++++++++++++++++++
*Requires wallet support.*

The `addmultisigaddress` RPC |summary addMultiSigAddress|

.. |summary addMultiSigAddress| replace:: adds a P2SH multisig transparent address to the wallet. *Multisig is not supported for shielded addresses.*

*Parameter #1---the number of signatures required*

.. csv-table::
    :header: |n|, |t|, |p|, |d|

    "Required", "number (int)", "Required (exactly 1)", "The minimum (*m*) number of signatures required to spend this m-of-n multisig script"

*Parameter #2---the full public keys, or addresses for known public keys*

.. csv-table::
    :header: |n|, |t|, |p|, |d|

    "Keys or Addresses", "array", "Required (exactly 1)", "An array of strings with each string being a public key or address"
    "→<br>Key Or Address", "string", "Required (1 or more)", "A public key against which signatures will be checked.  Alternatively, this may be a P2PKH address belonging to the wallet---the corresponding public key will be substituted.  There must be at least as many keys as specified by the Required parameter, and there may be more keys"

*Parameter #3---the account name*

*DEPRECATED. If provided, MUST be set to the empty string "" to represent the default account. Passing any other string will result in an error.*

.. csv-table::
    :header: |n|, |t|, |p|, |d|

    "Account", "string", "Optional (0 or 1)", "Accounts are deprecated and if provided MUST be set to the empty string \"\" to represent the default account."

*Result---a P2SH address printed and stored in the wallet*

.. csv-table::
    :header: |n|, |t|, |p|, |d|

    "`result`", "string (base58)", "Required (exactly 1)", "The P2SH multisig address.  The address will also be added to the wallet, and outputs paying that address will be tracked by the wallet"

*Example*

Add a multisig address from 2 of 3 addresses: ::

  zcash-cli addmultisigaddress 2 "[\"t14sSauSf5pF2UkUwvKGq4qjNRzBZYqgEL5\",\"t1G1sgjn4YtPu27adkKGrdDwzRTxnRkBfKV\",\"t1PoHp2v54vfmdgQ3v3SNuQga8JKHTNi2a1\"]"

Result: ::

  t3yVxxgNBk5zHRPRY2iVjGRJHYZEp1pMCSq

(New P2SH multisig address also stored in wallet.)

*See also*

- `CreateMultiSig`_: |summary_createMultiSig|
- `DecodeScript`_: |summary_decodeScript|
- `Pay-To-Script-Hash (P2SH)`_ </en/glossary/p2sh-address>

AddNode
+++++++
The `addnode` RPC |summary_addNode|

.. |summary_addNode| replace:: attempts to add or remove a node from the addnode list, or to try a connection to a node once.


*Parameter #1---hostname/IP address and port of node to add or remove*

+------+--------+-------------+--------------------------------------------------------------------------------------+
| |n|  | |t|    | |p|         | |d|                                                                                  |
+======+========+=============+======================================================================================+
| Node | string | Required    | The node to add as a string in the form of `<IP address>:<port>`. The IP address may |
|      |        | (exactly 1) | be a hostname resolvable through DNS, an IPv4 address, an IPv4-as-IPv6 address, or   |
|      |        |             | IPv6 address                                                                         |
+------+--------+-------------+--------------------------------------------------------------------------------------+

*Parameter #2---whether to add or remove the node, or to try only once to connect*

+---------+--------+-------------+-----------------------------------------------------------------------------------+
| |n|     | |t|    | |p|         | |d|                                                                               |
+=========+========+=============+===================================================================================+
| Command | string | Required    | What to do with the IP address above.  Options are:                               |
|         |        | (exactly 1) |                                                                                   |
|         |        |             | - `add` to add a node to the addnode list.  This will not connect immediately if  |
|         |        |             |   the outgoing connection slots are full                                          |
|         |        |             |                                                                                   |
|         |        |             | - `remove` to remove a node from the list.  If currently connected, this will     |
|         |        |             |   disconnect immediately                                                          |
|         |        |             |                                                                                   |
|         |        |             | - `onetry` to immediately attempt connection to the node even if the outgoing     |
|         |        |             |   connection slots are full; this will only attempt the connection once           |
+---------+--------+-------------+-----------------------------------------------------------------------------------+

*Result---`null` plus error on failed remove*

+----------+--------+-------------+----------------------------------------------------------------------------------+
| |n|      | |t|    | |p|         | |d|                                                                              |
+==========+========+=============+==================================================================================+
| `result` | null   | Required    | Always JSON `null` whether the node was added, removed, tried-and-connected, or  |
|          |        | (exactly 1) | tried-and-not-connected.  The JSON-RPC error field will be set only if you try   |
|          |        |             | removing a node that is not on the addnodes list                                 |
+----------+--------+-------------+----------------------------------------------------------------------------------+

*Example*

Connect to node `192.168.0.6`, trying once: ::

  zcash-cli addnode "192.168.0.6:8233" "onetry"

Result (no output from `zcash-cli` because result is set to `null`).

*See also*

- `GetAddedNodeInfo`_: |summary_getAddedNodeInfo|

BackupWallet
++++++++++++
*Requires wallet support.*

The `backupwallet` RPC |summary_backupWallet|

.. |summary_backupWallet| replace:: safely copies `wallet.dat` to the specified alphanumeric filename only after setting ``-exportdir=``.

*Parameter #1---destination filename*

+----------+----------------+-------------+--------------------------------------------------------------------------+
| |n|      | |t|            | |p|         | |d|                                                                      |
+==========+================+=============+==========================================================================+
| Filename | string         | Required    | A filename to be created or overwritten within the directory specified   |
|          | (alphanumeric) | (exactly 1) | in ``-exportdir=``.                                                      |
+----------+----------------+-------------+--------------------------------------------------------------------------+

*Result---the full path of the destination file*

+----------+----------------------+-------------+--------------------------------------------------------------------+
| |n|      | |t|                  | |p|         | |d|                                                                |
+==========+======================+=============+====================================================================+
| `result` | string (full path    | Required    | Returns full path of destination file or error message. The        |
|          | of destination file) | (exactly 1) | JSON-RPC error and message fields will be set if a failure         |
|          |                      |             | occurred                                                           |
+----------+----------------------+-------------+--------------------------------------------------------------------+

*Example*

Backup wallet after setting ``-exportdir=/home/user/zcash-backup/``: ::

  zcash-cli backupwallet backup

Result: ::

  /home/user/zcash-backup/backup

*See also*

- `DumpWallet`_: |summary_dumpWallet|
- `ImportWallet`_: |summary_importWallet|

ClearBanned
+++++++++++
The `clearbanned` RPC |summary_clearBanned|

.. |summary_clearBanned| replace:: clears list of banned nodes.

*Parameters: none*

*Result---`null` on success*

.. csv-table::
    :header: |n|, |t|, |p|, |d|

    "`result`", "null", "Required(exactly 1)", "JSON `null` when the list was cleared"

*Example*

Clears the ban list. ::

    zcash-cli clearbanned 

Result (no output from `zcash-cli` because result is set to `null`).

*See also*

-  `ListBanned`_: |summary_listBanned|
-  `SetBan`_: |summary_setBan|

  
CreateMultiSig
++++++++++++++
The `createmultisig` RPC |summary_createMultiSig|

.. |summary_createMultiSig| replace:: creates a P2SH multi-signature transparent address. *Multisig is not supported for shielded addresses.*

*Parameter #1---the number of signatures required*

+----------+--------------+-------------+----------------------------------------------------------------------------+
| |n|      | |t|          | |p|         | |d|                                                                        |
+==========+==============+=============+============================================================================+
| Required | number (int) | Required    | The minimum (*m*) number of signatures required to spend this m-of-n       |
|          |              | (exactly 1) | multisig script                                                            |
+----------+--------------+-------------+----------------------------------------------------------------------------+

*Parameter #2---the full public keys of transparent addresses, or transparent addresses for known public keys*

+-------------------+--------+-------------+-------------------------------------------------------------------------+
| |n|               | |t|    | |p|         | |d|                                                                     |
+===================+========+=============+=========================================================================+
| Keys Or Addresses | array  | Required    | An array of strings with each string being a transparent public key or  |
|                   |        | (exactly 1) | address                                                                 |
+-------------------+--------+-------------+-------------------------------------------------------------------------+
| Key Or Address    | string | Required    | A transparent public key against which signatures will be checked.      |
|                   |        | (1 or more) | Alternatively, this may be a P2PKH address belonging to the wallet---   |
|                   |        |             | the corresponding public key will be substituted.  There must be at     |
|                   |        |             | least as many keys as specified by the Required parameter, and there    |
|                   |        |             | may be more keys                                                        |
+-------------------+--------+-------------+-------------------------------------------------------------------------+

*Result---P2SH address and hex-encoded redeem script*

+----------------+-----------------+-------------+-------------------------------------------------------------------+
| |n|            | |t|             | |p|         | |d|                                                               |
+================+=================+=============+===================================================================+
| `result`       | object          | Required    | An object describing the multisig address                         |
|                |                 | (exactly 1) |                                                                   |
+----------------+-----------------+-------------+-------------------------------------------------------------------+
| `address`      | string (base58) | Required    | The P2SH address for this multisig redeem script                  |
|                |                 | (exactly 1) |                                                                   |
+----------------+-----------------+-------------+-------------------------------------------------------------------+
| `redeemScript` | string (hex)    | Required    | The multisig redeem script encoded as hex                         |
|                |                 | (exactly 1) |                                                                   |
+----------------+-----------------+-------------+-------------------------------------------------------------------+

*Example*

Creating a 2-of-3 P2SH multisig address by mixing two P2PKH addresses and one full public key (all transparent): ::

  zcash-cli createmultisig 2 '''
  [
    "t14sSauSf5pF2UkUwvKGq4qjNRzBZYqgEL5",
    "02f74d95ca23bba0180c1a38e972a3d3ef0f3045a47007f7ac39cf1c272bd4f770",
    "t1PoHp2v54vfmdgQ3v3SNuQga8JKHTNi2a1"
  ]
  '''

Result: ::

  {
    "address" : "t3yVxxgNBk5zHRPRY2iVjGRJHYZEp1pMCSq",
    "redeemScript" : "5221039cd63fb8cb183868b7cf71e0189e58786fcbf8dce452c1cf2ceb68ebf66d5e9b210231d344af857c98139f16427d037a5a52faca5a47e2900fa84ad15babbd5aaf01210395afaa4e56b887721634150f656c081896358a26fe3d46e4a0cee5d965152d7453ae"
  }

*See also*

- `AddMultiSigAddress`_: |summary_addMultiSigAddress|
- `DecodeScript`_: |summary_decodeScript|
- `[Pay-To-Script-Hash (P2SH)`_ </en/glossary/p2sh-address>

CreateRawTransaction
++++++++++++++++++++
The `createrawtransaction` RPC |summary_createRawTransaction|

.. |summary_createRawTransaction| replace:: creates an unsigned serialized transaction that spends a previous output to a new output with a P2PKH or P2SH address. The transaction is not stored in the wallet or transmitted to the network.

*Parameter #1---references to previous outputs*

+-----------+--------------+-------------+---------------------------------------------------------------------------+
| |n|       | |t|          | |p|         | |d|                                                                       |
+===========+==============+=============+===========================================================================+
| Outpoints | array        | Required    | An array of objects, each one being an unspent outpoint                   |
|           |              | (exactly 1) |                                                                           |
+-----------+--------------+-------------+---------------------------------------------------------------------------+
| Outpoint  | object       | Required    | An object describing a particular unspent outpoint                        |
|           |              | (1 or more) |                                                                           |
+-----------+--------------+-------------+---------------------------------------------------------------------------+
| `txid`    | string (hex) | Required    | The TXID of the outpoint encoded as hex in RPC byte order                 |
|           |              | (exactly 1) |                                                                           |
+-----------+--------------+-------------+---------------------------------------------------------------------------+
| `vout`    | number (int) | Required    | The output index number (vout) of the outpoint; the first output in a     |
|           |              | (exactly 1) | transaction is index `0`                                                  |
+-----------+--------------+-------------+---------------------------------------------------------------------------+

*Parameter #2---P2PKH or P2SH addresses and amounts*

+-----------+--------+-------------+---------------------------------------------------------------------------------+
| |n|       | |t|    | |p|         | |d|                                                                             |
+===========+========+=============+=================================================================================+
| Outpoints | object | Required    | The addresses and amounts to pay                                                |
|           |        | (exactly 1) |                                                                                 |
+-----------+--------+-------------+---------------------------------------------------------------------------------+
| Address/  | number | Required    | A key/value pair with the address to pay as a string (key) and the amount to    |
| Amount    | (ZEC)  | (1 or more) | pay that address (value) in ZEC                                                 |
+-----------+--------+-------------+---------------------------------------------------------------------------------+

*Result---the unsigned raw transaction in hex*

+----------+--------+-------------+----------------------------------------------------------------------------------+
| |n|      | |t|    | |p|         | |d|                                                                              |
+==========+========+=============+==================================================================================+
| `result` | string | Required    | The resulting unsigned raw transaction in serialized transaction format encoded  |
|          |        | (exactly 1) | as hex.  If the transaction couldn't be generated, this will be set to JSON      |
|          |        |             | `null` and the JSON-RPC error field may contain an error message                 |
+----------+--------+-------------+----------------------------------------------------------------------------------+

*Example*
::
  zcash-cli createrawtransaction '''
    [
      {
        "txid": "e9c77353da98e8897bf34ac589db0d3a407987d566cf8cbc19b6cf57e64fa1ad",
        "vout" : 0
      }
    ]''' '{ "t14sSauSf5pF2UkUwvKGq4qjNRzBZYqgEL5": 0.15 }'

Result (wrapped): ::

  01000000011da9283b4ddf8d89eb996988b89ead56cecdc44041ab38bf787f1206cd90b51e0000000000ffffffff01405dc600000000001976a9140dfc8bafc8419853b34d5e072ad37d1a5159f58488ac00000000

*See also*

- `DecodeRawTransaction`_: |summary_decodeRawTransaction|
- `SignRawTransaction`_: |summary_signRawTransaction|
- `SendRawTransaction`_: |summary_sendRawTransaction|
- `Serialized Transaction Format`_

DecodeRawTransaction
++++++++++++++++++++
The `decoderawtransaction` RPC |summary_decodeRawTransaction|

.. |summary_decodeRawTransaction| replace:: decodes a serialized transaction hex string into a JSON object describing the transaction.

*Parameter #1---serialized transaction in hex*

+-------------+--------------+-------------+-------------------------------------------------------------------------+
| |n|         | |t|          | |p|         | |d|                                                                     |
+=============+==============+=============+=========================================================================+
| Serialized  | string (hex) | Required    | The transaction to decode in serialized transaction format              |
| Transaction |              | (exactly 1) |                                                                         |
+-------------+--------------+-------------+-------------------------------------------------------------------------+

*Result---the decoded transaction*

+----------------+--------------+-------------+--------------------------------------------------------------------+
| |n|            | |t|          | |p|         | |d|                                                                |
+================+==============+=============+====================================================================+
| `result`       | object       | Required    | An object describing the decoded transaction, or JSON `null` if    |
|                |              | (exactly 1) | the transaction could not be decoded                               |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| →              | string (hex) | Required    | The transaction's TXID encoded as hex in RPC byte order            |
| `txid`         |              | (exactly 1) |                                                                    |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| →              | number (int) | Required    | The transaction format version number                              |
| `version`      |              | (1 or more) |                                                                    |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| →              | number (int) | Required    | The transaction's locktime: either a Unix epoch date or block      |
| `locktime`     |              | (exactly 1) | height; see the `Locktime parsing rules`_                          |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| →              | array        | Required    | An array of objects with each object being an input vector (vin)   |
| `vin`          |              | (exactly 1) | for this transaction.  Input objects will have the same order      |
|                |              |             | within the array as they have in the transaction, so the first     |
|                |              |             | input listed will be input 0                                       |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| → →            | object       | Required    | An object describing one of this transaction's inputs.  May be a   |
| Input          |              | (1 or more) | regular input or a coinbase                                        |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| → → →          | string       | Optional    | The TXID of the outpoint being spent, encoded as hex in RPC byte   |
| `txid`         |              | (0 or 1)    | order.  Not present if this is a coinbase transaction              |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| → → →          | number (int) | Optional    | The output index number (vout) of the outpoint being spent.  The   |
| `vout`         |              | (0 or 1)    | first output in a transaction has an index of `0`.  Not present if |
|                |              |             | this is a coinbase transaction                                     |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| → → →          | object       | Optional    | An object describing the signature script of this input.  Not  if  |
| `scriptSig`    |              | (0 or 1)    | this is a coinbase present transaction                             |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| → → → →        | string       | Required    | The signature script in decoded form with non-data-pushing opcodes |
| `asm`          |              | (exactly 1) | listed                                                             |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| → → → →        | string (hex) | Required    | The signature script encoded as hex                                |
| `hex`          |              | (exactly 1) |                                                                    |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| → → →          | string (hex) | Optional    | The coinbase (similar to the hex field of a scriptSig) encoded as  |
| `coinbase`     |              | (0 or 1)    | hex. Only present if this is a coinbase transaction                |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| → → →          | number (int) | Required    | The input sequence number                                          |
| `sequence`     |              | (exactly 1) |                                                                    |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| →              | array        | Required    | An array of objects each describing an output vector (vout) for    |
| `vout`         |              | (exactly 1) | this transaction. Output objects will have the same order within   |
|                |              |             | the array as they have in the transaction, so the first output     |
|                |              |             | listed will be output 0                                            |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| → →            | object       | Required    | An object describing one of this transaction's outputs             |
| Output         |              | (1 or more) |                                                                    |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| → → →          | number (ZEC) | Required    | The number of ZEC paid to this output.  May be `0`                 |
| `value`        |              | (exactly 1) |                                                                    |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| → → →          | number (int) | Required    | The output index number of this output within this transaction     |
| `n`            |              | (exactly 1) |                                                                    |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| → → →          | object       | Required    | An object describing the pubkey script                             |
| `scriptPubKey` |              | (exactly 1) |                                                                    |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| → → → →        | string       | Required    | The pubkey script in decoded form with non-data-pushing opcodes    |
| `asm`          |              | (exactly 1) | listed                                                             |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| → → → →        | string (hex) | Required    | The pubkey script encoded as hex                                   |
| `hex`          |              | (exactly 1) |                                                                    |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| → → → →        | number (int) | Optional    | The number of signatures required; this is always `1` for P2PK,    |
| `regSigs`      |              | (0 or 1)    | P2PKH, and P2SH (including P2SH multisig because the redeem script |
|                |              |             | is not available in the pubkey script).  It may be greater than 1  |
|                |              |             | for bare multisig.  This value will not be returned for `nulldata` |
|                |              |             | or `nonstandard` script types (see the `type` key below)           |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| → → → →        | string       | Optional    | The type of script.  This will be one of the following:            |
| `type`         |              | (0 or 1)    | - `pubkey` for a P2PK script                                       |
|                |              |             | - `pubkeyhash` for a P2PKH script                                  |
|                |              |             | - `scripthash` for a P2SH script                                   |
|                |              |             | - `multisig` for a bare multisig script                            |
|                |              |             | - `nulldata` for nulldata scripts                                  |
|                |              |             | - `nonstandard` for unknown scripts                                |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| → → → →        | string:      | Optional    | The P2PKH or P2SH addresses used in this transaction, or the       |
| `addresses`    | array        | (0 or 1)    | computed P2PKH address of any pubkeys in this transaction.  This   |
|                |              |             | array will not be returned for `nulldata` or `nonstandard` script  |
|                |              |             | types                                                              |
+----------------+--------------+-------------+--------------------------------------------------------------------+
| → → → → →      | string:      | Required    | A P2PKH or P2SH address                                            |
| `Address`      |              | (1 or more) |                                                                    |
+----------------+--------------+-------------+--------------------------------------------------------------------+

*Example*

Decode a signed one-input, three-output transaction: ::

  zcash-cli decoderawtransaction 0100000001268a9ad7bfb21d3c086f0ff28f73a064964aa069ebb69a9e437da85c7e55c7d7000000006b483045022100ee69171016b7dd218491faf6e13f53d40d64f4b40123a2de52560feb95de63b902206f23a0919471eaa1e45a0982ed288d374397d30dff541b2dd45a4c3d0041acc0012103a7c1fd1fdec50e1cf3f0cc8cb4378cd8e9a2cee8ca9b3118f3db16cbbcf8f326ffffffff0350ac6002000000001976a91456847befbd2360df0e35b4e3b77bae48585ae06888ac80969800000000001976a9142b14950b8d31620c6cc923c5408a701b1ec0a02088ac002d3101000000001976a9140dfc8bafc8419853b34d5e072ad37d1a5159f58488ac00000000

Result: ::

  {
      "txid" : "e9c77353da98e8897bf34ac589db0d3a407987d566cf8cbc19b6cf57e64fa1ad",
      "version" : 1,
      "locktime" : 0,
      "vin" : [
          {
              "txid" : "b039c06ed7d729907f7d23f7edd017e413fe550afe78b50e3d8af241e1666d2f",
              "vout" : 0,
              "scriptSig" : {
                  "asm" : "3045022100ee69171016b7dd218491faf6e13f53d40d64f4b40123a2de52560feb95de63b902206f23a0919471eaa1e45a0982ed288d374397d30dff541b2dd45a4c3d0041acc001 03a7c1fd1fdec50e1cf3f0cc8cb4378cd8e9a2cee8ca9b3118f3db16cbbcf8f326",
                  "hex" : "483045022100ee69171016b7dd218491faf6e13f53d40d64f4b40123a2de52560feb95de63b902206f23a0919471eaa1e45a0982ed288d374397d30dff541b2dd45a4c3d0041acc0012103a7c1fd1fdec50e1cf3f0cc8cb4378cd8e9a2cee8ca9b3118f3db16cbbcf8f326"
              },
              "sequence" : 4294967295
          }
      ],
      "vout" : [
          {
              "value" : 0.39890000,
              "n" : 0,
              "scriptPubKey" : {
                  "asm" : "OP_DUP OP_HASH160 56847befbd2360df0e35b4e3b77bae48585ae068 OP_EQUALVERIFY OP_CHECKSIG",
                  "hex" : "76a91456847befbd2360df0e35b4e3b77bae48585ae06888ac",
                  "reqSigs" : 1,
                  "type" : "pubkeyhash",
                  "addresses" : [
                      "t14sSauSf5pF2UkUwvKGq4qjNRzBZYqgEL5"
                  ]
              }
          },
          {
              "value" : 0.10000000,
              "n" : 1,
              "scriptPubKey" : {
                  "asm" : "OP_DUP OP_HASH160 2b14950b8d31620c6cc923c5408a701b1ec0a020 OP_EQUALVERIFY OP_CHECKSIG",
                  "hex" : "76a9142b14950b8d31620c6cc923c5408a701b1ec0a02088ac",
                  "reqSigs" : 1,
                  "type" : "pubkeyhash",
                  "addresses" : [
                      "t1mtya1DiYpTF166zTsBnB3nY1BP8LsSqN7"
                  ]
              }
          },
          {
              "value" : 0.20000000,
              "n" : 2,
              "scriptPubKey" : {
                  "asm" : "OP_DUP OP_HASH160 0dfc8bafc8419853b34d5e072ad37d1a5159f584 OP_EQUALVERIFY OP_CHECKSIG",
                  "hex" : "76a9140dfc8bafc8419853b34d5e072ad37d1a5159f58488ac",
                  "reqSigs" : 1,
                  "type" : "pubkeyhash",
                  "addresses" : [
                      "t1PLfoTX6Z3XuWPpS45hntVZerDKCkrsMg3"
                  ]
              }
          }
      ]
  }

*See also*

- `CreateRawTransaction`_: |summary_createRawTransaction|
- `SignRawTransaction`_: |summary_signRawTransaction|
- `SendRawTransaction`_: |summary_sendRawTransaction|

DecodeScript
++++++++++++

The `decodescript` RPC |summary_decodeScript|

.. |summary_decodeScript| replace:: decodes a hex-encoded P2SH redeem script.

*Parameter #1---a hex-encoded redeem script*

+----------------+--------------+-------------+----------------------------------------------------------------------+
| |n|            | |t|          | |p|         | |d|                                                                  |
+================+==============+=============+======================================================================+
| Redeem Script  | string (hex) | Required    | The redeem script to decode as a hex-encoded serialized script       |
|                |              | (exactly 1) |                                                                      |
+----------------+--------------+-------------+----------------------------------------------------------------------+

*Result---the decoded script*

+----------------+--------------+-------------+----------------------------------------------------------------------+
| |n|            | |t|          | |p|         | |d|                                                                  |
+================+==============+=============+======================================================================+
| `result`       | object       | Required    | An object describing the decoded script, or JSON `null` if the       |
|                |              | (exactly 1) | script could not be decoded                                          |
+----------------+--------------+-------------+----------------------------------------------------------------------+
| →              | string       | Required    | The redeem script in decoded form with non-data-pushing opcodes      |
| `asm`          |              | (exactly 1) | listed.  May be empty                                                |
+----------------+--------------+-------------+----------------------------------------------------------------------+
| →              | string       | Optional    | The type of script.  This will be one of the following:              |
| `type`         |              | (0 or 1)    |                                                                      |
|                |              |             | - `pubkey` for a P2PK script inside P2SH                             |
|                |              |             | - `pubkeyhash` for a P2PKH script inside P2SH                        |
|                |              |             | - `multisig` for a multisig script inside P2SH                       |
|                |              |             | - `nonstandard` for unknown scripts                                  |
+----------------+--------------+-------------+----------------------------------------------------------------------+
| →              | number (int) | Optional    | The number of signatures required; this is always `1` for P2PK or    |
| `regSigs`      |              | (0 or 1)    | P2PKH within P2SH.  It may be greater than 1 for P2SH multisig. This |
|                |              |             | value will not be returned for `nonstandard` script types (see the   |
|                |              |             | `type` key above)                                                    |
+----------------+--------------+-------------+----------------------------------------------------------------------+
| →              | array        | Optional    | A P2PKH addresses used in this script, or the computed P2PKH         |
| `addresses`    |              | (0 or 1)    | addresses of any pubkeys in this script.  This array will not be     |
|                |              |             | returned for `nonstandard` script types                              |
+----------------+--------------+-------------+----------------------------------------------------------------------+
| → →            | string       | Required    | A P2PKH address                                                      |
| Address        |              | (1 or more) |                                                                      |
+----------------+--------------+-------------+----------------------------------------------------------------------+
| →              | string (hex) | Required    | The P2SH address of this redeem script                               |
| `p2sh`         |              | (exactly 1) |                                                                      |
+----------------+--------------+-------------+----------------------------------------------------------------------+

*Example*

A 2-of-3 P2SH multisig pubkey script: ::

  zcash-cli decodescript 522103ede722780d27b05f0b1169efc90fa15a601a32fc6c3295114500c586831b6aaf2102ecd2d250a76d204011de6bc365a56033b9b3a149f679bc17205555d3c2b2854f21022d609d2f0d359e5bc0e5d0ea20ff9f5d3396cb5b1906aa9c56a0e7b5edc0c5d553ae

Result: ::

  {
      "asm" : "2 03ede722780d27b05f0b1169efc90fa15a601a32fc6c3295114500c586831b6aaf 02ecd2d250a76d204011de6bc365a56033b9b3a149f679bc17205555d3c2b2854f 022d609d2f0d359e5bc0e5d0ea20ff9f5d3396cb5b1906aa9c56a0e7b5edc0c5d5 3 OP_CHECKMULTISIG",
      "reqSigs" : 2,
      "type" : "multisig",
      "addresses" : [
          "t14sSauSf5pF2UkUwvKGq4qjNRzBZYqgEL5",
          "t1PoHp2v54vfmdgQ3v3SNuQga8JKHTNi2a1",
          "t1LLfoTX6Z3XuWPpS45hntVZerDKCkrsMg3"
      ],
      "p2sh" : "t3yVxxgNBk5zHRPRY2iVjGRJHYZEp1pMCSq"
  }

*See also*

- `CreateMultiSig`_: |summary_createMultiSig|
- `Pay-To-Script-Hash`_ </en/glossary/p2sh-address>

DumpPrivKey
+++++++++++
*Requires wallet support. Requires an unlocked wallet or an unencrypted wallet.*

The `dumpprivkey` RPC {{summary_dumpPrivKey}}

.. |summary_dumpPrivKey| replace:: returns the wallet-import-format (WIP) private key corresponding to a transparent address. (But does not remove it from the wallet.)

*Parameter #1---the address corresponding to the private key to get*

+---------+-----------------+-------------+--------------------------------------------------------------------------+
| |n|     | |t|             | |p|         | |d|                                                                      |
+=========+=================+=============+==========================================================================+
| P2PKH   | string (base58) | Required    | The P2PKH address corresponding to the private key you want returned.    |
| Address |                 | (exactly 1) | Must be the address corresponding to a private key in this wallet        |
+---------+-----------------+-------------+--------------------------------------------------------------------------+

*Result---the private key*

+----------+-----------------+-------------+-------------------------------------------------------------------------+
| |n|      | |t|             | |p|         | |d|                                                                     |
+==========+=================+=============+=========================================================================+
| `result` | string (base58) | Required    | The private key encoded as base58check using wallet import format       |
|          |                 | (exactly 1) |                                                                         |
+----------+-----------------+-------------+-------------------------------------------------------------------------+

*Example*
::

  zcash-cli dumpprivkey t14sSauSf5pF2UkUwvKGq4qjNRzBZYqgEL5

Result: ::

  KxPQjJKyK2MT8hnbuXNutcqfh81t3XxHoTfQyF857qXuQHoTaGbX

*See also*

- `ImportPrivKey`_: |summary_importPrivKey|
- `DumpWallet`_: |summary_dumpWallet|

DumpWallet
++++++++++
*Requires wallet support.  Requires an unlocked wallet or an unencrypted wallet.*

The `dumpwallet` RPC {{summary_dumpWallet}}

.. |summary_dumpWallet| replace:: creates or overwrites a file with all of the wallet's transparent keys in a human-readable format to the specified alphanumeric filename only after setting ``-exportdir=``.

*Parameter #1---a filename*

+----------+----------------+-------------+--------------------------------------------------------------------------+
| |n|      | |t|            | |p|         | |d|                                                                      |
+==========+================+=============+==========================================================================+
| Filename | string         | Required    | A filename to be created or overwritten within the directory specified   |
|          | (alphanumeric) | (exactly 1) | in ``-exportdir=``.                                                      |
+----------+----------------+-------------+--------------------------------------------------------------------------+

*Result---the full path of the destination file*

+----------+----------------------+-------------+--------------------------------------------------------------------+
| |n|      | |t|                  | |p|         | |d|                                                                |
+==========+======================+=============+====================================================================+
| `result` | string (full path    | Required    | Returns full path of destination file or error message. The        |
|          | of destination file) | (exactly 1) | JSON-RPC error and message fields will be set if a failure         |
|          |                      |             | occurred                                                           |
+----------+----------------------+-------------+--------------------------------------------------------------------+

*Example*

Dump transparent keys in wallet after setting ``-exportdir=/home/user/zcash-backup/``:  ::

  zcash-cli dumpwallet dump

Results: ::

  /home/user/zcash-backup/dump

*See also*

* `BackupWallet`_: |summary_backupWallet|
* `ImportWallet`_: |summary_importWallet|

EncryptWallet
+++++++++++++
**Wallet encryption is DISABLED. This call always fails.**

EstimateFee
+++++++++++

The `estimatefee` RPC |summary_estimateFee|

.. |summary_estimateFee| replace:: estimates the transaction fee per kilobyte that needs to be paid for a transaction to be included within a certain number of blocks.

*Parameter #1---how many blocks the transaction may wait before being included*

+----------+--------------+-------------+----------------------------------------------------------------------------+
| |n|      | |t|          | |p|         | |d|                                                                        |
+==========+==============+=============+============================================================================+
| Blocks   | number (int) | Required    | The maximum number of blocks a transaction should have to wait before it   |
|          |              | (exactly 1) | is predicted to be included in a block                                     |
+----------+--------------+-------------+----------------------------------------------------------------------------+

*Result---the fee the transaction needs to pay per kilobyte*

+----------+--------------+-------------+----------------------------------------------------------------------------+
| |n|      | |t|          | |p|         | |d|                                                                        |
+==========+==============+=============+============================================================================+
| `result` | number (ZEC) | Required    | The estimated fee the transaction should pay in order to be included       |
|          |              | (exactly 1) |  within the specified number of blocks.  If the node doesn't have enough   |
|          |              |             | information to make an estimate, the value `-1` will be returned           |
+----------+--------------+-------------+----------------------------------------------------------------------------+

*Examples*
::
  zcash-cli estimatefee 3

Result:

  0.00005000

Requesting data the node can't calculate yet: ::

  zcash-cli estimatefee 200

Result: ::

  -1

*See also*

* `EstimatePriority`_: |summary_estimatePriority|
* `SetTxFee`_: |summary_setTxFee|

EstimatePriority
++++++++++++++++
The `estimatepriority` RPC {{summary_estimatePriority}}

.. |summary_estimatePriority| replace:: estimates the priority that a transaction needs in order to be included within a certain number of blocks as a free high-priority transaction.

Transaction priority is relative to a transaction's byte size.

*Parameter #1---how many blocks the transaction may wait before being included as a free high-priority transaction*

+----------+--------------+-------------+----------------------------------------------------------------------------+
| |n|      | |t|          | |p|         | |d|                                                                        |
+==========+==============+=============+============================================================================+
| Blocks   | number (int) | Required    | The maximum number of blocks a transaction should have to wait before it   |
|          |              | (exactly 1) | is predicted to be included in a block based purely on its priority        |
+----------+--------------+-------------+----------------------------------------------------------------------------+

*Result---the priority a transaction needs*

+----------+--------+-------------+----------------------------------------------------------------------------------+
| |n|      | |t|    | |p|         | |d|                                                                              |
+==========+========+=============+==================================================================================+
| `result` | number | Required    | The estimated priority the transaction should have in order to be included       |
|          | (real) | (exactly 1) |  within the specified number of blocks.  If the node doesn't have enough         |
|          |        |             | information to make an estimate, the value `-1` will be returned                 |
+----------+--------+-------------+----------------------------------------------------------------------------------+

*Examples*
::
  zcash-cli estimatepriority 6

Result: ::

  718158904.10958910

Requesting data the node can't calculate yet: ::

  zcash-cli estimatepriority 100

Result: ::

  -1

*See also*

.. `EstimateFee`_: |summary_estimateFee|


.. table::

  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | →              | string (hex) | Required    | The transaction's TXID encoded as hex in RPC byte order            |
  | `txid`         |              | (exactly 1) |                                                                    |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | →              | number (int) | Required    | The transaction format version number                              |
  | `version`      |              | (1 or more) |                                                                    |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | →              | number (int) | Required    | The transaction's locktime: either a Unix epoch date or block      |
  | `locktime`     |              | (exactly 1) | height; see the `Locktime parsing rules`_                          |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | →              | array        | Required    | An array of objects with each object being an input vector (vin)   |
  | `vin`          |              | (exactly 1) | for this transaction.  Input objects will have the same order      |
  |                |              |             | within the array as they have in the transaction, so the first     |
  |                |              |             | input listed will be input 0                                       |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | → →            | object       | Required    | An object describing one of this transaction's inputs.  May be a   |
  | Input          |              | (1 or more) | regular input or a coinbase                                        |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | → → →          | string       | Optional    | The TXID of the outpoint being spent, encoded as hex in RPC byte   |
  | `txid`         |              | (0 or 1)    | order.  Not present if this is a coinbase transaction              |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | → → →          | number (int) | Optional    | The output index number (vout) of the outpoint being spent.  The   |
  | `vout`         |              | (0 or 1)    | first output in a transaction has an index of `0`.  Not present if |
  |                |              |             | this is a coinbase transaction                                     |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | → → →          | object       | Optional    | An object describing the signature script of this input.  Not  if  |
  | `scriptSig`    |              | (0 or 1)    | this is a coinbase present transaction                             |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | → → → →        | string       | Required    | The signature script in decoded form with non-data-pushing opcodes |
  | `asm`          |              | (exactly 1) | listed                                                             |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | → → → →        | string (hex) | Required    | The signature script encoded as hex                                |
  | `hex`          |              | (exactly 1) |                                                                    |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | → → →          | string (hex) | Optional    | The coinbase (similar to the hex field of a scriptSig) encoded as  |
  | `coinbase`     |              | (0 or 1)    | hex. Only present if this is a coinbase transaction                |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | → → →          | number (int) | Required    | The input sequence number                                          |
  | `sequence`     |              | (exactly 1) |                                                                    |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | →              | array        | Required    | An array of objects each describing an output vector (vout) for    |
  | `vout`         |              | (exactly 1) | this transaction. Output objects will have the same order within   |
  |                |              |             | the array as they have in the transaction, so the first output     |
  |                |              |             | listed will be output 0                                            |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | → →            | object       | Required    | An object describing one of this transaction's outputs             |
  | Output         |              | (1 or more) |                                                                    |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | → → →          | number (ZEC) | Required    | The number of ZEC paid to this output.  May be `0`                 |
  | `value`        |              | (exactly 1) |                                                                    |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | → → →          | number (int) | Required    | The output index number of this output within this transaction     |
  | `n`            |              | (exactly 1) |                                                                    |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | → → →          | object       | Required    | An object describing the pubkey script                             |
  | `scriptPubKey` |              | (exactly 1) |                                                                    |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | → → → →        | string       | Required    | The pubkey script in decoded form with non-data-pushing opcodes    |
  | `asm`          |              | (exactly 1) | listed                                                             |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | → → → →        | string (hex) | Required    | The pubkey script encoded as hex                                   |
  | `hex`          |              | (exactly 1) |                                                                    |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | → → → →        | number (int) | Optional    | The number of signatures required; this is always `1` for P2PK,    |
  | `regSigs`      |              | (0 or 1)    | P2PKH, and P2SH (including P2SH multisig because the redeem script |
  |                |              |             | is not available in the pubkey script).  It may be greater than 1  |
  |                |              |             | for bare multisig.  This value will not be returned for `nulldata` |
  |                |              |             | or `nonstandard` script types (see the `type` key below)           |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | → → → →        | string       | Optional    | The type of script.  This will be one of the following:            |
  | `type`         |              | (0 or 1)    | - `pubkey` for a P2PK script                                       |
  |                |              |             | - `pubkeyhash` for a P2PKH script                                  |
  |                |              |             | - `scripthash` for a P2SH script                                   |
  |                |              |             | - `multisig` for a bare multisig script                            |
  |                |              |             | - `nulldata` for nulldata scripts                                  |
  |                |              |             | - `nonstandard` for unknown scripts                                |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | → → → →        | string:      | Optional    | The P2PKH or P2SH addresses used in this transaction, or the       |
  | `addresses`    | array        | (0 or 1)    | computed P2PKH address of any pubkeys in this transaction.  This   |
  |                |              |             | array will not be returned for `nulldata` or `nonstandard` script  |
  |                |              |             | types                                                              |
  +----------------+--------------+-------------+--------------------------------------------------------------------+
  | → → → → →      | string:      | Required    | A P2PKH or P2SH address                                            |
  | `Address`      |              | (1 or more) |                                                                    |
  +----------------+--------------+-------------+--------------------------------------------------------------------+




.. |n| replace:: Name
.. |t| replace:: Type
.. |p| replace:: Presence
.. |d| replace:: Description
