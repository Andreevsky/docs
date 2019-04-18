<html>

<head>
<h1 style='line-height:137%'><a name="_mpgin9mcrkvf"></a><span lang=EN>Introduction</span></h1>

<h2><a name="_i8imargcgha5"></a><span lang=EN>Starting the daemon and the
wallet application as RPC server</span></h2>

<p class=normal style='line-height:137%'><span lang=EN>Zano command-line wallet
application (</span><span lang=EN style='font-family:Consolas;mso-fareast-font-family:
Consolas;mso-bidi-font-family:Consolas'>simplewallet</span><span lang=EN>) can
be run in RPC server mode. In this mode it can be controlled by RPC calls via
HTTP and be used as a back-end for an arbitrary service.</span></p>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><span lang=EN>Starting the wallet in
RPC server mode:</span></p>

<p class=normal style='line-height:137%'><span lang=EN>1. Run zanod (the daemon
application).</span></p>

<p class=normal style='line-height:137%'><span lang=EN>2. Run </span><span
lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas'>simplewallet </span><span lang=EN>with the
following options:</span></p>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='margin-left:35.0pt;text-indent:-21.0pt;line-height:137%'><span
lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas;background:#CCCCCC'>simplewallet --wallet-file
PATH_TO_WALLET_FILE --password PASSWORD --rpc-bind-ip RPC_IP --rpc-bind-port
RPC_PORT --daemon-address DEAMON_ADDR:DAEMON_PORT --log-file LOG_FILE_NAME<o:p></o:p></span></p>

<p class=normal><span lang=EN style='font-family:Consolas;mso-fareast-font-family:
Consolas;mso-bidi-font-family:Consolas;background:#CCCCCC'><o:p>&nbsp;</o:p></span></p>

<p class=normal style='margin-left:35.0pt;text-indent:-21.0pt;line-height:137%'><span
lang=EN>where:</span></p>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='margin-left:35.0pt;text-indent:-21.0pt;line-height:137%'><span
lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas;background:#CCCCCC'>PATH_TO_WALLET_FILE</span><span
lang=EN> — path to an existing wallet file (should be created beforehand
using<span style='mso-spacerun:yes'>  </span></span><span lang=EN
style='font-family:Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:
Consolas;background:#CCCCCC'>--generate-new-wallet</span><span lang=EN>
command);</span></p>

<p class=normal style='margin-left:35.0pt;text-indent:-21.0pt;line-height:137%'><span
lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas;background:#CCCCCC'>PASSWORD</span><span lang=EN>
— wallet password;</span></p>

<p class=normal style='margin-left:35.0pt;text-indent:-21.0pt;line-height:137%'><span
lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas;background:#CCCCCC'>RPC_IP</span><span lang=EN> —
IP address to bind RPC server to (127.0.0.1 will be used if not specified);</span></p>

<p class=normal style='margin-left:35.0pt;text-indent:-21.0pt;line-height:137%'><span
lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas;background:#CCCCCC'>RPC_PORT</span><span lang=EN>
— TCP port for RPC server;</span></p>

<p class=normal style='margin-left:35.0pt;text-indent:-21.0pt;line-height:137%'><span
lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas;background:#CCCCCC'>DEAMON_ADDR:DAEMON_PORT</span><span
lang=EN> — daemon address and port (may be omitted if the daemon is running on
the same machine with the default settings);</span></p>

<p class=normal style='margin-left:35.0pt;text-indent:-21.0pt;line-height:137%'><span
lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas;background:#CCCCCC'>LOG_FILE_NAME</span><span
lang=EN> — path and filename of simplewallet log file.</span></p>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><span lang=EN>Examples below are given
with assumption that the wallet application is running in RPC server mode and
listening at 127.0.0.1:12233.</span></p>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><span lang=EN>All amounts and balances
are represented as unsigned integers and measured in atomic units — the
smallest fraction of a coin. One coin equals 10<sup>12</sup> atomic units.</span></p>

<p class=normal style='line-height:137%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<h2 style='line-height:137%'><a name="_pfmkvw36e4d3"></a><span lang=EN>Integrated
addresses for exchanges</span></h2>

<p class=normal style='line-height:137%'><span lang=EN>Unlike Bitcoin,
CryptoNote family coins have different, more effective approach on how to
handle user deposits.</span></p>

<p class=normal style='line-height:137%'><span lang=EN>An exchange generates
only one address for receiving coins and all users send coins to that address.
To distinguish different deposits from different users the exchange generates
random identifier (called <b style='mso-bidi-font-weight:normal'>payment ID</b>)
for each one and a user attaches this payment ID to his transaction while
sending. Upon receiving, the exchange can extract payment ID and thus identify
the user.</span></p>

<p class=normal style='line-height:137%'><span lang=EN>In original CryptoNote
there were two separate things: exchange deposit address (the same for all
users) and payment ID (unique for all users). Later, for user convenience and
to avoid missing payment ID we combined them together into one thing, called <b
style='mso-bidi-font-weight:normal'>integrated address</b>. So nowadays modern
exchanges usually give to a user an integrated address for depositing instead
of pair of standard deposit address and a payment ID.</span></p>

<p class=normal style='line-height:137%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><span lang=EN>For more information on
how to handle integrated addresses, please refer to RPCs </span><span lang=EN
style='font-family:Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:
Consolas'>make_integrated_address</span><span lang=EN> and </span><span
lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas'>split_integrated_address</span><span lang=EN>
below.</span></p>

<p class=normal style='line-height:137%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<h1 style='margin-bottom:4.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_nbrfojzgsvna"></a><span lang=EN>List of Wallet
RPCs</span></h1>

<h2 style='margin-bottom:4.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_yy47jm28yga4"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:17.0pt;line-height:137%;font-family:
Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>getbalance<o:p></o:p></span></b></h2>

<p class=normal style='line-height:137%'><span lang=EN>Retrieves current wallet
balance: total and unlocked.</span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Inputs</span></b><span lang=EN>:</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>None</span><span
lang=EN>.</span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Outputs</span></b><span lang=EN>:</span></p>

<p class=normal style='margin-left:36.0pt;text-indent:-36.0pt;line-height:137%'><span
lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas'>balance</span><span lang=EN> — unsigned integer;
total amount of funds the wallet has (unlocked and locked coins).</span></p>

<p class=normal style='margin-left:36.0pt;text-indent:-36.0pt;line-height:137%'><span
lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas'>unlocked_balance</span><span lang=EN> — unsigned
integer; unlocked funds, i.e. coins that are stored deep enough in the
blockchain to be considered relatively safe to spend. Only this amount of coins
are immediately spendable.<br>
</span><span lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas'>unlocked_balance</span><span lang=EN> is always
less or equal to </span><span lang=EN style='font-family:Consolas;mso-fareast-font-family:
Consolas;mso-bidi-font-family:Consolas'>balance</span><span lang=EN>.</span></p>

<h3 style='margin-top:14.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_7bvpla31bsc7"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:13.0pt;line-height:137%;color:black'>Examples<o:p></o:p></span></b></h3>

<table class=a border=0 cellspacing=0 cellpadding=0 width=1104
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:34.0pt'>
  <td width=1104 valign=top style='width:552.0pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:34.0pt'>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>$ curl
  http://127.0.0.1:12233/json_rpc -s -H 'content-type:application/json;'
  --data-binary
  '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;0&quot;,&quot;method&quot;:&quot;getbalance&quot;}'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:109.0pt'>
  <td width=1104 valign=top style='width:552.0pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:109.0pt'>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>{<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span
  style='mso-spacerun:yes'> </span>&quot;id&quot;: &quot;0&quot;,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span
  style='mso-spacerun:yes'> </span>&quot;jsonrpc&quot;: &quot;2.0&quot;,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span
  style='mso-spacerun:yes'> </span>&quot;result&quot;: {<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  
  </span>&quot;balance&quot;: 160810073397000000,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  
  </span>&quot;unlocked_balance&quot;: 160810073397000000<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span
  style='mso-spacerun:yes'> </span>}<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>}<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<h2 style='margin-bottom:4.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_ift5nr6hx7bt"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:17.0pt;line-height:137%;font-family:
Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>getaddress<o:p></o:p></span></b></h2>

<p class=normal style='line-height:137%'><span lang=EN>Obtains wallet public
address.</span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Inputs</span></b><span lang=EN>:</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>None.<o:p></o:p></span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Outputs</span></b><span lang=EN>:</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>address</span><span
lang=EN> — string; standard public address of the wallet.</span></p>

<h3 style='margin-top:14.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_4rqgqjrki2l3"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:13.0pt;line-height:137%;color:black'>Examples<o:p></o:p></span></b></h3>

<table class=a0 border=0 cellspacing=0 cellpadding=0 width=1106
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:34.0pt'>
  <td width=1106 valign=top style='width:552.75pt;background:#D9EAD3;
  padding:5.0pt 5.0pt 5.0pt 5.0pt;height:34.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>$ curl http://127.0.0.1:12233/json_rpc -s -H
  'content-type:application/json;' --data-binary '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;0&quot;,&quot;method&quot;:&quot;getaddress&quot;}'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:109.0pt'>
  <td width=1106 valign=top style='width:552.75pt;background:#CCCCCC;
  padding:5.0pt 5.0pt 5.0pt 5.0pt;height:109.0pt'>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>{<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span
  style='mso-spacerun:yes'> </span>&quot;id&quot;: &quot;0&quot;,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span
  style='mso-spacerun:yes'> </span>&quot;jsonrpc&quot;: &quot;2.0&quot;,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span
  style='mso-spacerun:yes'> </span>&quot;result&quot;: {<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  
  </span>&quot;address&quot;:
  &quot;ZxCb5oL6RTEffiH9gj7w3SYUeQ5s53yUBFGoyGChaqpQdud2uNUaA936Q2ngcEouvmgA48WMZQyv41R2ASstyYHo2Kzeoh7GA&quot;<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span
  style='mso-spacerun:yes'> </span>}<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>}<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<h2 style='margin-bottom:4.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_b9s86ohlfsdn"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:17.0pt;line-height:137%;font-family:
Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>transfer<o:p></o:p></span></b></h2>

