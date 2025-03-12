---
title: Setting up your Bitcoin Node on a Raspberry Pi 4 and enable SSH
layout: post
description: Quick tutorial on How to get the Bitcoin Ledger on a Raspberry Pi 4 with Umbrel and enable SSH
categories: [Bitcoin, Umbrel]
tags: [informative, technology, bitcoin, umbrel, raspberry pi 4, SSH]
image:
  path: /assets/posts/images/Bitcoin-Raspberry-Pi.png
  alt: Bitcoin Blockchain
---

For the next series of posts, we will be **interacting** with the Bitcoin Ledger.

If you're interested in working with **Bitcoin**, whether it's developing **applications**, conducting **research**, or just using it for **personal transactions**, it's crucial to download your own *Bitcoin ledger*. 

>In this post, we'll discuss why downloading and maintaining your own *Bitcoin ledger* is important and what benefits it can provide.
{: .prompt-info }

**Bitcoin** is a *decentralized digital currency* that operates on a *peer-to-peer network*. 
To interact with the **Bitcoin network**, you'll need to have access to the *Bitcoin ledger*, which is a database of **all** Bitcoin **transactions** that have ever occurred. 

Using a **Raspberry Pi 4** with *Umbrel*, you can *download* and store the *Bitcoin ledger*, this solution has become increasingly popular nowadays.

>  You can download the *Bitcoin ledger* on your computer by accessing the [official Bitcoin website](https://bitcoin.org/en/download).
{: .prompt-tip }

In this *post*, we'll explore why you should consider *downloading* your own **Bitcoin ledger** with a *Raspberry Pi 4* and *Umbrel*.

## What is a Raspberry Pi 4?
A **Raspberry Pi 4** is a *small* **single-board** computer that runs on the *Linux operating system*. It's **small**, **energy-efficient**, and relatively **inexpensive**, making it an excellent choice for a variety of projects, like *robot controllers*, *game/web servers*, and  *repetitive tasks* like connecting to a *downloaded ledger*.

## What is Umbrel?
Umbrel is a *self-hosted personal server* that you can run on a *Raspberry Pi 4* to manage your **Bitcoin wallet**, **run your own bitcoin node**, and **interact** with the *Bitcoin network*. It's an *open-source project* that provides an *easy-to-use interface* for downloading and storing the *Bitcoin ledger*, and it also includes additional features like **Lightning Network integration**.

![Umbrel Homepage](/assets/posts/images/umbrel-homepage.jpg)
_Check Umbrel github @ https://github.com/getumbrel/umbrel_

## Why use a Raspberry Pi 4 with Umbrel for downloading the Bitcoin ledger?
There are several reasons why using a *Raspberry Pi 4* with **Umbrel** is an excellent option for **downloading** and storing the Bitcoin ledger:

1. **Energy efficiency**: A *Raspberry Pi* 4 is *energy-efficient*, using only a *fraction* of the energy that a traditional computer would use. This means you can leave it *running 24/7* without worrying about a significant increase in your *electricity bill*.

2. **Cost-effective**: A *Raspberry Pi 4* is relatively *inexpensive*, making it an *affordable* option.

3. **Easy to use**: *Umbrel* provides a very *Uuer friendly interface* that makes it *simple* to download and manage the *Bitcoin ledger*, even for those without *extensive* technical knowledge.

4. **Increased privacy**: When you download and store the ledger on your own device, you have increased *privacy* and *control* over your data. You're not relying on a *third-party service* to *store* your data, which means you have full control over how your data is *stored* and *accessed*.

## How to Set Up a Raspberry Pi 4 with Umbrel
Setting up a *Raspberry Pi 4* with **Umbrel** is relatively *straightforward*. You'll need a few things to get started:
- **Raspberry Pi 4**
- **16GB + MicroSD**
- **Power supply**
- **Ethernet cable**
- **Storage Drive** (preferable an  **SSD** of at *least 1* **TB**)

### Steps for a fully synced Bitcoin ledger:
1. *Download* the **Umbrel OS image** from the [Umbrel website](https://umbrel.com/#).

2. *Flash* the *Umbrel OS image* to your *SD* *card* using a program like *Balena Etcher*.

3. Insert the *SD* *card* into your **Raspberry Pi 4** and connect the **Ethernet cable** and **power supply**.

4. Follow the on-screen instructions to set up **Umbrel** and download the *Bitcoin ledger*.

5. Once the *Bitcoin ledger* has been downloaded and synced, you can interact with the *Bitcoin network* using your own node.

## What is SSH?
**SSH** (*Secure Shell*) is a *way* to *access* and *control* a **device** (like your *Umbrel* server that is running on a Raspberry Pi 4) through the command line. 
To access *Umbrel* via **SSH**, we need to follow the following **steps**:

1. Open a *command prompt* on your computer (on the *windows* search menu for "cmd"). On **macOS**, you can open the *Terminal* app.

2. Type in the following *command* and press the *Enter* key:

    ```bash
    ssh -t umbrel@umbrel.local
    ```

    > In the command given above, you can replace **umbrel.local** with the local IP of your *Umbrel* if you prefer.If you are using PowerShell on Windows 10, you may need to run ssh umbrel@umbrel instead of the command given above.
    {: .prompt-tip }

3. You will then be prompted to enter your password. 

    > This is the same password you use to access your *Umbrel* through a browser. 
    {: .prompt-info }

4. After you have typed in your password, it will establish an **SSH** connection to your *Umbrel* server. You can now **begin** using the **command line interface** to perform various *actions* and *troubleshoot* issues that require **SSH**.

## Conclusion

In conclusion, downloading your own *Bitcoin ledger* with a *Raspberry Pi 4* and *Umbrel* is an excellent option for those interested in *Bitcoin*. It's *energy-efficient*, *cost-effective*, *easy to use*, and provides increased *privacy* and *control* over your data. 
In *future* posts we will check on "How to interact with our *Bitcoin* *Ledger* using *Python* and *RPC* *Calls*".