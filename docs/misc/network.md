# network

---

## Data Overhead

ref: ChatGPT + [TCP Over IP Bandwidth Overhead](https://packetpushers.net/blog/tcp-over-ip-bandwidth-overhead/)

Data Overhead in TCP/IP File Transfers.

When transferring files over the internet using the TCP/IP protocol, there is an additional data overhead that increases the total data consumption. This overhead is typically caused by:

- TCP headers: Used to ensure reliable delivery and proper sequencing of data.  
- IP headers: Provide information for routing the data packets.  
- Other layers: Such as Ethernet or Wi-Fi headers, depending on the medium.  
- For most file transfers, this overhead results in an increase of approximately **3%** of the file size.

---