<p class=normal style='line-height:137%'><span lang=EN>Creates a transaction
and broadcasts it to the network.</span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Inputs</span></b><span lang=EN>:</span></p>

<p class=normal style='margin-left:22.5pt;line-height:137%'><span lang=EN
style='font-family:Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:
Consolas'>destinations</span><span lang=EN> — list of transfer_destination
objects (see below); list of recipients with corresponding amount of coins for
each.</span></p>

<p class=normal style='margin-left:22.5pt;line-height:137%'><span lang=EN
style='font-family:Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:
Consolas'>fee</span><span lang=EN> — unsigned int; transaction fee in atomic
units. Minimum 10<sup>5</sup> atomic units, recommended 10<sup>6</sup> or 10<sup>7</sup>.</span></p>

<p class=normal style='margin-left:22.5pt;line-height:137%'><span lang=EN
style='font-family:Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:
Consolas'>mixin</span><span lang=EN> — unsigned int; number of foreign outputs
to be mixed in with each input. Increases untraceability. Use 0 for direct and
traceable transfers.</span></p>

<p class=normal style='margin-left:22.5pt;line-height:137%'><span lang=EN
style='font-family:Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:
Consolas'>payment_id</span><span lang=EN> — string; hex-encoded payment id. Can
be empty if payment id is not required for this transfer.</span></p>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>transfer_destination</span></b><span lang=EN> object fields:</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>address</span><span
lang=EN> — string; standard or integrated address of a recipient.</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>amount</span><span
lang=EN> — unsigned int; amount of coins to be sent;</span></p>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Outputs</span></b><span lang=EN>:</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>tx_hash</span><span
lang=EN> — string; hash identifier of the transaction that was successfully
sent.</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>tx_unsigned_hex</span><span
lang=EN> — string; hex-encoded unsigned transaction (for watch-only wallets; to
be used in cold-signing process).</span></p>

<h3 style='margin-top:14.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_qj2cm8as0qtr"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:13.0pt;line-height:137%;color:black'>Examples<o:p></o:p></span></b></h3>

<h4 style='margin-top:12.0pt;margin-right:0cm;margin-bottom:2.0pt;margin-left:
0cm;line-height:137%;mso-pagination:widow-orphan;page-break-after:auto'><a
name="_bnaq8rfbl3ft"></a><b style='mso-bidi-font-weight:normal'><span lang=EN
style='font-size:11.0pt;line-height:137%;color:black'>Not enough money<o:p></o:p></span></b></h4>

<table class=a1 border=0 cellspacing=0 cellpadding=0 width=972
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:60.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:60.0pt'>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>$ curl
  http://127.0.0.1:12233/json_rpc -s -H 'content-type:application/json;'
  --data-binary
  '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;5&quot;,&quot;method&quot;:&quot;transfer&quot;,
  &quot;params&quot;:{&quot;destinations&quot;:[{&quot;address&quot;:&quot;ZxCb5oL6RTEffiH9gj7w3SYUeQ5s53yUBFGoyGChaqpQdud2uNUaA936Q2ngcEouvmgA48WMZQyv41R2ASstyYHo2Kzeoh7GA&quot;,
  &quot;amount&quot;:10000000000000000000}]}}'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:109.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:109.0pt'>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>{<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;error&quot;: {<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-tab-count:1'>       </span>&quot;code&quot;:
  -4,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-tab-count:1'>       </span>&quot;message&quot;:
  &quot;not enough money&quot;<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>},<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;id&quot;: &quot;5&quot;,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;jsonrpc&quot;: &quot;2.0&quot;<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>}<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<h4 style='margin-top:12.0pt;margin-right:0cm;margin-bottom:2.0pt;margin-left:
0cm;line-height:137%;mso-pagination:widow-orphan;page-break-after:auto'><a
name="_u4qifvtpwia4"></a><b style='mso-bidi-font-weight:normal'><span lang=EN
style='font-size:11.0pt;line-height:137%;color:black'>Fee is too small<o:p></o:p></span></b></h4>

<table class=a2 border=0 cellspacing=0 cellpadding=0 width=972
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:60.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:60.0pt'>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>$ curl
  http://127.0.0.1:12233/json_rpc -s -H 'content-type:application/json;'
  --data-binary
  '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;5&quot;,&quot;method&quot;:&quot;transfer&quot;,
  &quot;params&quot;:{&quot;destinations&quot;:[{&quot;address&quot;:&quot;ZxCb5oL6RTEffiH9gj7w3SYUeQ5s53yUBFGoyGChaqpQdud2uNUaA936Q2ngcEouvmgA48WMZQyv41R2ASstyYHo2Kzeoh7GA&quot;,
  &quot;amount&quot;:100000000}], &quot;fee&quot;:10}}'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:109.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:109.0pt'>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>{<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span
  style='mso-spacerun:yes'> </span>&quot;error&quot;: {<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  
  </span>&quot;code&quot;: -4,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  
  </span>&quot;message&quot;: &quot;transaction was rejected by daemon&quot;<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span
  style='mso-spacerun:yes'> </span>},<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span
  style='mso-spacerun:yes'> </span>&quot;id&quot;: &quot;5&quot;,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span
  style='mso-spacerun:yes'> </span>&quot;jsonrpc&quot;: &quot;2.0&quot;<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>}<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<h4 style='margin-top:12.0pt;margin-right:0cm;margin-bottom:2.0pt;margin-left:
