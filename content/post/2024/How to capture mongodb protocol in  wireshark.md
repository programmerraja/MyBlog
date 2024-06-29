+++
title = 'How to capture mongodb protocol in  wireshark'
date = 2024-02-12T08:28:27.2727+05:30
draft = false
tags =[]
+++ 

`export SSLKEYLOGFILE=/tmp/tlskey.log`
Run a nodejs program as `node --tls-keylog=/tmp/tlskey index.js` where the tls key will be stored on that path and add this tlskey path to wire shark to decrypt the msg by doing following

Go to Preferences->Protocols->TLS and edit the path as shown in the screenshot below.
![[Pasted image 20240212083026.png]]

Mongodb uses `Mongodb wire protocol` at application level to know more check [here](https://www.mongodb.com/docs/manual/reference/mongodb-wire-protocol/#:~:text=The%20MongoDB%20Wire%20Protocol%20is,a%20regular%20TCP%2FIP%20socket.) 