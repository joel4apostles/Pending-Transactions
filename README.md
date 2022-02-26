# Pending-Transactions
EVM compiatble Mempool Pending Transaction

On Ethereum and EVM compitatble protocol, when a transaction is sent, before being added to a block, it resides in Mempool, a mempool or TxPool is a waiting area for the transactions that have not been added to a block and are still unconfirmed. To receive information about this transaction, the Mempool must be queried. When Etherum of any EVM blockchain receives a transaction, the transaction needs to be propagated to a peer node until a miner picks up the transaction and added it to a block, however, before the transaction is added to the next block, the transaction remains in the waiting pool known as mempool, pending transaction or txpool. 


In order to query a Geth node's Mempool, TX-POOL or Transaction Pool: Make a cURL request to the transaction pool/mempool of your node using below cURL request.

$ curl --data '{"method":"txpool_content","id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST ADD_YOUR_ETHEREUM_NODE_endpoint (HTTPS/WSS)_URL

Replace "ADD_YOUR_ETHEREUM_NODE_endpoint (HTTPS/WSS)_URL" with with the HTTP provider 

This above line of code will make a cURL JSON-RPC request to your Geth node's transaction pool/mempool by using the `txpool_content` method, which will return the pending transactions and those queued for future execution. The output stream will have two fields: pending and queued.

Querying with Web3.js library to connect and subscribe to pending transactions using web3.js
Let’s examine how to subscribe to pending transactions from the mempool using Web3.js. 

Prerequisites
NodeJS installed on your system.
A text editor
CLI

Step 1.
Install web3.js by executing the following command in your terminal or command line: $ npm install web3

Make sure you have node.js installed and running on our system by checking the version of node installed with this command line: $ node -v
If web3 is installed the installed version will display on your terminal or command line.

Step 2.
Now, let create a text script file and call it index.js, copy this script to your text editor and save

var Web3 = require("web3");
var url = "ADD_YOUR_ETHEREUM_NODE_WSS_URL";

var options = {
  timeout: 30000,
  clientConfig: {
    maxReceivedFrameSize: 100000000,
    maxReceivedMessageSize: 100000000,
  },
  reconnect: {
    auto: true,
    delay: 5000,
    maxAttempts: 15,
    onTimeout: false,
  },
};

var web3 = new Web3(new Web3.providers.WebsocketProvider(url, options));
const subscription = web3.eth.subscribe("pendingTransactions", (err, res) => {
  if (err) console.error(err);
});

var init = function () {
  subscription.on("data", (txHash) => {
    setTimeout(async () => {
      try {
        let tx = await web3.eth.getTransaction(txHash);
        console.log(tx)
      } catch (err) {
        console.error(err);
      }
    });
  });
};

init();


Explanation of the code above:

Line 32-33: Importing the web3 library, and adding HTTPS/WSS)_URL" with with the HTTP provider's wss URL to the variable url.

Line 35-47: Adding a reconnect logic, so that if the connection is dropped for some reason the script will reattempt to establish a connection.

Line 49-52: Instantiating a WebSocketProvider instance, and assigning it to the web3 variable, subscribing to pending transactions. 

Line 54-65: Getting the transaction body by hash, and printing the transactions. 

Step3.
When we run this file with this command on your terminal $ node index.js, it should output a stream of pending transactions currently in the mempool.