0cm;line-height:137%;mso-pagination:widow-orphan;page-break-after:auto'><a
name="_yd1dsiqxzskw"></a><b style='mso-bidi-font-weight:normal'><span lang=EN
style='font-size:11.0pt;line-height:137%;color:black'>Correct request<o:p></o:p></span></b></h4>

<table class=a3 border=0 cellspacing=0 cellpadding=0 width=972
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:67.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:67.0pt'>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>$ curl
  http://127.0.0.1:12233/json_rpc -s -H 'content-type:application/json;'
  --data-binary
  '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;5&quot;,&quot;method&quot;:&quot;transfer&quot;,
  &quot;params&quot;:{&quot;destinations&quot;:[{&quot;address&quot;:&quot;ZxCb5oL6RTEffiH9gj7w3SYUeQ5s53yUBFGoyGChaqpQdud2uNUaA936Q2ngcEouvmgA48WMZQyv41R2ASstyYHo2Kzeoh7GA&quot;,
  &quot;amount&quot;:1000000000000}], &quot;fee&quot;:1000000000}}'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:96.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:96.0pt'>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>{<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;id&quot;: &quot;5&quot;,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;jsonrpc&quot;: &quot;2.0&quot;,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;result&quot;: {<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-tab-count:1'>       </span>&quot;tx_blob&quot;:
  &quot;00-LONG-HEX-00&quot;,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-tab-count:1'>       </span>&quot;tx_hash&quot;:
  &quot;5412a90afa64faf727946697d70c2989585bbb18c9a232b2c4ac7b7ebd6307aa&quot;,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-tab-count:1'>       </span>&quot;tx_unsigned_hex&quot;:
  &quot;00-LONG-HEX-00&quot;<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>}<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>}<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<h2 style='margin-bottom:4.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_z4npinaq6hs7"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:17.0pt;line-height:137%;font-family:
Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>store<o:p></o:p></span></b></h2>

<p class=normal style='line-height:137%'><span lang=EN>Saves wallet update
progress into a wallet file. Although progress is always saved upon graceful
wallet application termination, with this call a user can manually trigger
saving process. Otherwise, in a case of abnormal wallet application termination
the progress won’t be saved and it will take some time to synchronize on the
next launch.</span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Inputs</span></b><span lang=EN>:</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>None.<o:p></o:p></span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Outputs</span></b><span lang=EN>:</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>None.<o:p></o:p></span></p>

<h3 style='margin-top:14.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_dmiqyuwg93qg"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:13.0pt;line-height:137%;color:black'>Examples<o:p></o:p></span></b></h3>

<table class=a4 border=0 cellspacing=0 cellpadding=0 width=972
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:34.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:34.0pt'>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>$ curl
  http://127.0.0.1:12233/json_rpc -s -H 'content-type:application/json;'
  --data-binary
  '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;3&quot;,&quot;method&quot;:&quot;store&quot;}'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:84.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:84.0pt'>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>{<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span
  style='mso-spacerun:yes'> </span>&quot;id&quot;: &quot;3&quot;,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span
  style='mso-spacerun:yes'> </span>&quot;jsonrpc&quot;: &quot;2.0&quot;,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span
  style='mso-spacerun:yes'> </span>&quot;result&quot;: {<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span
  style='mso-spacerun:yes'> </span>}<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>}<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<h2 style='margin-bottom:4.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_o7haqx42zern"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:17.0pt;line-height:137%;font-family:
Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>get_payments<o:p></o:p></span></b></h2>

<p class=normal style='line-height:137%'><span lang=EN>Gets list of incoming
transfers by a given payment id.</span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Inputs</span></b><span lang=EN>:</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>payment_id</span><span
lang=EN> — string; hex-encoded payment id.</span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Outputs</span></b><span lang=EN>:</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>result</span><span
lang=EN> — list of </span><span lang=EN style='font-family:Consolas;mso-fareast-font-family:
Consolas;mso-bidi-font-family:Consolas'>payments</span><span lang=EN> object.</span></p>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>payments</span><span
lang=EN> object fields:</span></p>

<p class=normal style='margin-left:36.0pt;text-indent:-36.0pt;line-height:137%'><span
lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas'>amount</span><span lang=EN> — unsigned int;
amount of coins in atomic units.</span></p>

<p class=normal style='margin-left:36.0pt;text-indent:-36.0pt;line-height:137%'><span
lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas'>block_height</span><span lang=EN> — unsigned
int; height of the block containing corresponding transaction.</span></p>

<p class=normal style='margin-left:36.0pt;text-indent:-36.0pt;line-height:137%'><span
lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas'>tx_hash</span><span lang=EN> — string;
transaction hash.</span></p>

<p class=normal style='margin-left:36.0pt;text-indent:-36.0pt;line-height:137%'><span
lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas'>unlock_time</span><span lang=EN> — unsigned int;
if non-zero — unix timestamp after which this transfer coins can be spent. If
it is less than 500000000, the value is treated as a minimum block height at
which this transfer coin can be spent.</span></p>

<h3 style='margin-top:14.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_z34njqf1ms3x"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:13.0pt;line-height:137%;color:black'>Examples<o:p></o:p></span></b></h3>

