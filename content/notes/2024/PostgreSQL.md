+++
title = 'PostgreSQL'
date = 2024-07-08T06:39:04.044+05:30
draft = true
tags =[]
+++ 


## Internal
- everything in append even when we update
- Every connection is seprate process (max 100 connection default) because mostly people used PGBoucer as proxy in front of PostgreSQL
- they go with this approach for fault tolrance so when one process get crashed it wont affect other but in thread it will 
- WAL shared memory used to communicate between process
- 

