---
title: Extract All Exchanges symbols by using Requests
layout: post
description: Simple script to retrieve to a CSV all crypto exchanges presented on CoinGecko
categories: [Python, Crypto]
tags: [informative, technology, crypto, python]
image:
  path: /assets/posts/images/crypto_exchanges.png
  alt: Image showing USA and soome random Crypto exchanges
---

The main goal of the following script is to retrieve information about **all the crypto exchanges** identified by *[https://www.coingecko.com/en](https://www.coingecko.com/en)*

We are acessing the Website by using the Requests Library 
```python
import requests
```

First we need to define our **base_url** for the request call
```python
base_url = "https://api.coingecko.com/api/v3/"
```

An then using *requests* to acess the information from get and save it in a variable.
```python
req = requests.get(base_url + "/exchanges/list")
```

Because the information that we are getting is a **JSON** file, i.e., **dictionaires**, several **keys** and respective **values** we need to transform this data into a **dataframe** so we can work it later.
```python
req = requests.get(base_url + "/exchanges/list")
res = json.loads(req.text)
df_exchange_symbols = pd.DataFrame.from_dict(res) 
```

Let's not forget to give a proper name to our **index column**, in this case we can just keep it simple and name it *index* so it wont be **empty**
```python
df_exchange_symbols.index.name = 'index'
```

Dont forget we need to save our **output directory** to save the file, for this example the folder will be called *"Output"*.
```python
output_folder= "Output"
if not os.path.exists(output_folder):
       os.mkdir(output_folder)
```

And finnaly just save the file to any location at your **choice!**
```python
df_exchange_symbols.to_csv(os.path.join(output_folder, 'AllExchanges_CoinGecko_{}.csv'.format(str(datetime.today().strftime('%Y%m%d')))))
```

This **method** is a little bigger because we are adding the **current date** to the file name!

We are also returning an **Dataframe** that contains this information.

To check the CSV created please look at the file inside of the Output table

To see the full Script please visit the repository in:
**[See project repository](https://github.com/Invoany/get_exchange_symbols_coingecko)**

Check here for an example of the CSV file:

[<img src="/assets/posts/images/excel.png" alt="Excel thumbnail">](https://github.com/Invoany/get_exchange_symbols_coingecko/blob/main/Output/AllExchanges_CoinGecko_20231102.csv)

Thank you for your attention!
