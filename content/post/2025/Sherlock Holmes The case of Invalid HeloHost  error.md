---
title : Sherlock Holmes The case of Invalid HeloHost  error
date : 2025-02-02T04:32:00.000+05:30
draft : true
tags : 
---
I am back with another intersting detective story of Invalid ELHO error when we try to connect user SMTP account 

`550 Requested action not taken: Invalid HeloHost (#5.5.0)","responseCode":550,"command":"MAIL FROM`

We are getting ready for the weekend vibe it was friday where i am going to leave the office to enjoy my weekend suddenly our customer facing team came up with issue that user not able able to connect send mail from our platform where user uses SMTP for mail sending i pulled my laptop and checked for the logs and i find the below error `550 Requested action not taken: Invalid HeloHost (#5.5.0)","responseCode":550,"command":"MAIL FROM` 

After seeing the error it new for me so i did googling and some how i thought may be user have the ip restrict of the server so we asked them to add our IP to there whitelist and we tried again but we get the same error then 