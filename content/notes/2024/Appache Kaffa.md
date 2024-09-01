+++
title = 'Appache Kaffa'
date = 2024-02-25T12:50:53.5353+05:30
draft = true
tags =[]
+++ 

internal archi course
https://developer.confluent.io/courses/architecture/get-started/


### Kafka's Zero-Copy Optimization: Simplified

If you've come across Kafka, you might have heard about its zero-copy optimization, a technique aimed at reducing unnecessary data copies. Let's break it down:

**What is Zero-Copy?**
Zero-copy operations minimize unnecessary data duplication, although they don't literally make zero copies.

**Kafka's Use of Zero-Copy**
Kafka leverages the OS's zero-copy optimization to bypass the Kafka broker Java program entirely when data is transferred from the page cache to the socket buffer.

### Traditional Data Transfer (Without Zero-Copy)

1. **Read buffer (OS page cache)** - Stores data for quick access.
2. **Socket buffer** - Manages data packets.
3. **NIC buffer** - Network card's byte buffer.
4. **DMA (Direct Memory Access)** - Allows hardware to access memory without the CPU.

#### Steps:
1. Disk to OS buffer (DMA copy, user to kernel mode).
2. OS buffer to app buffer (kernel to user mode).
3. App buffer to socket buffer (user to kernel mode).
4. Socket buffer to NIC buffer (DMA copy, kernel to user mode).

### Optimized Data Transfer (With Zero-Copy)

Kafka stores data in a binary format compatible with its responses, skipping unnecessary steps:
- The read buffer copies data directly to the NIC buffer.
- The socket buffer stores read buffer pointers, enabling the DMA engine to read directly from memory.

### Benefits of Zero-Copy
- Fewer user/kernel mode switches (reduced from 4 to 2).
- Same number of DMA copies (2).
- One minimal CPU copy of pointers (2 fewer CPU copies).

### The Reality Check

Despite the efficiency gains, zero-copy might not significantly impact most Kafka deployments:
- The network often saturates before CPU becomes a bottleneck.
- Encryption and SSL/TLS prevent zero-copy use.

**Kafka remains performant even without zero-copy optimization.**



1. What is Kafka?  
    Kafka is a distributed event store and a streaming platform. It began as an internal project at LinkedIn and now powers some of the largest data pipelines in the world in orgs like Netflix, Uber, etc.  
    
2. Kafka Messages  
    Message is the basic unit of data in Kafka. It’s like a record in a table consisting of headers, key, and value.  
    
3. Kafka Topics and Partitions  
    Every message goes to a particular Topic. Think of the topic as a folder on your computer. Topics also have multiple partitions.  
    
4. Advantages of Kafka  
    Kafka can handle multiple producers and consumers, while providing disk-based data retention and high scalability.  
    
5. Kafka Producer  
    Producers in Kafka create new messages, batch them, and send them to a Kafka topic. They also take care of balancing messages across different partitions.  
    
6. Kafka Consumer  
    Kafka consumers work together as a consumer group to read messages from the broker.  
    
7. Kafka Cluster  
    A Kafka cluster consists of several brokers where each partition is replicated across multiple brokers to ensure high availability and redundancy.  
    
8. Use Cases of Kafka  
    Kafka can be used for log analysis, data streaming, change data capture, and system monitoring.

