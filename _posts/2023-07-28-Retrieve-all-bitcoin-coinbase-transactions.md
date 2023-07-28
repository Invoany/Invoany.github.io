---
title: Retrieve all Bitcoin Coinbase Transactions by API Call using Python
layout: post
description: Project to demonstrte how the user can retrieve the reward transaction information doirectly from the node using several tools like Python, SQLite, Dataframes and others
categories: [Python, Coinbase]
tags: [Database, technology, SQL, Python, SQLite3]
image:
  path: /assets/posts/images/mining.png
  alt: How Mining Works
---

## Introduction
In the domain of *cryptocurrencies*, **Bitcoin** has appeared as a pioneer and continues to shape the scene of digital finance. 

A fundamental aspect of a **Bitcoin** **transaction** is the concept of a *Coinbase transaction*. 

In this blog post, we will explore the importance of a **Bitcoin** *Coinbase transaction*, its responsibility in the *blockchain network*, and the significance it holds for *users* and the broader *cryptocurrency ecosystem*.

## Characteristics of a Coinbase Transaction
A *Coinbase transaction*, also known as a **reward transaction**, is the *first* *transaction* in every **Bitcoin** block and has the following characteristics:
- *Unique* type of *transaction* that creates new **bitcoins**.
- *Rewards* the *miner* who successfully solves the *complex mathematical puzzle* to **validate** the block. 
- This *transaction* is crucial for maintaining the *integrity* of the **blockchain**.
- **Incentivizing** *miners* to participate in the **network**.

### Block Rewards and Incentives
The *Coinbase transaction* plays a vital role in the incentive structure of the **Bitcoin network**. 

*Miners* invest significant computational **power** and **resources** to **secure** the **blockchain**, *verify transactions*, and add **new** **blocks**. 

As a **reward** for their efforts, *miners* receive a predetermined amount of *newly minted bitcoins* through the *Coinbase transaction*. 

This **incentivizes** *miners* to continue their *work*, ensuring the *security* and *stability* of the *network*.

### Supply and Halving
*Coinbase transactions* have a **direct impact** on the **Bitcoin** *supply*. 

Initially, the *Coinbase reward* was set at **50 bitcoins** *per block*. However, as part of the **Bitcoin protocol**, the **reward** is *halved* approximately every *four* *years* in an event known as the "*halving*". 

This *reduction in block rewards* ensures a **controlled** and **predictable** issuance of **new bitcoins** *over time*, ultimately leading to a *finite supply* of *21 million bitcoins*. 

The **halving event**, eagerly anticipated by the *crypto community*, has significant implications for **Bitcoin**'s **scarcity** and **potential value**.

### Verification and Blockchain Consensus
Every *Coinbase transaction* is a **critical** element in the *verification* process for **Bitcoin transactions**. 
1. It establishes the **proof of work** and confirms that the *miner* has successfully *solved* the *cryptographic* *puzzle* to *validate* the **block**. 
2. Once the *Coinbase transaction* is **included** in the **block**, it becomes part of the **immutable** *blockchain*, serving as a *proof* of the *miner*'s efforts and *validating* the *authenticity* of subsequent **transactions** within the **block**.

### Impact on Transaction Fees
Beyond the **block rewards**, *Coinbase transactions* **indirectly** affect the *transaction fees* within the **Bitcoin network**. 
- As **block rewards** decrease over time due to *halving events*, **fees** become a more significant source of income for miners. 
- **Users** who want their transactions to be *prioritized* can add *higher fees* to **incentivize** *miners* to include their transactions in the **blocks** they **mine**.
- The presence of *Coinbase transactions* influences the **fee dynamics** and *market forces* within the **Bitcoin ecosystem**.

## Retrieving Coinbase Transactions

It is possible through *API calls* to retrieve information about Coinbase *Transactions*, including the **value**, the **address** with the minted *bitcoins*, the **fees**, and even the **messages** of each *Transaction* Reward. Each **miner**, after *mining the block*, can write a *message/note* on that same *transaction*.

In the following paragraphs, I will show a **project/script** that was *developed* with the *goal* of retrieving information regarding *Coinbase Transactions* from the whole *Bitcoin Ledger*.

### Used Libraries

For the following Project, we used the following modules:
```python
import pandas as pd
from Adhoc.sql_calls import bitcoin_sqlite3, check_database, check_max
from Adhoc.rpc_calls import getblockhash, getblock, getrawtransaction, getblockcount
from Adhoc.decrypt import address, hex_to_ascii
```

