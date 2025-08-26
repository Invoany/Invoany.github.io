---
title: Retrieve important information about the current status of the Bitcoin Blockchain and my Raspberry Pi
layout: post
description: Project to demonstrte how the user can retrieve important information from the current status of the Bitcoin Blockchain and its full bitcoin node in my case my Raspberry Pi
categories: [Python, Blockchain, Monitoring]
tags: [Database, technology, SQL, Python, SQLite3]
image:
  path: /assets/posts/images/Monitoring_KPI.png
  alt: Blockchain and Raspberry Pi Monitoring
---

## Introduction
Running a **full Bitcoin** node comes with its unique set of challenges. 

Beyond the **initial setup**, the constant need to stay synchronized with the latest blocks in the blockchain is demanding, especially on *limited hardware* like the *Raspberry Pi*. 

With the blockchain now exceeding **600GB**, it's not just about keeping your node *up-to-date*, but also about *managing* *storage*, *bandwidth*, and *power* *consumption* *effectively*.

One of the key reasons I maintain a *full* *node* is to contribute to the *decentralized* nature of **Bitcoin**. 

Every *full* *node* strengthens the network by verifying *transactions* *independently*, providing additional *security* and *resilience* against attacks. However, this comes at the cost of ensuring my system is *operational*, *optimized*, and capable of handling the *ever-growing* **blockchain**.


## Bitcoin Blockchain Overview

To fully understand the *state* and *performance* of the **Bitcoin** **network**, it's essential to examine *key* *metrics* that reflect its operation. This section provides an *overview* of these crucial aspects:

- **Highest** **Block**: Or the current **block** **height**, indicates the *latest block* that has been added to the **blockchain**. It represents the most recent set of *transactions* that have been *verified* and *recorded*, showcasing the blockchain's **progress** and **current** **length**.

- **Last** **Hash**: Each block in the **Bitcoin** **blockchain** is identified by its unique **cryptographic** **hash**. The *last* *hash* of the most recent block ensures the *integrity* and *security* of the **blockchain**, as it *links* the new block to the *previous* one, forming a *continuous* chain.

- **Difficulty**: The *difficulty* of mining a *new* *block* adjusts *approximately* every **two weeks**, reflecting the current computational challenge required to add a *new* *block*. This mechanism ensures that blocks are added at a *steady* *rate*, *maintaining* the *stability* and *predictability* of the **Bitcoin** **network**.

- **Blockchain** **Size**: The *size* of the **blockchain**, measured in **gigabytes**, indicates the total volume of *data* *stored* within the *network*. As the **blockchain** grows with each added block, monitoring its *size* is crucial for understanding *storage* *requirements* and the overall *scalability* of the *network*.

These metrics provide a snapshot of the blockchain's current *state* and help in assessing the **overall health** and **efficiency** of the **Bitcoin** **network**.

<div class="table-wrapper">
<table border="1" class="dataframe my_table">
  <thead>
    <tr style="text-align: right;">
      <th>Data Point Description</th>
      <th>Data Point Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Current time</td>
      <td>26/08/2025 12:22:03</td>
    </tr>
    <tr>
      <td>Height of the most-work fully-validated block in my Raspberry</td>
      <td>911,774</td>
    </tr>
    <tr>
      <td>Hash of the best (tip) block in the most-work fully-validated chain</td>
      <td>00000000000000000000f16a672267fa17b024c1bc24b54503de32c51626c988</td>
    </tr>
    <tr>
      <td>Current network name (main, test, regtest)</td>
      <td>main</td>
    </tr>
    <tr>
      <td>Current number of headers validated on-chain</td>
      <td>911,774</td>
    </tr>
    <tr>
      <td>Current difficulty</td>
      <td>129699156960680.9</td>
    </tr>
    <tr>
      <td>Median time for the current best block</td>
      <td>1756204463</td>
    </tr>
    <tr>
      <td>Estimated size of the block and undo files on disk</td>
      <td>724 GB</td>
    </tr>
    <tr>
      <td>Difference between number of headers validated vs height of most validated</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>

_Current Metrics and information regarding my Bitcoin Full node._


## Is the Bitcoin Network Operational?
For those managing a **Bitcoin full** node on a **Raspberry** **Pi**, ensuring that your node is *operational* is essential for *maintaining* network *health* and *reliability*. Here’s how to assess the operational status of your **Bitcoin full node** and **Raspberry** **Pi**:

### **Node Synchronization**:

- Current **Sync Status**: Check the *synchronization* status of your **Bitcoin full node**. Your node should be *actively* *syncing* with the **blockchain**, meaning it is continually *downloading* and *verifying* blocks. Look for indicators in your node’s software that show how *up-to-date* your node is with the latest blocks.
- **Block** **Height**: Compare the **current block height** of your node with the **known highest block** on the **Bitcoin** **network**. If your node is *significantly behind*, it may be experiencing *synchronization issues*.

### **Last Block Hash**:

- Verify **Recent** **Hash**: Ensure that your node’s *last block hash *matches the *latest hash* recorded on the **blockchain**. This confirms that your node is *correctly* processing and *validating* the most *recent block*.

### **System Performance**:

- **Monitor Resource Usage**: Regularly check the *resource* *usage* on your **Raspberry** **Pi**, including *CPU*, *memory*, and *storage*. *High* resource utilization may indicate that your node is *struggling* to keep up with the **blockchain** or that there are issues with your *setup*.
- **Check Disk Space**: Ensure that you have adequate **disk** **space** for the growing **blockchain**. The **Bitcoin** **blockchain** is *large* and continues to *grow*, so it's *essential* to monitor and *manage* disk space *effectively*.