sheet2 | DVOP3-Location | DVOP3-Qty | DVOP3-option Elected | DVOP3-INX Status | DVOP1-Location | DVOP1-Qty | DVOP1-option Elected | DVOP1-INX Status | | -------------- | --------- | -------------------- | ---------------- | -------------- | --------- | -------------------- | ---------------- | | 1MMIC | 1245 | Cash | Accepted | 1MMIC | 1478 | Secu | Accepted | | 5GHTY | 8975 | Secu | TBD | 5GHTY | 8888 | Secu | Accepted | | 5GHTY | 23 | Secu | TBD | 1V3CH | 54 | Secu | TBD | | 1V3CH | 123 | Cash | Accepted | 1V3CH | 123 | Cash | Accepted | | 1V3CH | 22 | Secu | TBD | 1V3CH | 22 | Secu | TBD | | 1V3CH | 44 | Secu | TBD | 1V3CH | 44 | Secu | TBD | sheet1 | Eligiblelocation | Name | Address | Foreign Non-induvidual or Foreign Fund | Legal Status | TIN(IF 0 %) | Tax Rate | DVOP3-Location | DVOP3-Qty | DVOP3-option Elected | DVOP3-INX Status | DVOP1-Location | DVOP1-Qty | DVOP1-option Elected | DVOP1-INX Status | DVOP3-Location | DVOP1-Location | | ---------------- | ---- | ------- | -------------------------------------- | ------------ | ----------- | -------- | -------------- | --------- | -------------------- | ---------------- | -------------- | --------- | -------------------- | ---------------- | -------------- | -------------- | | 1MCGH | | | | | | | | | | | | | | | | | | 1MMIC | | | | | | | | | | | | | | | | | | 1V3CH | | | | | | | | | | | | | | | | | | 1V3CH | | | | | | | | | | | | | | | | | | 1V3CH | | | | | | | | | | | | | | | | | | 1V3CH | | | | | | | | | | | | | | | | | | 5GHTY | | | | | | | | | | | | | | | | | | 5GHTY | | | | | | | | | | | | | | | | | after merge | Eligiblelocation | Name | Address | Foreign Non-induvidual or Foreign Fund | Legal Status | TIN(IF 0 %) | Tax Rate | DVOP3-Location | DVOP3-Qty_x | DVOP3-option Elected_x | DVOP3-INX Status_x | DVOP1-Location _x | DVOP1-Qty_x | DVOP1-option Elected_x | DVOP1-INX Status_x | DVOP3-Location | DVOP1-Location | DVOP3-Qty_y | DVOP3-option Elected_y | DVOP3-INX Status_y | DVOP1-Location _y | DVOP1-Qty_y | DVOP1-option Elected_y | DVOP1-INX Status_y | | ---------------- | ---- | ------- | -------------------------------------- | ------------ | ----------- | -------- | -------------- | ----------- | ---------------------- | ------------------ | ----------------- | ----------- | ---------------------- | ------------------ | -------------- | -------------- | ----------- | ---------------------- | ------------------ | ----------------- | ----------- | ---------------------- | ------------------ | | 1MCGH | | | | | | | | | | | | | | | | | | | | | | | | | 1MMIC | | | | | | | | | | | | | | | | | 1245 | Cash | Accepted | 1MMIC | 1478 | Secu | Accepted | | 1V3CH | | | | | | | | | | | | | | | | | 123 | Cash | Accepted | 1V3CH | 123 | Cash | Accepted | | 1V3CH | | | | | | | | | | | | | | | | | 22 | Secu | TBD | 1V3CH | 22 | Secu | TBD | | 1V3CH | | | | | | | | | | | | | | | | | 44 | Secu | TBD | 1V3CH | 44 | Secu | TBD | | 1V3CH | | | | | | | | | | | | | | | | | 123 | Cash | Accepted | 1V3CH | 123 | Cash | Accepted | | 1V3CH | | | | | | | | | | | | | | | | | 22 | Secu | TBD | 1V3CH | 22 | Secu | TBD | | 1V3CH | | | | | | | | | | | | | | | | | 44 | Secu | TBD | 1V3CH | 44 | Secu | TBD | | 1V3CH | | | | | | | | | | | | | | | | | 123 | Cash | Accepted | 1V3CH | 123 | Cash | Accepted | | 1V3CH | | | | | | | | | | | | | | | | | 22 | Secu | TBD | 1V3CH | 22 | Secu | TBD | | 1V3CH | | | | | | | | | | | | | | | | | 44 | Secu | TBD | 1V3CH | 44 | Secu | TBD | | 1V3CH | | | | | | | | | | | | | | | | | 123 | Cash | Accepted | 1V3CH | 123 | Cash | Accepted | | 1V3CH | | | | | | | | | | | | | | | | | 22 | Secu | TBD | 1V3CH | 22 | Secu | TBD | | 1V3CH | | | | | | | | | | | | | | | | | 44 | Secu | TBD | 1V3CH | 44 | Secu | TBD | | 5GHTY | | | | | | | | | | | | | | | | | 8975 | Secu | TBD | 5GHTY | 8888 | Secu | Accepted | | 5GHTY | | | | | | | | | | | | | | | | | 23 | Secu | TBD | 1V3CH | 54 | Secu | TBD | | 5GHTY | | | | | | | | | | | | | | | | | | | | | | | | but it merege wrong fix the bug







Sure! Let's walk through an example to illustrate what the final output would look like.

### Example Data

Assume the initial data in `Sheet1` and `Sheet2` are as follows:

**Sheet1:**

|thing|petrol|price|state|brand|country|
|---|---|---|---|---|---|
|bike||||||
|car||||||

**Sheet2:**

|name|petrol|price|brand|
|---|---|---|---|
|bike|200l|3000k|pulser|
|car|1l|200k|BMW|
|car|400|4000k|ferrari|

### After Running the Script

The script will merge data from `Sheet2` into `Sheet1` based on the `thing` column. The final content of `Sheet1` will be:

**Sheet1 (Updated):**

|thing|petrol|price|state|brand|country|
|---|---|---|---|---|---|
|bike|200l|3000k||pulser||
|car|1l|200k||BMW||
|car|400|4000k||ferrari||

write a python script that will do copy work from sheet 2 to sheet 1 but it need to do follow blelow instruction to copy let say sheet2 have thing pertrol price brand bike 200l 3000k pulser car 1l 200k BMW car 400 4000k ferrari sheet 1 thing pertrol price state brand country bike car after copying data from sheet 2 to sheet1 thing pertrol price state brand country bike 200l 3000k pulser car 1l 200k BMW car 400 4000k ferrari only copy the shee2 data to sheet1 if sheet1 name in sheet2 in name other wise dont and move the correspoding column and note the sheet name column thing name can be any and that user can use where to lookup on merege and sheet1 have the column that don't have vaule in sheet2 leave it as it is NOTE common_column = 'thing' it can be common but also uncommon let say in sheet1 it will in thing and sheet2 it will in name so adujeuct the code if you have any question feel free to ask and use pandas