<table class=a5 border=0 cellspacing=0 cellpadding=0 width=972
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:34.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:34.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>$ curl http://127.0.0.1:12233/json_rpc -s -H
  'content-type:application/json;' --data-binary '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;5&quot;,&quot;method&quot;:&quot;get_payments&quot;,
  &quot;params&quot;:{&quot;payment_id&quot;:&quot;aaaa&quot;}'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:159.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:159.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>{<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;id&quot;: &quot;5&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;jsonrpc&quot;: &quot;2.0&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;result&quot;: {<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-tab-count:1'>       </span>&quot;payments&quot;:
  [{<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span><span
  style='mso-tab-count:1'>     </span>&quot;amount&quot;: 100000000000000,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span><span
  style='mso-tab-count:1'>     </span>&quot;block_height&quot;: 131944,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span><span
  style='mso-tab-count:1'>     </span>&quot;payment_id&quot;: &quot;aaaa&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span><span
  style='mso-tab-count:1'>     </span>&quot;tx_hash&quot;: &quot;176416cb542884e10f826627f87df6cf45a16039f913deb2e41f5f2d0647a96d&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span><span
  style='mso-tab-count:1'>     </span>&quot;unlock_time&quot;: 0<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-tab-count:1'>       </span>}]<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span>}<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>}<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<h2 style='margin-bottom:4.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_24fxv5mau6mu"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:17.0pt;line-height:137%;font-family:
Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>get_bulk_payments<o:p></o:p></span></b></h2>

<p class=normal style='line-height:137%'><span lang=EN>Gets list of incoming
transfers by given payment IDs.</span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Inputs</span></b><span lang=EN>:</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>payment_ids</span><span
lang=EN> — array of strings; hex-encoded payment IDs.</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>min_block_height</span><span
lang=EN> — integer; minimum block height.</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Outputs</span></b><span lang=EN>:</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>payments</span><span
lang=EN> — list of </span><span lang=EN style='font-family:Consolas;mso-fareast-font-family:
Consolas;mso-bidi-font-family:Consolas'>payment_details</span><span lang=EN>
object (see <i style='mso-bidi-font-style:normal'>get_payments</i> for
details).</span></p>

<h3 style='margin-top:14.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_lso03dnibjm7"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:13.0pt;line-height:137%;color:black'>Example<o:p></o:p></span></b></h3>

<table class=a6 border=0 cellspacing=0 cellpadding=0 width=972
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:34.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:34.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>$<span style='mso-spacerun:yes'>  </span>curl
  http://127.0.0.1:12233/json_rpc -s -H 'content-type:application/json;'
  --data-binary
  '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;5&quot;,&quot;method&quot;:&quot;get_bulk_payments&quot;,
  &quot;params&quot;:{&quot;payment_ids&quot;:[&quot;aaaa&quot;,
  &quot;bbbb&quot;]}'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:40.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:40.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>{<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;id&quot;: &quot;5&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;jsonrpc&quot;: &quot;2.0&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;result&quot;: {<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-tab-count:1'>       </span>&quot;payments&quot;:
  [{<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span><span
  style='mso-tab-count:1'>     </span>&quot;amount&quot;: 100000000000000,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span><span
  style='mso-tab-count:1'>     </span>&quot;block_height&quot;: 131944,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span><span
  style='mso-tab-count:1'>     </span>&quot;payment_id&quot;: &quot;aaaa&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span><span
  style='mso-tab-count:1'>     </span>&quot;tx_hash&quot;: &quot;176416cb542884e10f826627f87df6cf45a16039f913deb2e41f5f2d0647a96d&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span><span
  style='mso-tab-count:1'>     </span>&quot;unlock_time&quot;: 0<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-tab-count:1'>       </span>}]<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span>}<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>}<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<h2 style='margin-bottom:4.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_nhwzzwymrsuu"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:17.0pt;line-height:137%;font-family:
Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>make_integrated_address<o:p></o:p></span></b></h2>

<p class=normal style='line-height:137%'><span lang=EN>Creates an integrated
address for the wallet by embedding the given payment ID together with the
wallet's public address.</span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Inputs</span></b><span lang=EN>:</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>payment_id</span><span
lang=EN> — string; hex-encoded payment identifier. If empty, random 8-byte
payment ID will be generated and used.</span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Outputs</span></b><span lang=EN>:</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>integrated_address</span><span
lang=EN> — string; the result.</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>payment_id</span><span
lang=EN> — string; hex-encoded payment ID, that was used (useful if an empty </span><span
lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas'>payment_id</span><span lang=EN> was given as an
input).</span></p>

<h3 style='margin-top:14.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_22dsu58sr9ma"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:13.0pt;line-height:137%;color:black'>Examples<o:p></o:p></span></b></h3>

<h4 style='margin-top:12.0pt;margin-right:0cm;margin-bottom:2.0pt;margin-left:
0cm;line-height:137%;mso-pagination:widow-orphan;page-break-after:auto'><a
name="_y5erka8f7wnz"></a><b style='mso-bidi-font-weight:normal'><span lang=EN
style='font-size:11.0pt;line-height:137%;color:black'>Empty payment ID<o:p></o:p></span></b></h4>

<table class=a7 border=0 cellspacing=0 cellpadding=0 width=972
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:34.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:34.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>$ curl http://127.0.0.1:12233/json_rpc -s -H
  'content-type:application/json;' --data-binary
  '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;0&quot;,&quot;method&quot;:&quot;make_integrated_address&quot;,&quot;params&quot;:{&quot;payment_id&quot;:&quot;&quot;}}'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:121.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:121.0pt'>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>{<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;id&quot;: &quot;0&quot;,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;jsonrpc&quot;: &quot;2.0&quot;,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;result&quot;: {<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>   
  </span>&quot;integrated_address&quot;:
  &quot;iZ25D6ReVWYffiH9gj7w3SYUeQ5s53yUBFGoyGChaqpQdud2uNUaA936Q2ngcEouvmgA48WMZQyv41R2ASstyYHoSrxe72RZzxJ1LQ5XnacR&quot;,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>   
  </span>&quot;payment_id&quot;: &quot;717f5ad22cab26f4&quot;<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>}<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'>}<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<h4 style='margin-top:12.0pt;margin-right:0cm;margin-bottom:2.0pt;margin-left:
0cm;line-height:137%;mso-pagination:widow-orphan;page-break-after:auto'><a
name="_s2xpteu23mvj"></a><b style='mso-bidi-font-weight:normal'><span lang=EN
style='font-size:11.0pt;line-height:137%;color:black'>Invalid payment ID<o:p></o:p></span></b></h4>

<table class=a8 border=0 cellspacing=0 cellpadding=0 width=972
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:34.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:34.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>$ curl http://127.0.0.1:12233/json_rpc -s -H
  'content-type:application/json;' --data-binary
  '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;3&quot;,&quot;method&quot;:&quot;make_integrated_address&quot;,&quot;params&quot;:{&quot;payment_id&quot;:&quot;
  !@&amp;#*&quot;}}'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:109.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:109.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>{<br>
  <span style='mso-spacerun:yes'>  </span>&quot;error&quot;: {<br>
  <span style='mso-spacerun:yes'>    </span>&quot;code&quot;: -5,<br>
  <span style='mso-spacerun:yes'>    </span>&quot;message&quot;: &quot;invalid
  payment id given: ' !@&amp;#*', hex-encoded string was expected&quot;<br>
  <span style='mso-spacerun:yes'>  </span>},<br>
  <span style='mso-spacerun:yes'>  </span>&quot;id&quot;: &quot;3&quot;,<br>
  <span style='mso-spacerun:yes'>  </span>&quot;jsonrpc&quot;: &quot;2.0&quot;<br>
  }<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<h4 style='margin-top:12.0pt;margin-right:0cm;margin-bottom:2.0pt;margin-left:
0cm;line-height:137%;mso-pagination:widow-orphan;page-break-after:auto'><a
name="_gl625o71jh7k"></a><b style='mso-bidi-font-weight:normal'><span lang=EN
style='font-size:11.0pt;line-height:137%;color:black'>Correct payment ID<o:p></o:p></span></b></h4>

<table class=a9 border=0 cellspacing=0 cellpadding=0 width=972
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:34.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:34.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>$ curl http://127.0.0.1:12233/json_rpc -s -H
  'content-type:application/json;' --data-binary
  '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;3&quot;,&quot;method&quot;:&quot;make_integrated_address&quot;,&quot;params&quot;:{&quot;payment_id&quot;:&quot;00000000FF00ff00&quot;}}'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:121.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:121.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>{<br>
  <span style='mso-spacerun:yes'>  </span>&quot;id&quot;: &quot;3&quot;,<br>
  <span style='mso-spacerun:yes'>  </span>&quot;jsonrpc&quot;: &quot;2.0&quot;,<br>
  <span style='mso-spacerun:yes'>  </span>&quot;result&quot;: {<br>
  <span style='mso-spacerun:yes'>    </span>&quot;integrated_address&quot;:
  &quot;iZ25D6ReVWYffiH9gj7w3SYUeQ5s53yUBFGoyGChaqpQdud2uNUaA936Q2ngcEouvmgA48WMZQyv41R2ASstyYHoSrwfaxRfrgw3Bz18Df3m&quot;,<br>
  <span style='mso-spacerun:yes'>    </span>&quot;payment_id&quot;:
  &quot;00000000ff00ff00&quot;<br>
  <span style='mso-spacerun:yes'>  </span>}<br>
  }<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<h2 style='margin-bottom:4.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_v4e3lerlc5ei"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:17.0pt;line-height:137%;font-family:
Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>split_integrated_address<o:p></o:p></span></b></h2>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Inputs</span></b><span lang=EN>:</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>integrated_address</span><span
lang=EN> — string; integrated or standard address.</span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Outputs</span></b><span lang=EN>:</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>standard_address</span><span
lang=EN> — string; standard address with no payment ID attached</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>payment_id</span><span
lang=EN> — string; hex-encoded payment ID, extracted from the given integrated
address. Can be empty. Will be empty when a standard address is given as an
input.<b style='mso-bidi-font-weight:normal'><o:p></o:p></b></span></p>

<h3 style='margin-top:14.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_nt6v776slnr7"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:13.0pt;line-height:137%;color:black'>Examples<o:p></o:p></span></b></h3>

<h4 style='margin-top:12.0pt;margin-right:0cm;margin-bottom:2.0pt;margin-left:
0cm;line-height:137%;mso-pagination:widow-orphan;page-break-after:auto'><a
name="_l0o3aur3oqvu"></a><b style='mso-bidi-font-weight:normal'><span lang=EN
style='font-size:11.0pt;line-height:137%;color:black'>Invalid integrated
address<o:p></o:p></span></b></h4>

<table class=aa border=0 cellspacing=0 cellpadding=0 width=972
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:47.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:47.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>$ curl http://127.0.0.1:12233/json_rpc -s -H
  'content-type:application/json;' --data-binary '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;3&quot;,&quot;method&quot;:&quot;split_integrated_address&quot;,&quot;params&quot;:{&quot;integrated_address&quot;:&quot;!k9s02j23n&quot;}}'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:109.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:109.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>{<br>
  <span style='mso-spacerun:yes'>  </span>&quot;error&quot;: {<br>
  <span style='mso-spacerun:yes'>    </span>&quot;code&quot;: -2,<br>
  <span style='mso-spacerun:yes'>    </span>&quot;message&quot;: &quot;invalid
  integrated address given: '!k9s02j23n'&quot;<br>
  <span style='mso-spacerun:yes'>  </span>},<br>
  <span style='mso-spacerun:yes'>  </span>&quot;id&quot;: &quot;3&quot;,<br>
  <span style='mso-spacerun:yes'>  </span>&quot;jsonrpc&quot;: &quot;2.0&quot;<br>
  }<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<h4 style='margin-top:12.0pt;margin-right:0cm;margin-bottom:2.0pt;margin-left:
0cm;line-height:137%;mso-pagination:widow-orphan;page-break-after:auto'><a
name="_xdoyn15mi7ok"></a><b style='mso-bidi-font-weight:normal'><span lang=EN
style='font-size:11.0pt;line-height:137%;color:black'>Valid integrated address<o:p></o:p></span></b></h4>

<table class=ab border=0 cellspacing=0 cellpadding=0 width=972
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:47.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:47.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>$ curl http://127.0.0.1:12233/json_rpc -s -H
  'content-type:application/json;' --data-binary
  '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;3&quot;,&quot;method&quot;:&quot;split_integrated_address&quot;,&quot;params&quot;:{&quot;integrated_address&quot;:&quot;iZ25D6ReVWYffiH9gj7w3SYUeQ5s53yUBFGoyGChaqpQdud2uNUaA936Q2ngcEouvmgA48WMZQyv41R2ASstyYHoSrwfaxRfrgw3Bz18Df3m&quot;}}'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:109.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:109.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>{<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;id&quot;: &quot;3&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;jsonrpc&quot;: &quot;2.0&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;result&quot;: {<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>   
  </span>&quot;payment_id&quot;: &quot;00000000ff00ff00&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>   
  </span>&quot;standard_address&quot;: &quot;ZxCb5oL6RTEffiH9gj7w3SYUeQ5s53yUBFGoyGChaqpQdud2uNUaA936Q2ngcEouvmgA48WMZQyv41R2ASstyYHo2Kzeoh7GA&quot;<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span>}<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>}<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><span lang=EN><br>
<b style='mso-bidi-font-weight:normal'>Valid standard address<o:p></o:p></b></span></p>

<table class=ac border=0 cellspacing=0 cellpadding=0 width=972
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:47.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:47.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>$ curl http://127.0.0.1:12233/json_rpc -s -H
  'content-type:application/json;' --data-binary
  '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;3&quot;,&quot;method&quot;:&quot;split_integrated_address&quot;,&quot;params&quot;:{&quot;integrated_address&quot;:&quot;ZxCb5oL6RTEffiH9gj7w3SYUeQ5s53yUBFGoyGChaqpQdud2uNUaA936Q2ngcEouvmgA48WMZQyv41R2ASstyYHo2Kzeoh7GA&quot;}}'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:109.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:109.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>{<br>
  <span style='mso-spacerun:yes'>  </span>&quot;id&quot;: &quot;3&quot;,<br>
  <span style='mso-spacerun:yes'>  </span>&quot;jsonrpc&quot;: &quot;2.0&quot;,<br>
  <span style='mso-spacerun:yes'>  </span>&quot;result&quot;: {<br>
  <span style='mso-spacerun:yes'>    </span>&quot;payment_id&quot;:
  &quot;&quot;,<br>
  <span style='mso-spacerun:yes'>    </span>&quot;standard_address&quot;:
  &quot;ZxCb5oL6RTEffiH9gj7w3SYUeQ5s53yUBFGoyGChaqpQdud2uNUaA936Q2ngcEouvmgA48WMZQyv41R2ASstyYHo2Kzeoh7GA&quot;<br>
  <span style='mso-spacerun:yes'>  </span>}<br>
  }<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal style='line-height:137%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<h2 style='margin-bottom:4.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_aihbucczq3os"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:17.0pt;line-height:137%;font-family:
Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'><o:p>&nbsp;</o:p></span></b></h2>

<h2 style='margin-bottom:4.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_tglku0vqgevd"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:17.0pt;line-height:137%;font-family:
Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>sign_transfer<o:p></o:p></span></b></h2>

<p class=normal style='line-height:137%'><span lang=EN>Signs a transaction
prepared by watch-only wallet (for cold-signing process).</span></p>

<p class=normal style='line-height:137%'><span lang=EN>NOTE: this method
requires spending private key and can't be served by watch-only wallets.</span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN><o:p>&nbsp;</o:p></span></b></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Inputs</span></b><span lang=EN>:</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>tx_unsigned_hex</span><span
lang=EN> — string; hex-encoded unsigned transaction as returned from <i
style='mso-bidi-font-style:normal'>transfer </i>call.</span></p>

<p class=normal style='line-height:137%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Outputs:<o:p></o:p></span></b></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>tx_hash</span><span
lang=EN> — string; hash identifier of signed transaction.</span><span lang=EN
style='font-family:Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:
Consolas'><o:p></o:p></span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>tx_signed_hex</span><span
lang=EN> — string; hex-encoded signed transaction.</span></p>

<p class=normal style='line-height:137%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN style='font-size:13.0pt;line-height:137%'>Example</span><span lang=EN><o:p></o:p></span></b></p>

<table class=ad border=0 cellspacing=0 cellpadding=0 width=972
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:34.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:34.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>$ curl http://127.0.0.1:12233/json_rpc -s -H
  'content-type:application/json;' --data-binary
  '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;0&quot;,&quot;method&quot;:&quot;sign_transfer&quot;,
  &quot;params&quot;:{<span style='mso-spacerun:yes'> 
  </span>&quot;tx_unsigned_hex&quot; : &quot;00-LONG-HEX-00&quot; }'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:40.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:40.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>{<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;id&quot;: &quot;0&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;jsonrpc&quot;: &quot;2.0&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;result&quot;: {<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>      
  </span>&quot;tx_hash&quot;:
  &quot;855ae466c59b24295152740e84d7f823eaf3c91adfb1ba7b4ff1dc6085b79e63&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-tab-count:1'>       </span>&quot;tx_signed_hex&quot;:
  &quot;00-LONG-HEX-00&quot;<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span>}<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>}<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal style='margin-left:18.0pt;line-height:137%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='margin-left:18.0pt;line-height:137%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<h2 style='margin-bottom:4.0pt;line-height:137%;mso-pagination:widow-orphan;