- The *Pandas* are mainly for the creation of *Dataframes* that are going to be used to *save information* about the **reward** for each **Transaction**.
- To save the information the library **SQLite** was used, please check the *previous* blog post, [Getting to Simplifying Database Operations with Python's sqlite3 Module](/posts/Simplifying-Database-Operations-with-Python-sqlite3-Module/), for more information about the *functions* that were used.
- For the connection to the **bitcoin ledger**, *RPC Calls* were used, just like the script used in [How to connect to your own Bitcoin Node using RPC Calls and Python](/posts/connect-to-Bitcoin-using-RPC-Calls/)
- Also, a library was imported to [Easily generate the bitcoin address from the public key using Python](https://gist.github.com/circulosmeos/ef6497fd3344c2c2508b92bb9831173f) and adapted to also Decode the **Coinbase Messages**.

> The **Coinbase Messages** are encoded in the form of *hexadecimal* characters that need to be converted to *ASCII* for us *humans* to understand.
{: .prompt-tip }

### The Script Header

Let's initialize our Script by defining some variables.
```python
try:
    min_height = check_max() + 1
    insert_type = 'append'
except:
    min_height = 1
    insert_type = 'replace'

max_height = getblockcount()
all_blocks = pd.DataFrame()
i = 0
```
1. The first thing to do is *check* if we already have any **blocks** on our *Database*; if so, then what is the **higher block** and sum 1 to get the next **valid block** (*Append*); if **no number** is found, then we need to create a new *Database*, and the starting block is **1** (*Replace*).
2. Save the **highest valid block** number in **Bitcoin** as a variable.
3. Create an *empty Dataframe* to **save** the information retrieved.
4. *Initialize* the **starting number** for looping through the blocks until the highest valid block.


###  Interacting with the Bitcoin Blocks 

After defining the **minimum height Block** in our Database (*If it exists*), we can start *interating* through the blocks until the last block is validated by the miners.

```python
for block_height in range(min_height,max_height):
    i += 1
    coinbase_txid = getblock(getblockhash(block_height))['tx'][0]
    tx_coinbase= getrawtransaction(coinbase_txid)
    series_tx = pd.Series([],dtype=pd.StringDtype())
    series_tx['block_height'] = block_height
    series_tx['txid'] = coinbase_txid
```
Each time we *interate* through a block, we are going to count the blocks we are *treating* (we will see at the *end* why!).

> So where is the information for each *Coinbase Transaction*?
We know that the **first transaction** of a block is going to be the **transaction reward**.
{: .prompt-info }

We are getting the *Hash* of **each block** we are *checking* by using *getblockhash*.
With that *Hash*, we check the information inside that specific block by using *getblock*.
> We can find a lot of information inside  a **Block**, like the *Hash*, the *Size*, the *Height*, the *Version*, the *Time* it takes, the *difficulty*, the *transactions*, etc.
{: .prompt-tip }

We want information **only** regarding the transaction (['tx']), and we **only** want the **first Transaction** information because it is the *one related* to the *reward*.

> The **ID** is going to be **Zero** because it is in Python, Indexes start at **Zero** and not at **One** ([0]).
{: .prompt-tip }

So the variable *coinbase_txid* will give us the *Hash* of the **Coinbase reward** transaction.
By using *getrawtransaction*, we can access **all the attributes** for the specific given *Hash* and save them in a *Dictionary* object called **tx_coinbase**.

We create a *Series object* that is going to accommodate all the attributes for each **Transaction**.
We save in the *Series* the *Block height* that we are currently working on and the respective *Hash* of that **transaction**.

### Inputs of a Coinbase Transaction

Inside the previous *IF statement*, we are going to access the *elements/items* of the saved *Dict* **tx_coinbase**.

```python
for key, value in tx_coinbase.items():
    if key == "vin" :
        for vin_dict in value:
            for vin_key, vin_value in vin_dict.items():
                if vin_key == "coinbase":
                    series_tx['coinbase_hex'] = vin_value
                    series_tx['coinbase_decoded'] = hex_to_ascii(vin_value)
```
We can see that we are looking for the **key** in the dict with the *key == "vin"* that represents **all inputs** of that **transaction**, and by itself, it's another dictionary.
For the *Block reward*, we are going to save two attributes:
- The **coibase_hex** (Represents the *Coinbase* message but **encoded** in *Hex*).
- The **coinbase_decoded** (it's the previous attribute but **decoded** using an *ASCII* table) is saved in the previous *Series object* created.

### Outputs of a Coinbase Transaction

Has for the next **ELIF** in our *IF statement*, we are going to find information about the **outputs** of a *Block reward Transaction*.

```python
elif key == "vout":
    a_value, b_value = []
    for tx_cb_receiver in value:
        a_value.append(format(tx_cb_receiver['value'],'.8f'))
        sc_dict = tx_cb_receiver['scriptPubKey']
        if sc_dict['type'] == 'pubkeyhash':
            b_value.append(sc_dict['address'])
        elif sc_dict['type'] == 'nonstandard':
            b_value.append("nonstandard")
        elif sc_dict['type'] == 'pubkey':
            asm_list = sc_dict['asm'].split()
            b_value.append(address(asm_list[0], False))
    series_tx['value'] = sum(list(map(float,a_value)))
    series_tx['address'] = ','.join(b_value)
```
It's a little more *complex*, but *easy* to understand. Let's see *step* by *step*:
1. First, we **identify** the *Attibute* with a dict of *output information*.
2. We create *empty* lists that we are going to use to save **Addresses** and **Values**, respectively.

    > A **Block reward** can have several **Output addresses**; the same happens to **values**. Don't forget that besides the *Reward*, the **Miner** also receives the **fees** originated from the *transaction* in that block.
    {: .prompt-warning }

3. For the attribute **Value** we want it to be *treated* as a **Float** with a maximum of **8 decimal places**, *append* the **values** to the list created.
4. Now we need to *identify* the *type* of **pubkey** that is inside the attribute **scriptPubKey**.
  - *If type =* **pubkeyhash**, *then* *append* the **address** attribute to the *list*.
  - *If type =* **nonstandard**, *then* *append* the string "**nonstandard**" (It is not possible to estimate the address, very few cases).
  - *if type =* **pubkey**, *then* pick the attribute "**asm**", and **Split** the *value*; however, we save the first *value* of that split and **Decode** it by using the function **address**.
5. At last, we save the **Value** by *summing* up all values inside the *list* into a **single float number**, and for the other list, we **concatenate** all identified **addresses** for the specific **Block reward** by adding a *comma* at the end of each **address**.

### Preparing the Dataframe

Ok, so by now we already have the *information* we need from one block; let's save it into a *Dataframe*.

```python
df = series_tx.to_frame()   
df_T= df.transpose()
df_T.set_index("block_height", inplace = True)
all_blocks = pd.concat([all_blocks, df_T])
```
- **First**, we need to pick our *Series* and convert it into a *Dataframe*.
- However, we have only **2 columns**, one for the *Attribute name* and the other for the *Value* of that attribute. Let's **transpose** that *information* so we have **only one line** by *transction*.
- Now it's easy to *identify* that the **Block Height** is going to be our *Index*; It's *unique* for each **block reward**.
- Finnally we append this information into the empty *Dataframe* we created on the **Script Header**. One by one, this *Dataframe* is going to append all *Transactions* and then *pass them on* to the **next block** or **next interaction** on the *for loop*.

### Saving Coinbase Transactions into a Database

Remeber in the beggining we created a variable to increment each time we go trough a interationt in the for loop (each block)? We created it so we can save our progress.

```python
if i==100 or block_height == (max_height - 1):
    bitcoin_sqlite3(all_blocks,insert_type)
    all_blocks = pd.DataFrame() 
    i=0
```

Each time we have **100 transactions** in our *Dataframe* or we reached the *highest valid block height* se save the *Dataframe* in a **SQLite** Table.
**Reset** the *Dataframe* and also the *incremental variable*.

## Conclusion
The **Bitcoin** *Coinbase transaction* holds immense importance in the world of **cryptocurrencies**. 

It not only **incentivizes** *miners* but also plays a crucial role in *maintaining* the **security**, **integrity**, and **consensus** of the *blockchain*. 

As the **Bitcoin network** continues to *evolve*, understanding the significance of *Coinbase transactions* provides a **deeper** insight into the **inner** workings of this *revolutionary digital currency*.

To check the *full Scipt* or *Github* project please check at [Github page - coinbase_transactions](https://github.com/Invoany/coinbase_transactions).

Thank you for your attention!
