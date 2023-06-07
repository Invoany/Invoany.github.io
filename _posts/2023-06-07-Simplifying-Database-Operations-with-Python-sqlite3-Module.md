---
title: Getting to Simplifying Database Operations with Python's sqlite3 Module
layout: post
description: Guide to Simplifying Database Operations with Python's sqlite3 Module
categories: [Python, SQLite3]
tags: [Database, technology, SQL, Python, SQLite3 ]
image:
  path: /assets/posts/images/SQLite_post.png
  alt: SQLite Logo
---

In the world of *software development*, working with **databases** is a *common requirement*. Whether it's **storing user** information, **managing inventory**, or processing **large datasets**, having a reliable and *efficient* way to interact with **databases** is *essential*. 

In the **Python** ecosystem, the **sqlite3** *module* provides a *powerful* and *user-friendly* interface for working with **SQLite** **databases**. 

In this blog post, we will explore the capabilities of Python's **sqlite3** *module* and learn how it *simplifies database operations*, by of course, using it to **store** and **save** information about the *Bitcoin Ledger*.

## What is SQLite and why use it?

**SQLite** is a *lightweight*, file-based *relational database* management system with several advantages:

1. **Easy Setup** and **Management**: All **SQLite** databases are file-based, meaning they are *self-contained* within a **single** *file*. 

    > This eliminates the **need** for server *setup* or *configuration*.
    {: .prompt-info }

2. **Portability** across *different* platforms and *operating* *systems*, so it's convenient for *sharing* *data* or *deploying* *applications* on different machines.

3. There are **no dependencies** or **external processes**; it's a **serverless** database engine. It doesn't rely on *external* *processes* or *libraries*, making it *lightweight* and *independent*.

4. **Low resource consumption** for a *small* memory *footprint* and *minimal disk space*. 

5. **ACID compliance**, or (*Atomicity*, *Consistency*, *Isolation*, *Durability*) compliance, guarantees data *integrity*, *transactional consistency*, and *reliability*.

6. **Wide Language Support** makes it available in several programming languages, including **Python**, the one that we are going to address,but also **C/C++**, **Java**, and *others*. 

    >This enables developers to use SQLite with their preferred programming language and leverage its features and benefits.
    {: .prompt-tip }

7. *SQLite* databases are designed for **speed** and **efficiency**. They are optimized for read-heavy workloads and can handle moderate write operations. 

    > Did you know that *SQLite* employs various **techniques** like *indexing*, *caching*, and *query optimization* to ensure *fast* *query* *execution*?
    {: .prompt-tip }

8. **Versatility**: a wide range of **data types** are available, like *integers*, *floats*, *strings*, *dates*, and *more*.

9. **Cross-platform compatibility** by allowing seamless database *access* and *management* across different *operating systems*, including **Windows**, **macOS**, **Linux**, and mobile platforms like **Android** and **iOS**.

10. **Community** and **documentation** are available to the *community* by providing *extensive documentation*, *tutorials*, and *resources*, making it easier for developers to *learn*, *troubleshoot*, and *optimize* their use of **SQLite**.

## Introduction to the SQLite3 Module

**Python**'s **sqlite3** is a built-in *module* that provides a straightforward *API* for working with *SQLite databases*. 
Unlike other database management systems, *SQLite* is **included** in the **Python** standard library, making it *readily* available without any *additional installations*. It offers a comprehensive set of **functions** and **methods** for executing **SQL queries**, managing **transactions**, and handling **database connections**.

## Connecting to a Database

To interact with an **SQLite** database using **Python**, the *first* step is establishing a **connection** to the database. The **sqlite3** *module* provides a **connect()** function that takes the *name* of the database file as a *parameter*. If the specified file doesn't exist, it will be **created** automatically.

> Don't forget to import the library!
```python
import sqlite3
```
{: .prompt-tip }

First, we need to establish a **connection**.
```python
connection = sqlite3.connect("coinbase_txs.db")
cursor = connection.cursor()
connection.commit()
```
- By running the previous command, a new file is created or overwrited in the *current directory*. This file will **store** our database.
- The *cursor* is needed to invoke **methods** that **execute** *SQLite* *statements* and **fetch** data from the result sets of the queries.
- At the end, the **Commit** method should be called so we can close or end the *transaction*.

## Inserting a Dataframe

One of the biggest challenges after creating a Dataframe is finding a place to store it.
```python
def bitcoin_sqlite3(df,insert_type):
    connection = sqlite3.connect("coinbase_txs.db")
    cursor = connection.cursor()
    df.to_sql('bitcoin_coinbase_tx', connection, if_exists = insert_type, index=True)
    connection.commit()
```

An *empty* table is created based on the "*meta*" of the **Dataframe** given.

> Notice that for the **function** we receive *two arguments*, the *dataframe* and a *string* representing the type of **Insert** (append or replace).
{: .prompt-tip }
After creating a **connection** and a **cursor**, we call a *method* built in the dataframe, **to_sql** where four arguments are given:
- **Table name** to where we want to *insert* the Dataframe.
- In our case, the **connection** that we are using is a **SQLite3** **connection**.
- The type of **Insert** if the table *already exists*
- Write Dataframe **index** as a column.

The main goal of this **Function** is to create a *database* of all **Coinbase Transactions**, i.e., the first transactions of each block. That transaction represents the payment of the *reward* to the *miner* of that mined block.

## Selecting all lines of the database

Let's suppose we want to see all the lines in our database.
```python
def check_database():
    connection = sqlite3.connect("coinbase_txs.db")
    cursor = connection.cursor()
    for row in cursor.execute("select * from bitcoin_coinbase_tx"):
        print(row)
    connection.commit()
```

It's the same principle: *connection* to the database and *cursor*.
In this case, we will check each row of the cursor and print it!

> For the query to show all the lines we have.
```sql
select * from bitcoin_coinbase_tx
```
{: .prompt-info }

## Retrieving Max of a column
Other simple SQL operations can be done using the same previous logic.
```python
def check_max():
    connection = sqlite3.connect("coinbase_txs.db")
    cursor = connection.cursor()
    for value in cursor.execute("select MAX(block_height) from bitcoin_coinbase_tx"):
        max_block_height = value[0]
    connection.commit()
    return max_block_height
```
To retrieve the **maximum** *value* of a column, we just need to provide the correct **SQL** statement.
Notice that to retrieve the information, we needed to call **value[0]**. This was necessary because the return *value* was a *list*.

> For the query to show the maximum block height.
```sql
select MAX(block_height) from bitcoin_coinbase_tx
```
{: .prompt-info }

We will leave this blog post open so we can add more *functions* in the future.
Let us know if you have any recommendations; in the meantime, you can find more information under the API documentation [interface for SQLite databases (SQLite3)](https://docs.python.org/3/library/sqlite3.html)!
