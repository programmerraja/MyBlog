+++
title = 'Nginx Notes'
date  = 2024-01-10T08:19:32.3232+05:30
draft = false
+++

## **How Does NGINX Work?**

NGINX uses a predictable process model that is tuned to the available hardware resources:

- The **master process** performs the privileged operations such as reading configuration and binding to ports, and then creates a small number of child processes (the next three types).
- The **cache loader** process runs at startup to load the disk‑based cache into memory, and then exits. It is scheduled conservatively, so its resource demands are low.
- The **cache manager** process runs periodically and prunes entries from the disk caches to keep them within the configured sizes.
- The **worker processes** do all of the work! They handle network connections, read and write content to disk, and communicate with upstream servers.
- Each request handled by each process


#### Configuration files

#### sites-enaled and sites-avalible

**sites-available**: This directory contains individual configuration files for each website or application that NGINX can serve

**sites-enable**: This directory contains symbolic links (or sometimes actual copies) to the configuration files in the `sites-available` directory.

NGINX only reads configuration files from the `sites-enabled` directory when it starts or reloads. This separation allows administrators to easily enable or disable sites without deleting or moving configuration files.

cd /etc/ngnix → has config file

The way nginx and its modules work is determined in the configuration file. By default, the configuration file is named `nginx.conf` and placed in the directory `/usr/local/nginx/conf`, `/etc/nginx`, or `/usr/local/etc/nginx`.


[http {},events {}] → these are called contexts

```jsx
http {

server {
listen : 8080
root : static path to html 
	location  /path {
	root :satic path to html
}
}
}
```

#### Example

```jsx
worker_processes  5;  ## Default: 1
error_log  logs/error.log;

worker_rlimit_nofile 8192;

events {
  worker_connections  4096;  ## Default: 1024
}

http {
  include    conf/mime.types;
  include    /etc/nginx/proxy.conf;
  include    /etc/nginx/fastcgi.conf;
  index    index.html index.htm index.php;

  default_type application/octet-stream;
  log_format   main '$remote_addr - $remote_user [$time_local]  $status '
    '"$request" $body_bytes_sent "$http_referer" '
    '"$http_user_agent" "$http_x_forwarded_for"';
  access_log   logs/access.log  main;
  sendfile     on;
  tcp_nopush   on;
  server_names_hash_bucket_size 128; # this seems to be required for some vhosts

  server { 
    listen       80;
    server_name  domain1.com www.domain1.com;
    access_log   logs/domain1.access.log  main;
    root         html;

    location ~ \.php$ {
      fastcgi_pass   127.0.0.1:1025;
    }
  }

  server { # simple reverse-proxy
    listen       80;
    server_name  domain2.com www.domain2.com;
    access_log   logs/domain2.access.log  main;

    # serve static files
    location ~ ^/(images|javascript|js|css|flash|media|static)/  {
      root    /var/www/virtual/big.server.com/htdocs;
      expires 30d;
    }

    # pass requests for dynamic content to rails/turbogears/zope, et al
    location / {
      proxy_pass      http://127.0.0.1:8080;
    }
  }

  upstream big_server_com {
    server 127.0.0.3:8000 weight=5;
    server 127.0.0.3:8001 weight=5;
    server 192.168.0.1:8000;
    server 192.168.0.1:8001;
  }

  server { # simple load balancing
    listen          80;
    server_name     big.server.com;
    access_log      logs/big.server.access.log main;

    location / {
      proxy_pass      http://big_server_com;
    }
  }
}
```

- **Context:**  determines the scope and inheritance of directives, helping organize configuration settings and control their application at different levels

```
Main Context
├── Events Context
│   ├── worker_connections
│   ├── use
│   └── multi_accept
├── HTTP Context
│   ├── log_format
│   ├── access_log
│   ├── default_type
│   ├── include
│   └── Server Context
│       ├── listen
│       ├── server_name
│       ├── ssl_certificate
│       ├── location
│       │   ├── root
│       │   ├── proxy_pass
│       │   ├── try_files
│       │   └── rewrite
│       └── Location Context
│           ├── root
│           ├── proxy_pass
│           ├── try_files
│           └── rewrite
└── Stream Context
    ├── log_format
    └── Server Context
        ├── listen
        └── upstream
```

