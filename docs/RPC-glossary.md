
##### AddMultiSigAddress

{% assign summary_addMultiSigAddress="adds a nRequired-to-sign multisignature transparent address to the wallet.<br>
Each key is a transparent Zcash address or hex-encoded public key.<br>
If 'account' is specified (DEPRECATED), assign address to that account." %}

*Requires wallet support.*

The `addmultisigaddress` RPC {{summary_addMultiSigAddress}}

*Arguments:*

{% itemplate ntpd1 %}
- n: "nRequired"
  t: "number (int)"
  p: "Required<br>(exactly 1)"
  d: "The number of required signatures out of the n keys or addresses."

- n: "Keyobject"
  t: "array"
  p: "Required<br>(exactly 1)"
  d: "A json array of transparent Zcash addresses or their hex-encoded public keys. They array's size must me equal to or greater than nRequired."

- n: "→<br>Address"
  t: "string"
  p: "Required<br>(1 or more)"
  d: "A transparent Zcash address or it's hex-encoded public key."

- n: "Account"
  t: "string"
  p: "Optional<br>(0 or 1)"
  d: "DEPRECATED. If provided, MUST be set to the empty string "" to represent the default account. Passing any other string will result in an error."

{% enditemplate %}

*Result:*

{% itemplate ntpd1 %}
- n: "Zcashaddress"
  t: "string"
  p: "Required<br>(exactly 1)"
  d: "The transparent Zcash multisig address associated with the keys provided in keyobject array."

{% enditemplate %}

*Example:*

Adding a multisig address from 2 addresses:

{% highlight bash %}
zcash-cli addmultisigaddress 2 "[\"t16sSauSf5pF2UkUwvKGq4qjNRzBZYqgEL5\",\"t171sgjn4YtPu27adkKGrdDwzRTxnRkBfKV\"]"
{% endhighlight %}

Result:

{% highlight text %}
t3fYkRZMkJmSKMdvEPRrkV5aixMuoRRRdgU
{% endhighlight %}

(The new P2SH multisig address is also stored in the wallet.)

*See also*

* [CreateMultiSig][rpc createmultisig]: {{summary_createMultiSig}}
* [DecodeScript][rpc decodescript]: {{summary_decodeScript}}

##### AddNode

{% assign summary_addNode="attempts to add or remove a node from the addnode list, or to try a connection to a node once." %}

The `addnode` RPC {{summary_addNode}}

*Arguments:*

{% itemplate ntpd1 %}
- n: "Node"
  t: "string"
  p: "Required<br>(exactly 1)"
  d: "The node to add."

- n: "Command"
  t: "string"
  p: "Required<br>(exactly 1)"
  d: "What to do with the node.  Options are:<br>• `add` to add a node to the addnode list.  Up to 8 nodes can be added additional to the default 8 nodes. Not limited by `-maxconnections`<br>• `remove` to remove a node from the list.  If currently connected, this will disconnect immediately<br>• `onetry` to immediately attempt connection to the node even if the outgoing connection slots are full; this will only attempt the connection once."

{% enditemplate %}

*Result:*

{% itemplate ntpd1 %}
- n: "`result`"
  t: "null"
  p: "Required<br>(exactly 1)"
  d: "Always JSON `null` whether the node was added, removed, tried-and-connected, or tried-and-not-connected.  The JSON-RPC error field will be set only if you try removing a node that is not on the addnodes list"

{% enditemplate %}

*Example:*

Try connecting to the following node.

{% highlight bash %}
zcash-cli addnode "192.168.0.6:8233" "onetry"
{% endhighlight %}

*See also*

* [GetAddedNodeInfo][rpc getaddednodeinfo]: {{summary_getAddedNodeInfo}}
* [GetPeerInfo][rpc getpeerinfo]: {{summary_getPeerInfo}}

##### BackupWallet

{% assign summary_backupWallet="safely copies `wallet.dat` to the specified alphanumeric filename only after setting `-exportdir=`." %}

*Requires wallet support.*

The `backupwallet` RPC {{summary_backupWallet}}

*Parameter #1---destination directory or filename*

{% itemplate ntpd1 %}
- n: "Destination"
  t: "string"
  p: "Required<br>(exactly 1)"
  d: "A filename to be created or overwritten within the directory specified in `-exportdir=`."

{% enditemplate %}

*Result---destination file path*

{% itemplate ntpd1 %}
- n: "Filepath"
  t: "string"
  p: "Required<br>(exactly 1)"
  d: "Returns the full path of destination file or error message. The JSON-RPC error and message fields will be set if a failure occurred."

{% enditemplate %}

*Example:*

Backup wallet after setting `-exportdir=/home/user/zcash-backup/`:

{% highlight bash %}
zcash-cli backupwallet backup
{% endhighlight %}

{% highlight text %}
/home/user/zcash-backup/backup

*See also*

* [DumpWallet][rpc dumpwallet]: {{summary_dumpWallet}}
* [ImportWallet][rpc importwallet]: {{summary_importWallet}}

##### ClearBanned

{% assign summary_clearBanned="clears list of banned nodes." %}

The `clearbanned` RPC {{summary_clearBanned}}

*Arguments: none*

*Result---`null` on success*

{% itemplate ntpd1 %}
- n: "`result`"
  t: "null"
  p: "Required<br>(exactly 1)"
  d: "JSON `null` when the list was cleared"

{% enditemplate %}

*Example:*

Clears the ban list.

{% highlight bash %}
zcash-cli clearbanned
{% endhighlight %}

Result (no output from `zcash-cli` because result is set to `null`).

*See also*

* [ListBanned][rpc listbanned]: {{summary_listBanned}}
* [SetBan][rpc setban]: {{summary_setBan}}

##### CreateMultiSig

{% assign summary_createMultiSig="creates a P2SH multi-signature address with n signatures of m keys required. *Note: multisig functionality is only supported for transparent addresses.*" %}

The `createmultisig` RPC {{summary_createMultiSig}}

*Arguments:*

{% itemplate ntpd1 %}
- n: "nRequired"
  t: "number (int)"
  p: "Required<br>(exactly 1)"
  d: "The number of signatures required out of the n keys or addresses."

- n: "Keysobject"
  t: "array"
  p: "Required<br>(exactly 1)"
  d: "A json array of transparent Zcash addresses or their hex-encoded public keys. They array's size must me equal to or greater than nRequired."

- n: "→<br>Address"
  t: "string"
  p: "Required<br>(1 or more)"
  d: "A transparent Zcash address or it's hex-encoded public key."
{% enditemplate %}

*Result:*

{% itemplate ntpd1 %}
- n: "`result`"
  t: "object"
  p: "Required<br>(exactly 1)"
  d: "An object describing the multisig address"

- n: "→<br>`address`"
  t: "string (base58)"
  p: "Required<br>(exactly 1)"
  d: "The value of the new multisig address."

- n: "→<br>`redeemScript`"
  t: "string (hex)"
  p: "Required<br>(exactly 1)"
  d: "The string value of the hex-encoded redemption script."

{% enditemplate %}

*Example:*

Creating a multisig address from 2 addresses:

{% highlight bash %}
zcash-cli createmultisig 2 "[\"t16sSauSf5pF2UkUwvKGq4qjNRzBZYqgEL5\",\"t171sgjn4YtPu27adkKGrdDwzRTxnRkBfKV\"]"
{% endhighlight %}

Result:

{%highlight json %}
{
  "address" : "t3yVxxgNBk5zHRPRY2iVjGRJHYZEp1pMCSq",
  "redeemScript" : "5221030740697daa0fb95efd120d0bfcd8c79cb3dda91fea6454041b4b2e34da56b2a72103cb43cc9d070a980b639e72879b81c1fe5537d48736cb5380d024de520b2a350a52ae"
}
{% endhighlight %}

*See also*

* [AddMultiSigAddress][rpc addmultisigaddress]: {{summary_addMultiSigAddress}}
* [DecodeScript][rpc decodescript]: {{summary_decodeScript}}
