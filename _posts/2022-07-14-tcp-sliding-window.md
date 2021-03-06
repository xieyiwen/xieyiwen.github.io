---
layout: post
title: TCP滑动窗口
categories: ["网络基础", "TCP"]
date: 2022-07-13
keywords: ["TCP", "TCP/IP"]
---

TCP通过滑动窗口实现可靠的批量数据传输。

## TCP滑动窗口

滑动窗口由接收方设定，本质是接收方提供一段缓存，用爱存储窗口内的数据。

为了确保传输可靠，数据重传已窗口为单位进行。

<img src="/assets/images/20220714/normal.png" width="60%">

如上图所示，窗口为4000，发送方按照1000数据为1组，一个窗口同时发送四组的操作进行。接收端每次回复的ACK为每组最后一个数+1.

<img src="/assets/images/20220714/send_lose.png" width="60%">

如果一个组内出现了一个ACK（3001）未被接收，但是最后一组的ack（4001）被接收了，那么接收端会按照窗口缓存的内容，更新收到的最新ack为最后一组的ack（4001），这种情况下，无须处理。

<img src="/assets/images/20220714/notify_lose.png" width="60%">

如果发送的窗口中，出现某组数据丢失。那么接收端会回复连续最近的一个ACK（1001），发送方收到相同ack后会知道传输丢失的数据组（1001～），发送端自行发送丢失的最近一组数据。接收端收到后，通过缓存的窗口中其他数据组，直接返回最后一组数据的ACK（4001）。