- **worker_processor**: default 1 set auto or set how many cores you have

- **worker_rlimit_nofile**: No of file can be open for connection set` 2* no of worker processor` if you using as reverse proxy set 3* worker because for listen one file and for stroing response from proxy 

- **worker_connections** : total amount of connection can be handle by per worker for per sec so if reach the threashold  it won't handle above the count it adviseable to set `2 * no of cpu` as count if we not using as reverse proxy

#### How nginx processes a request

```yaml
server {
    listen      80 default_server;
    server_name example.org www.example.org;
    ...
}

server {
    listen      80;
    server_name example.net www.example.net;
    ...
}

server {
    listen      80;
    server_name example.com www.example.com;
    ...
}
```

In this configuration nginx tests only the request’s header field “Host” to determine which server the request should be routed to. If its value does not match any server name, or the request does not contain this header field at all, then nginx will route the request to the default server for this port.

#### Reloading server with zero down time

Updating NGINX configuration is a very simple, lightweight, and reliable operation. It typically just means running the `nginx` `-s` `reload` command, which checks the configuration on disk and sends the master process a SIGHUP signal.

When the master process receives a SIGHUP, it does two things:

1. Reloads the configuration and forks a new set of worker processes. These new worker processes immediately begin accepting connections and processing traffic (using the new configuration settings).
2. Signals the old worker processes to gracefully exit. The worker processes stop accepting new connections. As soon as each current HTTP request completes, the worker process cleanly shuts down the connection (that is, there are no lingering keepalives). Once all connections are closed, the worker processes exit.

This reload process can cause a small spike in CPU and memory usage, but it’s generally imperceptible compared to the resource load from active connections

## Logs

#### accesslog 
have all api logs what are the requset came .

```
http {
    ...
    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    
    access_log /var/log/nginx/access.log main;
    ...
}
```

#### Error Logs

```
http {
    ...
    error_log /var/log/nginx/error.log;
    ...
}

```

## Caching

When nginx reads the response from an upstream server, the content is first written to a temporary file outside of the cache directory structure. When nginx finishes processing the request it renames the temporary file and moves it to the cache directory. If the temporary files directory for proxying is on another file system, the file will be copied, thus it's recommended to keep both temporary and cache directories on the same file system. It is also quite safe to delete files from the cache directory structure when they need to be explicitly purged. There are third-party extensions for nginx which make it possible to control cached content remotely, and more work is planned to integrate this functionality in the main distribution.

The `ngx_http_limit_conn_module` module is used to limit the number of connections per the defined key, in particular, the number of connections from a single IP address.


```jsx
http {
	proxy_cache_path /path  (physical loation to live)
	max_size=300g inactive=12
	proxy_cache_key $scheme$proxy_host -> define key for caching we can give cookie any unique key
}
```

cache manager → activated perodically to check the state of cache and clear it based on constraints

Cache loader → runs only after ngnix start it load metadata about perv cached data

## load balancer

To load balance the request

```yaml
http {
    upstream backend_servers {
        server backend1.example.com:8080 weight=20;
        server backend2.example.com:8080;
        server backend3.example.com:8080;
    }

    server {
        listen 80;
        server_name example.com;

        location / {
            proxy_pass http://backend_servers;
        }
    }
}
```



## Socket sharding

When we run a server the ip and port are bind to the socket and if try to bind same ip and port again it will throw error but by using **Socket sharding** we can bind multiple app instance to single socket and kernel will do load balancing for the incoming request.

**Configuring Socket Sharding**

To enable the `SO_REUSEPORT` socket option, include the new `reuseport` parameter to the `listen` directive for HTTP or TCP (**stream** module) traffic,

```yaml
http {
     server {
          listen 80 reuseport;
          server_name  localhost;
          # ...
     }
}

stream {
     server {
          listen 12345 reuseport;
          # ...
     }
}
```




Rate limiting →https://www.nginx.com/blog/rate-limiting-nginx/


## Optimization 

