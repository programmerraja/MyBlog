+++
title = 'Sherlock Holmes and the Case of the Broken Website'
date = 2024-07-11T05:15:01.011+05:30
draft = false
tags =['nginx','debugging']
+++ 


Welcome back to our series, where we unravel the mysteries of infrastructure issues with a touch of Sherlock Holmes flair. Today, we tackle a perplexing case involving our trusty web server, Nginx. The problem at hand? A broken website UI after a seemingly simple routing change.

### The Initial Clue: A Broken UI

Our journey began with a routine change. We needed to route requests starting with `/something` to a new server. The Nginx configuration was updated as follows:

```nginx
server {
    location ~ /something/* {
        proxy_pass http://newserver;
    }
}
```

With the change in place, we restarted Nginx and visited our website, only to find the UI in shambles. This was our first clue that something was amiss.

### The Investigation Begins: Network Tab Analysis

Like any good detective, I started my investigation by examining the network tab in the browser's developer tools. It didn't take long to spot the problem: a JavaScript file request was failing with a 404 error. The request path looked something like `/something/.../jsfile`.

### The Suspects: Nginx Configuration and Regex

First, I suspected our Nginx configuration. Was the regex pattern correct? I double-checked it, ensuring it should route requests starting with `/something` to the new server. Everything seemed in order, but the 404 errors persisted. 

Here, I noticed an odd pattern: while some paths starting with `/something` were correctly proxied to the new server, the JavaScript file request was still being served from the default server.

### The Breakthrough: A Clue from Cloudflare

The investigation took a new turn when I remembered that we use Cloudflare. Could it be that Cloudflare was caching the old server's responses? To test this theory, I examined the response headers of the failing requests and found that they were indeed being served from Cloudflare's cache. (`X-Cache: Hit from cloudfront`)

### The Solution: Purging the Cache

With the culprit identified, the solution was straightforward: purge the cache on Cloudflare. Once the cache was cleared, the requests were correctly routed to the new server, and the website UI was restored to its former glory.

### The Conclusion

This case taught us the importance of considering caching layers when troubleshooting routing issues. A seemingly perfect server configuration can be undermined by an overlooked cache.

Stay tuned for the next adventure in our Sherlock Holmes series, where we continue to unearth and solve the mysteries.