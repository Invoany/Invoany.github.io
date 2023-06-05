---
title: Retrieving All Cryptocurrencies by using Requests
layout: post
description: Script to retrieve all CryptoCurrencies that are presented on CoinGecko
tags: [informative, technology, crypto, python]
categories: [Python, Crypto]
image:
  path: /assets/posts/images/cryptocurrencies.png
  alt: Several random physical crypto currencies (google)
---

Hello dear Reader!

The next post is again a simple one, where the goal is to retrieve **all the cryptocurrencies** available in *[https://www.coingecko.com/en](https://www.coingecko.com/en)*

For this **Script** we will need some libraries.
We are going to access the **CoinGecko API** so the Requests library is needed!
The information is retrieved  in **JSON** format and saved into a **Dataframe**.
Also, Modules like the **Datetime** and [OS](https://docs.python.org/3/library/os.html) (operating system dependent functionality) are going to be used for easier management of the final Outputs.

```python
import json
import requests
import pandas as pd
from datetime import datetime
import os
```

Let's define our base_url for the **request call** to the API and a variable for our **Output folder**.
P.S. I chosed to call my folder **"Output"** but you can change this just by changing the **variable**
```python
base_url = "https://api.coingecko.com/api/v3/"
output_folder= "Output"
```

Now to create a function, let's define it and print the beginning to the console.
```python
def get_coins_list_coingecko():
    print("Getting the List with all the Coins under CoinGecko")
```

For the request we need to merge the **base_url** with *"coins/list"*, that's the place where the list that we require is.
Let's print the **URL Request** to see what we are going to retrieve.
```python
    response = requests.get(base_url + "coins/list")
    print("Request Url = {}".format(response.url))
```

If we access the link given by the **Print** command, we get the following output.

[![CoinGecko API Call Coins List](/assets/posts/images/Coins-List-Api-CoinGecko-Call.png)](https://api.coingecko.com/api/v3/coins/list)

Seems confusing but in fact is very simple, this is in **JSON** format, and the first **bracket** means we are starting a list, next we have a **curly bracket**, that means we are starting a **dictionary**, notice we have one dictionary for **each coin**, and that coin contains an *ID*, a *Symbol* and a *Name*, each different coin/dictionary are separated by commas.

![CoinGecko API Call Coins List](/assets/posts/images/Coins-List-Json-Example.png)

We now need to retrieve this **text** to **Json** and load it into a **Dataframe** called *all_coins*.
```python
    response_json = json.loads(response.text)
    all_coins = pd.DataFrame.from_dict(response_json)
    all_coins.index.name = 'index'
```

We want to give a name to the *index* column, so it won't be Blank.
Let's **print** the Dataframe to see the output that we already have.

![Dataframe Example for Coins List](/assets/posts/images/Coins-List-Dataframe-Example.png)

Now we just need to save our List!
```python
    if not os.path.exists(output_folder):
        os.mkdir(output_folder)
```

The **OS Module** is very useful, it helps to interact with the **Operating System**.
In this case we have an **IF** condition, it is going to check if the folder(the one we defined at the beginning) exists!

*Keep in mind* that you need to run the Script on the **Parent Folder** of your output Variable, if in your Parent Folder doesn't exist any folder with the same name as the variable output_folder then in that same folder a new folder be created under the command **mkdir**.

For the last step, we need to save the file.
```python
    try:
        all_coins.to_csv(os.path.join(output_folder, 'AllCoins_CoinGecko_{}.csv'.format(str(datetime.today().strftime('%Y%m%d'))) ))
    except:
        print("It was not possible to save/create the file")
```

We are going to use the **try** command, it means that the program will try to execute what we have under the try command, if it fails than the **except** command will be raised.
In this case we are taking the dataframe that we previously created and using the **to_csv** method we are going to export it to an **CSV** file.
As for the arguments, we are joining two things:
1. The path where we want to save the CSV file.
2. The file name, "AllCoins_CoinGecko_CURRENT_DATE.csv"

To calculate the current date, we are using the Module **Datetime** from the library Datetime and converting the date to a desired string format.

At the end we return the Dataframe so anytime we need to access the List we just need to call the function!

To see the full Script please visit the [repository](https://github.com/Invoany/get_all_coins_coingecko "Retrieve all Cryptocurrencies by API(CoinGecko) Call, ID's, Symbols and Names").

Thanks for the reading!