page-break-after:auto'><a name="_rqh2wls40wwu"></a><b style='mso-bidi-font-weight:
normal'><span lang=EN style='font-size:17.0pt;line-height:137%;font-family:
Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>submit_transfer<o:p></o:p></span></b></h2>

<p class=normal style='line-height:137%'><span lang=EN>Broadcasts transaction
that was previously signed using <i style='mso-bidi-font-style:normal'>sign_transfer</i>
call.</span></p>

<p class=normal style='line-height:137%'><span lang=EN>This method is designed
for using with watch-only wallets that are unable to sign transactions by
themselves.</span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN><o:p>&nbsp;</o:p></span></b></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Inputs</span></b><span lang=EN>:</span></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>tx_signed_hex</span><span
lang=EN> — string; hex-encoded signed transaction as returned from <i
style='mso-bidi-font-style:normal'>sign_transfer </i>call.</span></p>

<p class=normal style='line-height:137%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Outputs:<o:p></o:p></span></b></p>

<p class=normal style='line-height:137%'><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas'>tx_hash</span><span
lang=EN> — string; transaction hash identifier.</span></p>

<p class=normal style='line-height:137%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN style='font-size:13.0pt;line-height:137%'>Examples<o:p></o:p></span></b></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Transaction was rejected for some reason </span></b><span lang=EN>(see
daemon logs for details)</span></p>