### **Network Connectivity**:

- **Connection Status**: Verify that your **Raspberry** **Pi** is properly connected to the **Bitcoin** **network**. This includes checking *network* *logs* for any *connectivity* *issues* and ensuring that your node can *communicate* with other *peers* on the **network**.

### **Error Logs and Troubleshooting**:

- **Review Logs**: Regularly review your **node’s** *error* *logs* for any *warnings* or *errors* that might indicate issues with *synchronization* or network **connectivity**.
- **Troubleshoot Issues**: Address any *errors* or *warnings* promptly to ensure your node remains *fully* operational and contributes *effectively* to the **Bitcoin network**.

By keeping a close eye on these factors, you can ensure that your **Bitcoin full node** is running *smoothly* and that your **Raspberry Pi** is performing well in *maintaining* the **network**.


## Raspberry Pi Usage

Monitoring your **Raspberry Pi’s performance** is crucial for ensuring that it runs efficiently as a Bitcoin full node. 

<div class="table-wrapper">
<table border="1" class="dataframe my_table">
  <thead>
    <tr style="text-align: right;">
      <th>Data Point Description</th>
      <th>Data Point Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Total Space</td>
      <td>1,375 GB</td>
    </tr>
    <tr>
      <td>Used Space</td>
      <td>805 GB</td>
    </tr>
    <tr>
      <td>Free Space</td>
      <td>500 GB</td>
    </tr>
    <tr>
      <td>Raspberry Pi Temperature</td>
      <td>52.095</td>
    </tr>
  </tbody>
</table>
</div>

In addition to tracking **total space**, **used space**, **free space**, and **temperature**, here are some other important metrics and factors to keep an eye on:

### **CPU Usage**
- **Monitor Load:** Check the **CPU load** to ensure that the Raspberry Pi is not being overwhelmed. **High CPU usage** can indicate that the node software or other processes are consuming excessive resources.
- **CPU Frequency:** Track the **CPU frequency** to ensure it’s running at expected speeds. **Throttling** or frequent frequency changes might affect performance.

### **Memory Usage**
- **RAM Utilization:** Keep an eye on **RAM usage** to ensure there is enough available memory for the node to operate efficiently. Excessive memory consumption might lead to **slow performance** or crashes.

### **Disk I/O**
- **Read/Write Operations:** Monitor **disk I/O operations** to assess the performance of data read/write processes. **High I/O activity** might impact overall system performance and response times.

### **Network Activity**
- **Bandwidth Usage:** Track the **network bandwidth usage** to ensure that your Raspberry Pi has sufficient network capacity for syncing with the Bitcoin network and processing transactions.
- **Network Errors:** Check for any **network-related errors** or connectivity issues that might affect the node’s ability to communicate with other peers.

### **System Uptime**
- **Monitor Uptime:** Regularly check the **system uptime** to ensure the Raspberry Pi remains operational without unexpected reboots or crashes. This is important for maintaining **continuous node availability**.

### **Temperature and Cooling**
- **Heat Management:** While you’ve already noted the **temperature**, ensure that your Raspberry Pi has adequate **cooling** to prevent **overheating**. Overheating can lead to throttling and reduced performance.

### **Power Supply**
- **Check Voltage:** Monitor the **power supply voltage** to ensure it is **stable** and **sufficient**. An inadequate or unstable power supply can cause **system instability** or crashes.

### **Software Updates and Maintenance**
- **Update Regularly:** Keep your Raspberry Pi’s **operating system** and **node software** up-to-date to benefit from the latest **security patches** and **performance improvements**.
- **Routine Maintenance:** Perform regular **maintenance checks** to ensure that your system remains in good health and to address any potential issues proactively.

By keeping track of these additional metrics, you can better manage the **performance** and **reliability** of your Raspberry Pi as it supports your Bitcoin full node.


## Conclusion

Keeping an eye on your **Bitcoin full node** and **Raspberry Pi** is more than just a technical requirement; it is essential for ensuring the **smooth operation** and **reliability** of your system.

For your **Bitcoin full node**, consistent monitoring ensures that it remains **in sync** with the network, verifies transactions accurately, and supports the overall health of the **Bitcoin blockchain**. By tracking key metrics such as **block height**, **last hash**, **network difficulty**, and **blockchain size**, you safeguard against potential issues that could affect the node’s performance or reliability. This vigilance helps in quickly identifying and resolving problems, ensuring that your node effectively contributes to the **decentralized network**.

Similarly, effective monitoring of your **Raspberry Pi** is crucial for **optimal performance** and **longevity**. Keeping an eye on **CPU load**, **memory usage**, **disk I/O**, and **network activity** helps in preventing bottlenecks and ensuring that your hardware can handle the demands of running a full node. Additionally, monitoring **temperature** and **power supply** is essential to avoid hardware failures and maintain system stability. Regular **updates** and **maintenance** further contribute to the smooth operation of your setup.

In summary, diligent monitoring of both your Bitcoin full node and Raspberry Pi not only enhances their **performance** and **reliability** but also supports the broader Bitcoin network. By staying proactive and addressing any issues promptly, you ensure that your node remains a valuable and effective component of the decentralized ecosystem.

