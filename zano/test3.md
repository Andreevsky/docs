# Introduction
## Starting the daemon and the wallet application as RPC server
Zano command-line wallet application (simplewallet) can be run in RPC server mode. In this mode it can be controlled by RPC calls via HTTP and be used as a back-end for an arbitrary service.

Starting the wallet in RPC server mode:
1. Run zanod (the daemon application).
2. Run simplewallet with the following options:

`simplewallet --wallet-file PATH_TO_WALLET_FILE --password PASSWORD --rpc-bind-ip RPC_IP --rpc-bind-port RPC_PORT --daemon-address DEAMON_ADDR:DAEMON_PORT --log-file LOG_FILE_NAME`

where:

`PATH_TO_WALLET_FILE` — path to an existing wallet file (should be created beforehand using  `--generate-new-wallet command`);
`PASSWORD` — wallet password;
`RPC_IP` — IP address to bind RPC server to (127.0.0.1 will be used if not specified);
`RPC_PORT` — TCP port for RPC server;
`DEAMON_ADDR:DAEMON_PORT` — daemon address and port (may be omitted if the daemon is running on the same machine with the default settings);
`LOG_FILE_NAME` — path and filename of simplewallet log file.

Examples below are given with assumption that the wallet application is running in RPC server mode and listening at 127.0.0.1:12233.

All amounts and balances are represented as unsigned integers and measured in atomic units — the smallest fraction of a coin. One coin equals 10^12 atomic units.

## Integrated addresses for exchanges
Unlike Bitcoin, CryptoNote family coins have different, more effective approach on how to handle user deposits.
An exchange generates only one address for receiving coins and all users send coins to that address. To distinguish different deposits from different users the exchange generates random identifier (called <strong>payment ID</strong>) for each one and a user attaches this payment ID to his transaction while sending. Upon receiving, the exchange can extract payment ID and thus identify the user.
In original CryptoNote there were two separate things: exchange deposit address (the same for all users) and payment ID (unique for all users). Later, for user convenience and to avoid missing payment ID we combined them together into one thing, called <strong>integrated address</strong>. So nowadays modern exchanges usually give to a user an integrated address for depositing instead of pair of standard deposit address and a payment ID.

For more information on how to handle integrated addresses, please refer to RPCs make_integrated_address and split_integrated_address below.

# List of Wallet RPCs

## getbalance
Retrieves current wallet balance: total and unlocked.
### Inputs:
None.
### Outputs:
balance — unsigned integer; total amount of funds the wallet has (unlocked and locked coins).
unlocked_balance — unsigned integer; unlocked funds, i.e. coins that are stored deep enough in the blockchain to be considered relatively safe to spend. Only this amount of coins are immediately spendable.
unlocked_balance is always less or equal to balance.
### Examples
`$ curl http://127.0.0.1:12233/json_rpc -s -H 'content-type:application/json;' --data-binary '{"jsonrpc":"2.0","id":"0","method":"getbalance"}'`

    {
     "id": "0",
     "jsonrpc": "2.0",
     "result": {
      "balance": 160810073397000000,
      "unlocked_balance": 160810073397000000
     }
    }

## getaddress
Obtains wallet public address.
### Inputs:
None.
### Outputs:
address — string; standard public address of the wallet.
### Examples
`$ curl http://127.0.0.1:12233/json_rpc -s -H 'content-type:application/json;' --data-binary '{"jsonrpc":"2.0","id":"0","method":"getaddress"}'`

    {
     "id": "0",
     "jsonrpc": "2.0",
     "result": {
       "address": "ZxCb5oL6RTEffiH9gj7w3SYUeQ5s53yUBFGoyGChaqpQdud2uNUaA936Q2ngcEouvmgA48WMZQyv41R2ASstyYHo2Kzeoh7GA"
     }
    }

## transfer
Creates a transaction and broadcasts it to the network.
### Inputs:
destinations — list of transfer_destination objects (see below); list of recipients with corresponding amount of coins for each.
fee — unsigned int; transaction fee in atomic units. Minimum 105 atomic units, recommended 106 or 107.
mixin — unsigned int; number of foreign outputs to be mixed in with each input. Increases untraceability. Use 0 for direct and traceable transfers.
payment_id — string; hex-encoded payment id. Can be empty if payment id is not required for this transfer.