<table class=ae border=0 cellspacing=0 cellpadding=0 width=972
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:34.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:34.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>$ curl http://127.0.0.1:12233/json_rpc -s -H
  'content-type:application/json;' --data-binary
  '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;0&quot;,&quot;method&quot;:&quot;submit_transfer&quot;,
  &quot;params&quot;:{<span style='mso-spacerun:yes'> 
  </span>&quot;tx_signed_hex&quot;: &quot;00-LONG-HASH-00&quot;<span
  style='mso-spacerun:yes'>  </span>}'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:40.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:40.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>{<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;error&quot;: {<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-tab-count:1'>       </span>&quot;code&quot;:
  -4,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-tab-count:1'>       </span>&quot;message&quot;:
  &quot;transaction was rejected by daemon&quot;<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span>},<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;id&quot;: &quot;0&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;jsonrpc&quot;: &quot;2.0&quot;<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>}<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><b style='mso-bidi-font-weight:normal'><span
lang=EN>Transaction successfully sent<o:p></o:p></span></b></p>

<table class=af border=0 cellspacing=0 cellpadding=0 width=972
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:34.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:34.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>$ curl http://127.0.0.1:12233/json_rpc -s -H
  'content-type:application/json;' --data-binary
  '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;0&quot;,&quot;method&quot;:&quot;submit_transfer&quot;,
  &quot;params&quot;:{<span style='mso-spacerun:yes'> 
  </span>&quot;tx_signed_hex&quot;: &quot;00-LONG-HASH-00&quot;<span
  style='mso-spacerun:yes'>  </span>}'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:40.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:40.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>{<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;id&quot;: &quot;0&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;jsonrpc&quot;: &quot;2.0&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;result&quot;: {<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-tab-count:1'>       </span>&quot;tx_hash&quot;:
  &quot;0554849abdb62f7d1902ddd14ce005722a340fc14fab4a375adc8749abf4e10b&quot;<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span>}<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>}<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<span lang=EN style='font-size:20.0pt;line-height:115%;font-family:"Arial","sans-serif";
mso-fareast-font-family:Arial;mso-ansi-language:EN;mso-fareast-language:RU;
mso-bidi-language:AR-SA'><br clear=all style='mso-special-character:line-break;
page-break-before:always'>
</span>

<h1><a name="_eh4as68cd98s"></a><span lang=EN><o:p>&nbsp;</o:p></span></h1>

<h1><a name="_y0ft6tx8skfb"></a><span lang=EN>Signing transactions offline
(cold-signing process)</span></h1>

<h2 style='line-height:150%'><a name="_fgxd3ool2n8s"></a><span lang=EN>Introduction</span></h2>

<p class=normal style='line-height:150%'><span lang=EN>In order to provide more
security it's possible to sign transactions offline using a dedicated wallet application
instance e.g. running in a secure environment.</span></p>

<p class=normal style='line-height:150%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:150%'><span style='mso-ansi-language:RU;
mso-no-proof:yes'><!--[if gte vml 1]><v:shapetype id="_x0000_t75" coordsize="21600,21600"
 o:spt="75" o:preferrelative="t" path="m@4@5l@4@11@9@11@9@5xe" filled="f"
 stroked="f">
 <v:stroke joinstyle="miter"/>
 <v:formulas>
  <v:f eqn="if lineDrawn pixelLineWidth 0"/>
  <v:f eqn="sum @0 1 0"/>
  <v:f eqn="sum 0 0 @1"/>
  <v:f eqn="prod @2 1 2"/>
  <v:f eqn="prod @3 21600 pixelWidth"/>
  <v:f eqn="prod @3 21600 pixelHeight"/>
  <v:f eqn="sum @0 0 1"/>
  <v:f eqn="prod @6 1 2"/>
  <v:f eqn="prod @7 21600 pixelWidth"/>
  <v:f eqn="sum @8 21600 0"/>
  <v:f eqn="prod @7 21600 pixelHeight"/>
  <v:f eqn="sum @10 21600 0"/>
 </v:formulas>
 <v:path o:extrusionok="f" gradientshapeok="t" o:connecttype="rect"/>
 <o:lock v:ext="edit" aspectratio="t"/>
</v:shapetype><v:shape id="image1.png" o:spid="_x0000_i1025" type="#_x0000_t75"
 style='width:552pt;height:407pt;visibility:visible;mso-wrap-style:square'>
 <v:imagedata src="ZANO%20wallet%20and%20daemon%20specs%20for%20EXCHANGES%20(DRAFT).files/image001.png"
  o:title=""/>
</v:shape><![endif]--><![if !vml]><img width=736 height=543
src="ZANO%20wallet%20and%20daemon%20specs%20for%20EXCHANGES%20(DRAFT).files/image002.gif"
v:shapes="image1.png"><![endif]></span></p>

<p class=normal style='line-height:137%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><span lang=EN>Zano as a CryptoNote
coin uses two key pairs (4 keys) per wallet: view key (secret+public) and spend
key (secret+public)</span></p>

<p class=normal style='line-height:137%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><span lang=EN>So called &quot;hot
wallet&quot; (or watch-only wallet) uses only <i style='mso-bidi-font-style:
normal'>view secret key</i>. This allows it to distinguish its transactions
among others in the blockchain. To spend coins a wallet needs <i
style='mso-bidi-font-style:normal'>spend secret key</i>. It is required to sign
a tx. Watch-only wallet don't have access to spend secret key and thus it can't
spend coins.</span></p>

<p class=normal style='line-height:137%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><span lang=EN>If someone has your
spend secret key, he can spend your coins. Master keys should be handled with
care.</span></p>

