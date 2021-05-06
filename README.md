# RSK_debug_traceTransaction

A Step By Step Guide on How to Debug Transactions on RSK Network

## What is a Transaction Debugging?

When a transaction in an EVM-compatible network fails, all the possible events which warns us about what happened in the internals steps of its execution get lost.

When this happens the smart contract transactions show internal errors in several block explorers, but the message related to the failed require condition doesn't appear in those error messages. Unless we run our own full node in that network, we only can see "internal error" or "reverted" in our transaction and we'll be unable to see which specific error occurred.

## Sovryn Platform Runs Its Own Full Nodes

Sovryn platform which base its code on the RSK network (a EVM-compatible network) runs its own nodes, and are publicly accessible. These nodes were configured according the specified in the stackoverflow discussion that we can see [here](https://cutt.ly/5bRuIsT). This way, these nodes are enabled to accept and execute debugging orders.

The feature enabled in these Sovryn nodes is called _*debug_traceTransaction*_, and with this tool any user which founds a failed transaction, whether in the RSK mainnet or testnet can make a query to these nodes and find out what prevented the transaction from succeed.

When it happens, all the user needs is to copy the failed transaction id (e.g._0x0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef_).

Then in a terminal or console native of his operative system (and supposing the [CURL](https://curl.se/download.html) package is installed in his system) he can run the following script in order to retrieve all the data related to the failed transaction:

$curl \
 -X POST \
 -H "Content-Type:application/json" \
 --data '{"jsonrpc":"2.0","method":"debug_traceTransaction","params":["0x0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef"],"id":1}' \
 https://mainnet2.sovryn.app/rpc

(ther last value can also consit of "testnet2,sovryn.app"; "mainnet.sovryn.app" or "testnet.sovryn.app")

## A Query to a Full Node to Debug a Transaction May Return a Very large File

Frequently it is better to ask the terminal in the very script to save the serialized response into a file.

This can be done adding to the script the destine file:

$curl \
 -X POST \
 -H "Content-Type:application/json" \
 --data '{"jsonrpc":"2.0","method":"debug_traceTransaction","params":["0x0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef"],"id":1}' \
 https://mainnet2.sovryn.app/rpc **&> Ouput.json**

## How To Make It Easy

The former script can be embedded in a shell file that can be executed in Windows and Linux environments.

The credits for the coders, who helped to develop the scripts in the file TXDebug.sh, are indicated inside of it.

The steps to debug a transaction are as follow:

1. Copy the transaction Id which failed. (e.g._0x0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef_).
2. From the file explorer of your O.S. simply execute (or double click on) the file TXDebug.sh
3. The script prompts the user to paste or write the transaction id. Please paste it there, and hit _Enter_.
4. Then the script asks the user in which network (and with which node) to perform the debugging. There are 4 options to choose, one for each node, and there are two nodes for each net: mainnet or testnet in RSK. Hit the chosen number (it must lie between 1 to 4).
5. Immediately the script will make the RPC call to the node, and start to listen the answer, injecting the serialized data as it is received in the file named _output.json_
6. If the returned file is too large it will be preferable to use a [special text editor](https://drive.google.com/file/d/1u3Z__VnRG8XlYP8yw1yUmxcBIPATS5gc/view?usp=sharing). Read the instructions on _BssEditor.txt_.
7. Then we need to look for the following response structure:

{
...
"result": "08c379a00000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000001e536166654d6174683a207375627472616374696f6e206f766572666c6f770000",
"error": "",
"reverted": true,
...
}

8. We can use a translator from the hexadecimal string (after "result") into ascii text like [this one](https://www.rapidtables.com/convert/number/hex-to-ascii.html).

9. In the example case the hex string means:

   _ Ãy SafeMath: subtraction overflow_

## You Can Easily Edit The Script

Editing the code of the script in TXDebug.sh you can:

a. Add more URL of servers (full operative nodes).
b. Change the output file
c. Any other feature you may think you can improve

**Thank you very much [Salvador Fuentes](https://github.com/DestroyerDarkNess)**
