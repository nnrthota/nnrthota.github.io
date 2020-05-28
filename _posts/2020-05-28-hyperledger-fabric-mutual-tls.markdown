---
layout: post
title:  "Hyperledger fabric Mutual TLS"
comments: true
date:   2020-05-28 13:29:35 +0400
categories: posts
---

<img align="center" width="850" src="../../../../../images/mutualtls/main.png">

***TLS*** — encryption & integrity

when A wants to send a message to B

***integrity***: B should know that the message comes from A not from C or D

***encryption***: A sends information to B in a encrypted way

<p>Anytime you use a web browser to connect to a secure site (https://something), you’re using Transport Layer Security (TLS). TLS is the successor to SSL and it’s an excellent standard with many features. TLS guarantees the identity of the server to the client and provides a two-way encrypted channel between the server and the client.</p>

***TLS: Authenticating the Server***
The server sends its digital X.509 certificate (and any intermediate certificates) to the client. The client verifies the server’s certificate by using one of its pre-trusted root certificates. Most clients use the Microsoft or Mozilla set of trusted root certificates. At the end of this process, the client knows exactly who the server is. When you’re browsing the web, the result of this process is the green lock symbol indicating that your browser has established a trust relationship with the server.
Mutual TLS to the rescue! It’s an optional feature for TLS. It enables the server to authenticate the identity of the client.

***How we can trigger mutual TLS in fabric?***

An orderer has TLS client authentication enabled if the ORDERER_GENERAL_TLS_CLIENTAUTHREQUIRED the environment variable is set to true. A peer has TLS client authentication enabled if the CORE_PEER_TLS_CLIENTAUTHREQUIRED the environment variable is set to true.

***Peer***
<img align="center" width="850" src="../../../../../images/mutualtls/peer.png">

***Orderer***
<img align="center" width="850" src="../../../../../images/mutualtls/orderer.png">

**Connecting to an orderer or peer with TLS client authentication enabled:**

After retrieving the client certificate and client key, assign those to the Client instance. The Client instance will then assign the material to each orderer and peer it creates.

***For example***, the following demonstrates how to assign to the client first. Then build an orderer and peer which will then have the TLS client authentication enabled. This assumes that the client’s PEM-encoded TLS key and certificate are at somepath/tls/client.key and somepath/tls/client.crt, respectively.

<img align="center" width="850" src="../../../../../images/mutualtls/code1.png">

**Let us implement this by adding the following snippet:**

<img align="center" width="850" src="../../../../../images/mutualtls/code2.png">

> If you mistake anything you will see below error

`E0923 16:30:14.963494564 31166 ssl_transport_security.cc:188] ssl_info_callback: error occured.
E0923 16:30:14.963567129 31166 ssl_transport_security.cc:989] Handshake failed with fatal error SSL_ERROR_SSL: error:14094412:SSL routines:ssl3_read_bytes:sslv3 alert bad certificate.
E0923 16:30:15.964456710 31166 ssl_transport_security.cc:188] ssl_info_callback: error occured.`

<img align="center" width="850" src="../../../../../images/mutualtls/error.png">