<p class=normal style='line-height:137%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<h2 style='line-height:150%'><a name="_dw04xtt2bznf"></a><span lang=EN>Setup</span></h2>

<p class=normal style='margin-left:18.0pt;text-indent:-18.0pt;mso-text-indent-alt:
-9.0pt;line-height:150%;mso-list:l0 level1 lfo2'><![if !supportLists]><span
lang=EN><span style='mso-list:Ignore'>1.<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></span><![endif]><span lang=EN>In a secure environment create a
new master wallet:</span></p>

<p class=normal style='margin-left:72.0pt;text-indent:-72.0pt;mso-text-indent-alt:
-9.0pt;line-height:150%;mso-list:l0 level2 lfo2'><![if !supportLists]><span
lang=EN><span style='mso-list:Ignore'><span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span>1.1.<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp; </span></span></span><![endif]><span
lang=EN>Start </span><span lang=EN style='font-family:Consolas;mso-fareast-font-family:
Consolas;mso-bidi-font-family:Consolas'>simplewallet</span><span lang=EN> to
generate master wallet:</span><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas;background:#CCCCCC'><br>
simplewallet --generate-new-wallet zano_wallet_master<br>
</span><span lang=EN>(zano_wallet_master is wallet's filename and can be
changed freely)</span></p>

<p class=normal style='margin-left:72.0pt;text-indent:-72.0pt;mso-text-indent-alt:
-9.0pt;line-height:150%;mso-list:l0 level2 lfo2'><![if !supportLists]><span
lang=EN><span style='mso-list:Ignore'><span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span>1.2.<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp; </span></span></span><![endif]><span
lang=EN>Type in a password when asked.</span></p>

<p class=normal style='margin-left:72.0pt;text-indent:-72.0pt;mso-text-indent-alt:
-9.0pt;line-height:150%;mso-list:l0 level2 lfo2'><![if !supportLists]><span
lang=EN><span style='mso-list:Ignore'><span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span>1.3.<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp; </span></span></span><![endif]><span
lang=EN>Type the following command into wallet's console:<br>
</span><span lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas;background:#CCCCCC'>save_watch_only
zano_wallet_watch_only.keys WATCH_PASSWORD<br>
</span><span lang=EN>where </span><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas;background:#CCCCCC'>WATCH_PASSWORD</span><span
lang=EN> is password for a watch-only wallet.<br>
You should see:<br>
</span><span lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas;background:#CCCCCC'>Keys stored to
zano_wallet_watch_only.keys</span></p>

<p class=normal style='margin-left:72.0pt;text-indent:-72.0pt;mso-text-indent-alt:
-9.0pt;line-height:150%;mso-list:l0 level2 lfo2'><![if !supportLists]><span
lang=EN><span style='mso-list:Ignore'><span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span>1.4.<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp; </span></span></span><![endif]><span
lang=EN>Type </span><span lang=EN style='font-family:Consolas;mso-fareast-font-family:
Consolas;mso-bidi-font-family:Consolas;background:#CCCCCC'>exit</span><span
lang=EN> to quit simplewallet.</span></p>

<p class=normal style='margin-left:13.5pt;text-indent:-13.5pt;mso-text-indent-alt:
-9.0pt;line-height:150%;mso-list:l0 level1 lfo2'><![if !supportLists]><span
lang=EN><span style='mso-list:Ignore'>2.<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></span><![endif]><span lang=EN>Copy </span><span lang=EN
style='font-family:Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:
Consolas'>zano_wallet_watch_only.keys</span><span lang=EN> file from secure
environment to your production environment where daemons and hot wallet are
supposed to be run.<br>
<span style='background:#F4CCCC'>NOTE: </span></span><span lang=EN
style='font-family:Consolas;mso-fareast-font-family:Consolas;mso-bidi-font-family:
Consolas;background:#F4CCCC'>zano_wallet_master.keys</span><span lang=EN
style='background:#F4CCCC'> file contains master wallet private keys! You may
want it to never leave secure environment.</span></p>

<p class=normal style='margin-left:13.5pt;text-indent:-13.5pt;mso-text-indent-alt:
-9.0pt;line-height:150%;mso-list:l0 level1 lfo2'><![if !supportLists]><span
lang=EN><span style='mso-list:Ignore'>3.<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></span><![endif]><span lang=EN>In production environment start
the daemon (let it perform initial sync if running for the first time and make
sure it is synchronized), then start the watch-only wallet:<br>
</span><span lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas;background:#CCCCCC'>simplewallet --wallet-file
zano_wallet_watch_only.keys --password WATCH_PASSWORD --rpc-bind-ip RPC_IP
--rpc-bind-port RPC_PORT --daemon-address DEAMON_ADDR:DAEMON_PORT --log-file
LOG_FILE_NAME<br>
</span><span lang=EN>(see also the Introduction; for the first run you can add </span><span
lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas;background:#CCCCCC'>--log-level=0</span><span
lang=EN> to avoid too verbose messages, for subsequent runs you can use </span><span
lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas;background:#CCCCCC'>--log-level=1</span><span
lang=EN> or </span><span lang=EN style='font-family:Consolas;mso-fareast-font-family:
Consolas;mso-bidi-font-family:Consolas;background:#CCCCCC'>--log-level=2</span><span
lang=EN>)</span></p>

<p class=normal style='line-height:150%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:150%'><span lang=EN>Setup is complete.</span></p>

<h2 style='line-height:150%'><a name="_4429kq9jgceg"></a><span lang=EN>Example
of a transaction cold-signing</span></h2>

<p class=normal style='line-height:150%'><span lang=EN>In order to sign a
transaction, follow these steps:</span></p>

<p class=normal style='line-height:150%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='margin-left:18.0pt;text-indent:-18.0pt;mso-text-indent-alt:
-9.0pt;line-height:150%;mso-list:l2 level1 lfo1'><![if !supportLists]><span
lang=EN><span style='mso-list:Ignore'>4.<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></span><![endif]><span lang=EN>Using RPC <i style='mso-bidi-font-style:
normal'>transfer </i>create a transaction.<br>
Because of using watch-only wallet keys for this instance of wallet application
(please note passing </span><span lang=EN style='font-family:Consolas;
mso-fareast-font-family:Consolas;mso-bidi-font-family:Consolas;background:#CCCCCC'>zano_wallet_watch_only.keys</span><span
lang=EN> in i.3) a transaction will not be signed and broadcasted. Instead,
unsigned transaction will be prepared and returned via RPC.<br
style='mso-special-character:line-break'>
<![if !supportLineBreakNewLine]><br style='mso-special-character:line-break'>
<![endif]></span></p>

<p class=normal style='line-height:150%'><span lang=EN>RPC example (please, see
also <i style='mso-bidi-font-style:normal'>transfer</i> RPC description in
&quot;List of RPC calls&quot; section above):<b style='mso-bidi-font-weight:
normal'><o:p></o:p></b></span></p>

