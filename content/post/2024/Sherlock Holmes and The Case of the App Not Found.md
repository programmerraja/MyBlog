+++
title = 'Sherlock Holmes and The Case of the App Not Found'
date = 2024-09-14T09:08:15.1515+05:30
draft = true
tags =[]
+++ 

Welcome to our Sherlock Holmes-inspired tech adventure Series! Imagine each technical challenge as a thrilling mystery waiting to be solved. Like Sherlock Holmes with his sharp eye for detail, I'll tackle the problem with wit and precision. Let's dive in and crack these cases together!

### Setting Up Nginx as a Proxy

We recently worked on proxying requests to our Heroku app through Nginx. The Nginx configuration was set up as follows:

```
server {
    listen 80;
    server_name ourdomain;

    location / {
        proxy_pass https://heroku-app.herokuapp.com;
        
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

After configuring Nginx, we restarted the server and updated the IP address in the domain provider. However, when we tried to access our website using the custom domain, we encountered a "Not Found" page.

## Investigating the Issue

At this point, the debugging process began. First, I reviewed the Nginx configuration by inspecting the file to ensure that there were no errors, but everything appeared to be correct.

I started to suspect that the issue might be related to whether the correct configuration was being applied to our domain, especially since we had multiple configurations. To confirm, I changed the `proxy_pass` directive to point to Google's homepage. After making this change, when I accessed our domain, it successfully redirected to Google, confirming that the correct configuration was being used.

Next, I tested accessing the Heroku app URL directly to verify whether the app itself was working, and it loaded without any issues.

## The Breakthrough

With no other options, I decided to go over the Nginx configuration again. Upon reviewing the file, I had an important realization: the issue was with the `Host` 
header configuration. Here’s the problematic line:

```
proxy_set_header Host $host;
```

If you've used Heroku before, you may know that when you create a new app, Heroku automatically assigns it a subdomain like `appname.herokuapp.com`. But how does Heroku route requests to the correct app when you hit a `*.herokuapp.com` URL?

Heroku uses the `Host` header to determine which app the request should be routed to. If the `Host` value does not match any Heroku app or if it's missing altogether, you'll receive an "App Not Found" error.

In our case, the `Host` header was set to `ourdomain`, which meant that when the Heroku router tried to forward the request, it looked for an app associated with `ourdomain`—which obviously doesn’t exist. That's why we were seeing the "App Not Found" error.

## The Fix

To resolve the issue, the `Host` header should be set to the Heroku app's subdomain (`appname.herokuapp.com`) instead of the custom domain. Here's the corrected line:  

```
proxy_set_header Host heroku-app.herokuapp.com;
```

By explicitly setting the `Host` header to the correct value, Heroku can route the request to the appropriate app, and everything works as expected.

Stay tuned for our next adventure, where we continue to unravel the mysteries of the infrastructure world, one case at a time. Until then, keep your magnifying glasses handy and your curiosity alive.

**Finally, if the article was helpful, please clap 👏and follow, thank you!**











we worked on proxing the herokuapp request through ngnix where we add a nginx config as below
```
server {
    listen 80;
    server_name ourdomain;

    location / {
        proxy_pass https://heroku-app.herokuapp.com;
        
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

```

After we setup the config we restarted the nignx and we updated the IP in Domain provider when we try to access the website with our domain we get a not found page as below


From this our invesitigation started to debug this i cat the nignix file and checked does i did anything wrong but everything looks good to me. 
i suspect does this config is used for our domain because we have some more config so to confirm that i changed the proxy pass to google such that i know it used this config once i changed the proxy pass and try hitting our domain it perfectly redirected to google so it confirms that it using this config only next i try acessing the heroku app url directly does it serving or not and it works good

i have no other option so i decided to check the config file again and when i am seeing that i got a strike the issue with host header i am putting in the config file
```
 proxy_set_header Host $host;
```

If you use heroku you may noticed when you create a new app it will automatically give you a subdomain as follow `appname.herokuapp.com` and you may wonder how heroku proxy the request to corresponding server when you hit a `appname.herokuapp.com` so basically heroku have a router will be proxy all the `*.herokuapp.com` to corresponding heroku app by using host header 

the request’s header field “Host” to determine which server the request should be routed to. If its value does not match any server name, or the request does not contain this header field at all then it will say app not found so you may get why the it showing app is not found 

because i have set host as ourdomain so when the heroku router try to proxy the request it will check host header it is ourdomain  so heroku router is not able to figure out the heroku app that has a domain ourdomain