When a client sends an HTTP request to the NGINX server, it typically establishes a TCP connection to send and receive data. This connection can be reused for multiple requests, especially if keepalive connections are enabled. 

**keepalive_requests** and **keepalive_timeout** directives to
alter the number of requests that can be made over a single connec‐
tion and the time idle connections can stay open:

```
http {
	
	keepalive_requests 320;
	keepalive_timeout 300s;

}
```
The keepalive_requests directive defaults to 100, and the
keepalive_timeout directive defaults to 75 seconds.

**Keeping Connections Open Upstream**

When opening connection to upstream server we can set keep live that make connection to open and resuse for further request such that for each request it won't open new connection

```
proxy_http_version 1.1; 
proxy_set_header Connection "" #remove the `Connection` header when forwarding requests to upstream servers. to make connection live

upstream backend {
    server 10.0.0.42;
    server 10.0.2.56;
    keepalive 32; # specifies that NGINX should maintain up to 32 idle connections to each of the upstream servers
}

```

**Response buffering**

When nginx recevied the response that is passed to a client synchronously, immediately as it is received. nginx will not try to read the whole response from the proxied server. we can enable buffering .

nginx receives a response from the proxied server as soon as possible, saving it into the buffers set by the [proxy_buffer_size](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffer_size) and [proxy_buffers](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffers) directives. If the whole response does not fit into memory, a part of it can be saved to a [temporary file](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_temp_path) on the disk.

```
server {
proxy_buffering on;
proxy_buffer_size 8k;
proxy_buffers 8 32k;
proxy_busy_buffer_size 64k;
...
}
```

`Note: **Proxy buffering is enabled by default in NGINX**`

**Buffering Access Logs**

buffer logs to reduce the opportunity of blocks to the NGINX worker process when the system is under load.
```
http {
	access_log /var/log/nginx/access.log main buffer=32k flush=1m;
}
```
## CMDS 

- `ngnix -t `→ to test the config file is everything ok

## Security

This will not send the ngix version
```
http{
# Turn off server tokens 
server_tokens off;
}

```

1. **Prevents MIME type sniffing**: Browsers sometimes try to guess the MIME type of a file based on its content, which can lead to security vulnerabilities. For example, a file with a misleading extension might be interpreted as a different type of file than it actually is, potentially leading to XSS (Cross-Site Scripting) attacks.
    
2. **Forces the browser to respect the declared content type**: By sending the `X-Content-Type-Options: nosniff` header, you're instructing the browser to trust the MIME type provided by the server and not try to infer it.

## Alternative
#### [Envoy](https://www.envoyproxy.io/docs/envoy/latest/)
it is layer 7 proxy and use Yaml extension for config

1. Downstream → request come from [client]
2. Upstream → response come from [server]
3. Cluster → host are called .it has load balancing policy
    1. cluster app1- > have 2 host
    2. cluster app2 → can have 2 host
4. connection pool → each host in cluster gets 1 or more connection pool and connection pools are per worker thread each thread bound to single connection. no cordination between threads

#### Pingora [cloudfare]

## Resources

1. [https://www.nginx.com/blog/inside-nginx-how-we-designed-for-performance-scale/](https://www.nginx.com/blog/inside-nginx-how-we-designed-for-performance-scale/)
2. [https://www.nginx.com/blog/tuning-nginx/](https://www.nginx.com/blog/tuning-nginx/)
3. [https://aosabook.org/en/v2/nginx.html](https://aosabook.org/en/v2/nginx.html)
4. [https://blog.cloudflare.com/how-we-built-pingora-the-proxy-that-connects-cloudflare-to-the-internet/](https://blog.cloudflare.com/how-we-built-pingora-the-proxy-that-connects-cloudflare-to-the-internet/)
5. [https://nginx.org/en/docs/](https://nginx.org/en/docs/)
6. [https://blog.martinfjordvald.com/optimizing-nginx-for-high-traffic-loads/](https://blog.martinfjordvald.com/optimizing-nginx-for-high-traffic-loads/)

## Tool

1. [https://github.com/louislam/uptime-kuma](https://github.com/louislam/uptime-kuma)