---
title: How to connect to your own Bitcoin Node using RPC Calls and Python
layout: post
description: Quick tutorial on How to access the Bitcoin Node using RPC Calls and Python
categories: [Bitcoin, RPC]
tags: [informative, technology, bitcoin, RPC, raspberry pi 4]
image:
  path: /assets/posts/images/bitcoinledger.png
  alt: Bitcoin Blockchain
---

In the following post, let's talk a little bit about **RPC** **calls** and how we can use them to access our *Bitcoin* *Node*.

## What is a RPC?

**Remote Procedure Call**, or **RPC**, is a protocol used by *applications* to request a service from **another** application running on a *remote* *computer*. 

The concept of **RPC** has been around for decades, but it is still widely used in modern **software development**. 

**RPC** **calls** allow different components of an application to communicate with each other, which is *essential* for the smooth functioning of the **application**. 
In this blog post, we'll explore *how to connect to our own **bitcoin** database using **RPC** **Calls***.

> Please ensure that you have the **bitcoin core** already *downloaded* and *updated* so that it won't cause any *constraints* and that we are able to send **requests**. For more information, check out the blog post [How to set up your Bitcoin Node on a Raspberry Pi 4 and SSH enable](/posts/How-to-get-Bitcoin-Ledger-in-Raspberry-Pi-4-with-Umbrel-and-SSH-enable/).
{: .prompt-warning }

