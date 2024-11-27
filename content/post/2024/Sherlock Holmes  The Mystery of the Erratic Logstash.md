---
title: "Sherlock Holmes: The Mystery of the"
date: 2024-11-25T06:52:20.2020+05:30
draft: true
tags:
---

As you have gone through my pervious blog you may get to know that we are recently migrated to the kubernetes after migration we face few that all are discussed on the previous blogs. today also we going to discuss about the kubernetes related error  so lets get started


We are using kuberentes observablity tool in that we have seen more then 1 Milions of NXdomain error which caught our attentaion and asked me to invistigate why

Basically NXdomain error mean the domain not exists and i checked for the domains it looks like `gmail.googleapis.com.cluster.local` ,  `oauth2.googleapis.com.es52e2p4cate3lv5fzg4m1it5a.bx.internal.cloudapp.net` etc so if you noticied there is something get 