transfer_destination object fields:
address — string; standard or integrated address of a recipient.
amount — unsigned int; amount of coins to be sent;

### Outputs:
tx_hash — string; hash identifier of the transaction that was successfully sent.
tx_unsigned_hex — string; hex-encoded unsigned transaction (for watch-only wallets; to be used in cold-signing process).
### Examples
#### Not enough money
`$ curl http://127.0.0.1:12233/json_rpc -s -H 'content-type:application/json;' --data-binary '{"jsonrpc":"2.0","id":"5","method":"transfer", "params":{"destinations":[{"address":"ZxCb5oL6RTEffiH9gj7w3SYUeQ5s53yUBFGoyGChaqpQdud2uNUaA936Q2ngcEouvmgA48WMZQyv41R2ASstyYHo2Kzeoh7GA", "amount":10000000000000000000}]}}'`

    {
      "error": {
    	"code": -4,
    	"message": "not enough money"
      },
      "id": "5",
      "jsonrpc": "2.0"
    }

#### Fee is too small
`$ curl http://127.0.0.1:12233/json_rpc -s -H 'content-type:application/json;' --data-binary '{"jsonrpc":"2.0","id":"5","method":"transfer", "params":{"destinations":[{"address":"ZxCb5oL6RTEffiH9gj7w3SYUeQ5s53yUBFGoyGChaqpQdud2uNUaA936Q2ngcEouvmgA48WMZQyv41R2ASstyYHo2Kzeoh7GA", "amount":100000000}], "fee":10}}'`

    {
     "error": {
       "code": -4,
       "message": "transaction was rejected by daemon"
     },
     "id": "5",
     "jsonrpc": "2.0"
    }

#### Correct request
`$ curl http://127.0.0.1:12233/json_rpc -s -H 'content-type:application/json;' --data-binary '{"jsonrpc":"2.0","id":"5","method":"transfer", "params":{"destinations":[{"address":"ZxCb5oL6RTEffiH9gj7w3SYUeQ5s53yUBFGoyGChaqpQdud2uNUaA936Q2ngcEouvmgA48WMZQyv41R2ASstyYHo2Kzeoh7GA", "amount":1000000000000}], "fee":1000000000}}'`

    {
      "id": "5",
      "jsonrpc": "2.0",
      "result": {
    	"tx_blob": "00-LONG-HEX-00",
    	"tx_hash": "5412a90afa64faf727946697d70c2989585bbb18c9a232b2c4ac7b7ebd6307aa",
    	"tx_unsigned_hex": "00-LONG-HEX-00"
      }
    }

## store
Saves wallet update progress into a wallet file. Although progress is always saved upon graceful wallet application termination, with this call a user can manually trigger saving process. Otherwise, in a case of abnormal wallet application termination the progress won’t be saved and it will take some time to synchronize on the next launch.
### Inputs:
None.
### Outputs:
None.
### Examples
`
$ curl http://127.0.0.1:12233/json_rpc -s -H 'content-type:application/json;' --data-binary '{"jsonrpc":"2.0","id":"3","method":"store"}'
`

     {
     "id": "3",
     "jsonrpc": "2.0",
     "result": {
     }
    }

## get_payments
Gets list of incoming transfers by a given payment id.
### Inputs:
payment_id — string; hex-encoded payment id.
### Outputs:
result — list of payments object.

payments object fields:
amount — unsigned int; amount of coins in atomic units.
block_height — unsigned int; height of the block containing corresponding transaction.
tx_hash — string; transaction hash.
unlock_time — unsigned int; if non-zero — unix timestamp after which this transfer coins can be spent. If it is less than 500000000, the value is treated as a minimum block height at which this transfer coin can be spent.
### Examples
`
$ curl http://127.0.0.1:12233/json_rpc -s -H 'content-type:application/json;' --data-binary '{"jsonrpc":"2.0","id":"5","method":"get_payments", "params":{"payment_id":"aaaa"}'
`

    {
      "id": "5",
      "jsonrpc": "2.0",
      "result": {
    	"payments": [{
      	"amount": 100000000000000,
      	"block_height": 131944,
      	"payment_id": "aaaa",
      	"tx_hash": "176416cb542884e10f826627f87df6cf45a16039f913deb2e41f5f2d0647a96d",
      	"unlock_time": 0
    	}]
      }
    }