First, we need to *prepare/install* the *libraries* that we are going to use. For that, please connect to your terminal on the *Raspberry Pi 4* using  **SSH** or **[Bitwise](https://www.bitvise.com/ssh-client-download)**.

### Steps to prepare RPC Calls:
1. We need to install the **Python** *Wrapper* for **Bitcoin RPC**. Please run:

    > pip install python-bitcoinrpc

    > Please execute the previous command on your window terminal.
    {: .prompt-tip }

2. You should update your *bitcoin.conf* file. You can find it under your *Umbrel*:

    > home/umbrel/umbrel/app-data/bitcoin/data/bitcoin/bitcoin.conf

    Example:
    ```python
    # Load additional configuration file, relative to the data directory.
    includeconf=umbrel-bitcoin.conf #File connected to this one, produced by Umbrel
    rpcuser=<someusername>          #Username for JSON-RPC connections
    rpcpassword=<somepassword>      #Password for JSON-RPC connections 
    server=1                        #Accept command line and JSON-RPC commands 
    txindex=1                       #Maintain a full transaction index, used by the getrawtransaction rpc call
    rpcallowip=192.168.1.0/24       #Allow JSON-RPC connections from specified source. Valid for <ip> are a single IP
    rpcallowip=127.0.0.1            #Allow JSON-RPC connections from specified source. Valid for <ip> are a single IP
    ```
3. After making changes, please **save** the file and **restart** your *Raspberry Pi*.

    ```bash
    ~/umbrel/scripts/app restart bitcoin
    ```

    > You can also use the restart option under the *Umbrel* definitons menu. You can corrupt the ledger that you downloaded if you force a restart/shutdown manually.
    {: .prompt-warning }

4. Create a *python* file called "*config.py*" in a folder called *Config*.

    > This is the file that your **Python** **Script** is going to use to connect to the *database*, so please **don't share it**!
    {: .prompt-tip }

5. The *config.py* file should have the following information (please ensure that *rpcuser* and *rpcpassword* are the same as the ones in **bitcoin.conf** from **step 2**):

    ```python
    # Load additional configuration file for bitcoin node
    rpcuser = "<someusername>"
    rpcpassword = "<somepassword>"
    host = "127.0.0.1"
    port = "8332"
    port_test = "18332"
    timeout= 200
    ```

## Coding the Main Script
Let's *start the main script* part by importing the *modules*.

```python
from bitcoinrpc.authproxy import AuthServiceProxy, JSONRPCException
from Config.config import rpcuser, rpcpassword, host, port, timeout
import time
```
### Import of the Libraries
This means that for the line number:
1. Calling the **library** that handles the call to the *RPC*
2. We need to retrieve the *credentials* and *other* *information* about our **node** (*remember, we save those under a folder called Config*).
3. Imported module *time*, this is just to **add** the feature for the *script* to *wait x seconds*.

Right now, we can start making *calls* through **RPC**.

### Definition of the functions

We need to **create** *functions*; those are *crucial* because those same *functions* are going to be called **several** **times**!

```python
def getblockhash(block_height):
    rpc_connection = AuthServiceProxy("http://%s:%s@%s:%s"%(rpcuser, rpcpassword, host, port ), timeout=timeout)
    try:
        block_hash= rpc_connection.getblockhash(block_height)
    except:
        print("Got error waiting 60 seconds on getblockhash")
        time.sleep(60)
        rpc_connection = AuthServiceProxy("http://%s:%s@%s:%s"%(rpcuser, rpcpassword, host, port ), timeout=timeout)
        block_hash= rpc_connection.getblockhash(block_height)
    return block_hash
```
### Function GetBlockHash

It seems *confusing*, but let's *break* each line down. The main goal here is to take directly from the **bitcoin blockchain** the *Hash* of a specific given **Block** **Height** (ex: *10*, *234*, *257321*).
```python
def getblockhash(block_height):
```
Function *Declaration*, Our *function* is going to call *getblockhash* and will receive one *single parameter*, which is going to be the **number of the block**.
```python
rpc_connection = AuthServiceProxy("http://%s:%s@%s:%s"%(rpcuser, rpcpassword, host,port ), timeout=timeout)
```
The previous line is one of the **most** **import**; it's how we construct our *call* to the *RPC* and save the result in a *variable*. 
The *AuthServiceProxy* needs the string or link where we should make a *request*; in fact, it is very similar to the requests library.

> In this specific case, our url needs **4 arguments** (*rpcuser*, *rpcpassword*, *host* and *port*), and if you remember, we previously defined those in a *config* *file*. That's why we are calling that same file when *importing* the *libraries*.
{: .prompt-tip }

Also, **timeout** is defined, but outside of the **URL**, it's the time that the *request* might wait for a **response**!

The block **Try/Except** just means that we are going to **try** to establish a *connection* to the provided url with the respective *function*.

> If not successful, we *print* an *error message*, wait *60 seconds*, and try again.
{: .prompt-warning }

And finally, we return the *block hash* of that specific *block height*.

> For example, for **Block number 421523**, the **Hash** returned is "*000000000000000000efb63533e54e338db2fa51315bb26b3ff3e5cec200a883*"
{: .prompt-info }

### Function GetBlock

The next functions follow the same *logical line*, but in this case, they give us **information about the block** regarding that specific given "*Hash*".

We get a JSON response containing a lot of information, like:
- Hash
- Number of confirmations
- Block size
- The block height or index
- The block version
- Etc ...

```python
def getblock(block_hash):
    rpc_connection = AuthServiceProxy("http://%s:%s@%s:%s"%(rpcuser, rpcpassword, host, port ), timeout=timeout)
    try:
        block= rpc_connection.getblock(block_hash)
    except:
        print("Got error waiting 60 seconds on getblock")
        time.sleep(60)
        rpc_connection = AuthServiceProxy("http://%s:%s@%s:%s"%(rpcuser, rpcpassword, host, port ), timeout=timeout)
        block= rpc_connection.getblock(block_hash)
    return block
```

Notice that the only *piece* of code that changes is the function name; in this case, it's the "*getblock*" function.

### Function GetRawTransaction

Now we have the *"Get Raw Transaction"* **Funtion**.

```python
def getrawtransaction(first_tx):
    rpc_connection = AuthServiceProxy("http://%s:%s@%s:%s"%(rpcuser, rpcpassword, host, port ), timeout=timeout)
    try:
        tx= rpc_connection.getrawtransaction(first_tx, True)
    except:
        print("Got error waiting 60 seconds on getrawtransaction")
        time.sleep(60)
        rpc_connection = AuthServiceProxy("http://%s:%s@%s:%s"%(rpcuser, rpcpassword, host, port ), timeout=timeout)
        tx= rpc_connection.getrawtransaction(first_tx, True)
    return tx
```

When calling this RPC Call, we can retrieve information about the specific transaction, like:
- Transfer Amounts
- Address Receiver/sender
- Size
- Version
- ...

This is going to be one of the **most important** functions due to the fact that it is the *Call* that gives information about a specific *transaction* between two entities directly from the *Bitcoin Ledger*.

### Function GetBlockCount

Last but not least, we have the *"Get Block Count"* **Funtion**.

```python
def getblockcount():
    rpc_connection = AuthServiceProxy("http://%s:%s@%s:%s"%(rpcuser, rpcpassword, host, port ), timeout=timeout)
    try:
        block_height= rpc_connection.getblockcount()
    except:
        print("Got error waiting 60 seconds on getblockcount")
        time.sleep(60)
        rpc_connection = AuthServiceProxy("http://%s:%s@%s:%s"%(rpcuser, rpcpassword, host, port ), timeout=timeout)
        block_height= rpc_connection.getblockcount()
    return block_height
```

It doesn't need any *argument*, the main goal of this function is to return the **height** of the most fully **validated** chain.

> For more information about the **RPC Call API** for **Bitcoin RPC**, please go to the [Bitcoin Developer Reference Page](https://developer.bitcoin.org/reference/rpc/index.html).
{: .prompt-tip }