---
title: "Fledger"
draft: false
date: 2025-01-07
description: "A decentralized network in your browser"
showTableOfContents: true
---

With fledger you can be part of a decentralized network simply by opening a web page in your browser:

[Decentralized Network](https://web.fledg.re)

Fledger uses WebRTC to connect to other browsers and to form a peer-to-peer network
without having to install a binary on a server.
Currently it has the following two services:

- gossip-network to propagate messages to create a simple chat application
- proxy protocol to send one GET request to a remote node

2025/01 - The current development of fledger is to create a distributed hash table (DHT) and to
store a website.
The goal is to gather tokens by storing websites of other people, and then being able
to use those tokens to have your website stored on other people's computer.
In the end, the following should be implemented:

- a DHT to store the data
- one (or multiple) distributed ledgers to store data

You can find more information in the github repository:

[Fledger](https://github.com/ineiti/fledger)