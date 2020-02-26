---
layout: post
title: "Hardening a Windows Server"
date: 2019-11-26
category: Security
tags: [Networking, cloud]
---



### Target
There is a Windows Server running on a VM in the cloud. To make sure that I'm the only person that have access to it. Besides a strong password, I'm going to close all the unnecessary ports and block all the unauthorized Internet access. However, I am not a Security professional. This blog is a journal of my own study.

<br/>

### Study
#### Check Current Open Ports
Simply run 'netstat -a' at the command Prompt

Each row is the details of an open socket. <br/>
There are four columns, which are:
1. **Protocol**: UDP or TCP
2. **Local Address**: local IP address : port number
3. **Foreign Address**: remote IP address: port number
4. **State**: port state

I learnt how to read this list form:
https://sites.google.com/site/xiangyangsite/home/technical-tips/linux-unix/networks-related-commands-on-linux/how-to-read-netstat--an-results