<table class=af0 border=0 cellspacing=0 cellpadding=0 width=972
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:34.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:34.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>$ curl http://127.0.0.1:12233/json_rpc -s -H
  'content-type:application/json;' --data-binary
  '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;0&quot;,&quot;method&quot;:&quot;transfer&quot;,
  &quot;params&quot;:{<span style='mso-spacerun:yes'>  
  </span>&quot;destinations&quot;:[{&quot;amount&quot;:1000000000000,
  &quot;address&quot;:&quot;ZxCb5oL6RTEffiH9gj7w3SYUeQ5s53yUBFGoyGChaqpQdud2uNUaA936Q2ngcEouvmgA48WMZQyv41R2ASstyYHo2Kzeoh7GA&quot;}],
  &quot;fee&quot;:1000000000, &quot;mixin&quot;:0,
  &quot;unlock_time&quot;:0<span style='mso-spacerun:yes'>   </span>}}'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:40.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:40.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>{<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;id&quot;: &quot;0&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;jsonrpc&quot;: &quot;2.0&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;result&quot;: {<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-tab-count:1'>       </span>&quot;tx_blob&quot;:
  &quot;&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-tab-count:1'>       </span>&quot;tx_hash&quot;:
  &quot;c41589e7559804ea4a2080dad19d876a024ccb05117835447d72ce08c1d020ec&quot;,<o:p></o:p></span></p>
  <p class=normal style='margin-right:3.0pt;line-height:137%'><span lang=EN
  style='font-size:9.0pt;line-height:137%;font-family:Consolas;mso-fareast-font-family:
  Consolas;mso-bidi-font-family:Consolas'><span style='mso-tab-count:1'>       </span>&quot;tx_unsigned_hex&quot;:
  &quot;00-LONG-HEX-00&quot;<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span>}<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>}<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal style='line-height:150%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:150%'><span lang=EN>Unsigned transaction
data retrieved in <i style='mso-bidi-font-style:normal'>tx_unsigned_hex</i>
field should be passed to secure environment for cold-signing by master wallet.<br
style='mso-special-character:line-break'>
<![if !supportLineBreakNewLine]><br style='mso-special-character:line-break'>
<![endif]></span></p>

<p class=normal style='margin-left:18.0pt;text-indent:-18.0pt;line-height:150%;
mso-list:l1 level1 lfo3'><![if !supportLists]><span lang=EN><span
style='mso-list:Ignore'>5.<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></span><![endif]><span lang=EN>Run master wallet within secure
environment:<br>
</span><span lang=EN style='font-family:Consolas;mso-fareast-font-family:Consolas;
mso-bidi-font-family:Consolas;background:#CCCCCC'>simplewallet --wallet-file
zano_wallet_master --password MASTER_PASSWORD --offline-mode</span></p>

<p class=normal style='margin-left:18.0pt;text-indent:-18.0pt;line-height:150%;
mso-list:l1 level1 lfo3'><![if !supportLists]><span lang=EN><span
style='mso-list:Ignore'>6.<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></span><![endif]><span lang=EN>Using RPC <i style='mso-bidi-font-style:
normal'>sing_transfer</i> sing the transaction using master wallet.</span></p>

<p class=normal style='line-height:150%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:150%'><span lang=EN>RPC example:<b
style='mso-bidi-font-weight:normal'><o:p></o:p></b></span></p>

<table class=af1 border=0 cellspacing=0 cellpadding=0 width=972
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:34.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:34.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>$ curl http://127.0.0.1:12233/json_rpc -s -H
  'content-type:application/json;' --data-binary
  '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;0&quot;,&quot;method&quot;:&quot;sign_transfer&quot;,
  &quot;params&quot;:{<span style='mso-spacerun:yes'> 
  </span>&quot;tx_unsigned_hex&quot; : &quot;00-LONG-HEX-00&quot; }'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:40.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:40.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>{<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;id&quot;: &quot;0&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;jsonrpc&quot;: &quot;2.0&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;result&quot;: {<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-tab-count:1'>       </span>&quot;tx_signed_hex&quot;:
  &quot;00-LONG-HEX-00&quot;<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span>}<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>}<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal style='margin-left:18.0pt;line-height:137%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:150%'><span lang=EN>Signed transaction data
retrieved in <i style='mso-bidi-font-style:normal'>tx_signed_hex</i> field
should be passed to the production environment along with the corresponding <i
style='mso-bidi-font-style:normal'>tx_unsigned_hex</i> data to be broadcasted
by watch-only wallet.</span></p>

<p class=normal style='line-height:150%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='margin-left:18.0pt;text-indent:-18.0pt;line-height:150%;
mso-list:l1 level1 lfo3'><![if !supportLists]><span lang=EN><span
style='mso-list:Ignore'>7.<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></span><![endif]><span lang=EN>Using RPC <i style='mso-bidi-font-style:
normal'>submit_transfer</i> broadcast the transaction using watch-only wallet.</span></p>

<p class=normal style='line-height:150%'><span lang=EN>RPC example:<b
style='mso-bidi-font-weight:normal'><o:p></o:p></b></span></p>

<table class=af2 border=0 cellspacing=0 cellpadding=0 width=972
 style='border-collapse:collapse;mso-table-layout-alt:fixed;mso-yfti-tbllook:
 1536;mso-padding-alt:5.0pt 5.0pt 5.0pt 5.0pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes;height:34.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#D9EAD3;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:34.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>$ curl http://127.0.0.1:12233/json_rpc -s -H
  'content-type:application/json;' --data-binary
  '{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;id&quot;:&quot;0&quot;,&quot;method&quot;:&quot;submit_transfer&quot;,
  &quot;params&quot;:{<span style='mso-spacerun:yes'> 
  </span>&quot;tx_unsigned_hex&quot;: &quot;00-LONG-HASH-00&quot;,
  &quot;tx_signed_hex&quot;: &quot;00-LONG-HASH-00&quot;<span
  style='mso-spacerun:yes'>  </span>}'<o:p></o:p></span></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1;mso-yfti-lastrow:yes;height:40.0pt'>
  <td width=972 valign=top style='width:485.8pt;background:#CCCCCC;padding:
  5.0pt 5.0pt 5.0pt 5.0pt;height:40.0pt'>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>{<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;id&quot;: &quot;0&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;jsonrpc&quot;: &quot;2.0&quot;,<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'> 
  </span>&quot;result&quot;: {<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-tab-count:1'>       </span>&quot;tx_hash&quot;:
  &quot;0554849abdb62f7d1902ddd14ce005722a340fc14fab4a375adc8749abf4e10b&quot;<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'><span style='mso-spacerun:yes'>  </span>}<o:p></o:p></span></p>
  <p class=normal style='line-height:137%'><span lang=EN style='font-size:9.0pt;
  line-height:137%;font-family:Consolas;mso-fareast-font-family:Consolas;
  mso-bidi-font-family:Consolas'>}<o:p></o:p></span></p>
  </td>
 </tr>
</table>

<p class=normal><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal><span lang=EN>The transaction was successfully broadcasted over
the network.</span></p>

<p class=normal style='line-height:137%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:137%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<h2 style='line-height:137%'><a name="_birzfkm66qku"></a><span lang=EN>Important
note on watch-only wallets</span></h2>

<p class=normal style='line-height:150%'><span lang=EN>Watch-only wallet is not
able naturally to calculate a balance using only a tracking view secret key and
an access to the blockchain. This happens because it can't distinguish spending
its own coins as it requires knowing key images for own coins, which are
unknown, as key image calculation requires spend secret key.</span></p>

<p class=normal style='line-height:150%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:150%'><span lang=EN>To workaround this
difficulty watch-only wallet extracts and stores key images for own coins each
time a signed transaction from a cold wallet is broadcasted using <i
style='mso-bidi-font-style:normal'>submit_transfer</i> RPC. This data is stored
locally and it is required to calculate wallet's balance in case of full wallet
resync.</span></p>

<p class=normal style='line-height:150%'><span lang=EN><o:p>&nbsp;</o:p></span></p>

<p class=normal style='line-height:150%'><span lang=EN>It's important to keep
this data safe and not to delete watch-only wallet's files. Otherwise,
watch-only wallet won't be able to calculate a balance correctly and cold
wallet may be required to be connected online for recovering funds.</span></p>

</div>

</body>

</html>
