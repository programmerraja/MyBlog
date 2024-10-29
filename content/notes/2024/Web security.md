---
title: Web security
date: 2024-08-31T08:39:15.1515+05:30
draft: true
tags:
  - hacking
---

## CORS
Before olden days where browser allow send request from same origin to different origin with creds which is bad and they introduces CORS

 It describes a policy for how cross-origin requests can be made and used. It is both incredibly flexible and completely insufficient.

The default policy allows making requests, but you can’t read the results. So `fun-games.example` is blocked from reading your address from `https://your-bank.example/profile`. It can also use side channels such as latency and if the request succeeded or failed to learn things.

But despite being incredibly annoying this doesn’t actually solve the problem! While `fun-games.example` can’t read the result, the request is still sent. This means that it can execute `POST https://your-bank.example/transfer?to=fungames&amount=1000000000` to transfer one billion dollars to their account.

This must be one of the biggest security compromises ever made in the name of backwards compatibility.

The best solution is to set up server-wide middleware that ignores implicit credentials on all cross-origin requests. This example strips cookies, if you use HTTP Authentication or TLS client certificates be sure to ignore those too. Thankfully the [`Sec-Fetch-*` headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-Fetch-Site) are now available on all modern browsers. This makes cross-site requests easy to identify.

```
def no_cross_origin_cookies(req):
	if req.headers["sec-fetch-site"] == "same-origin":
		# Same origin, OK
		return

	if req.headers["sec-fetch-mode"] == "navigate" and req.method == "GET":
		# GET requests shouldn't mutate state so this is safe.
		return

	req.headers.delete("cookie")
```



My Ideas
- when user vistie our website we can user window.open("bank.com?money=ee&hdh&d") that cannot be blocked by CORS are anything