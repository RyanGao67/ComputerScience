# Install Nignx
```
apt-get update
```

```
// go to nginx.org for nginx
// go to nginx.com for paid product

wget source_code_version_in_nginx_org
tar -xzf source_code_version_in_nginx_org
```

Then we need to configure our source code for the build. 
```
apt-get install build-essential
```

dependencies
```
apt-get install libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev 
```

```
./configure
```

Check this page to see how to build from source
[http://nginx.org/en/docs/configure.html](http://nginx.org/en/docs/configure.html)


```
./configure \
--sbin-path=/usr/bin/nginx \
--conf-path=/etc/nginx/nginx.conf \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-pcre \
--pid-path=/var/run/nginx.pid \
--with-http_ssl_module
```

```
make
```

Install the compiled code
```
make install 
```


Check the installed nginx
```
ls -l /etc/nginx
nginx -V 
```

Start nginx 
```
nginx
```

```
ps aux | grep nginx
```

Now you can go to the page to view nginx. 

# systemd
```
ubuntu@ip-172-31-15-149:~/nginx-1.19.8$ cat /lib/systemd/system/nginx.service
[Unit]
Description=The NGINX HTTP and reverse proxy server
After=syslog.target network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
Type=forking
PIDFile=/var/run/nginx.pid
ExecStartPre=/usr/bin/nginx -t
ExecStart=/usr/bin/nginx
ExecReload=/usr/bin/nginx -s reload
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target

```

You may need :
```
sudo pkill nginx && sudo service nginx restart
```

# Term 
1. Context 
2. Directive

# Location
Location priority
1. Exact Match = uri
2. Preferential Prefix Match ^~ uri
3. REGEX Match ~* uri
4. Prefix Match uri

# List all services
```
systemctl list-units
```


# Example nginx conf

```
#user  nobody;
#worker_processes  1;
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;
#pid        logs/nginx.pid;
events {
    worker_connections  1024;
}
http {
#    include       mime.types;
#    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

#    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
#    keepalive_timeout  65;

    #gzip  on;
    include mime.types;
    server {
      listen 80;
      server_name 99.79.67.59;
      root /sites/demo;

#     rewrite ^/user/\w+ /greet;  
#     compare rewrite with redirect
#     Another important and powerful feature of rewrites is the ability to capture certain parts of the original uri. Using standard regular expression capture groups  
#     rewrite ^/user/(\w+)/(\d+) /greet/$1 $2
#     rewrite .... last (last means this is the last time rewrite)
#
#
#     try_files /cat.png /greet;  (understand try_file, the last parameter is evaluated with rewrite)

      set $weekend 'No';
      if ( $date_local ~ 'Saturday|Sunday' ){
          set $weekend 'Yes';
      }
#      if ( $arg_apikey != 1234 ) {
#          return 401 "incorrect api key";
#      }


      # Exact match
      location /greet {
        return 200 'hello from NGINX "/greet" location.';
      }
      # REGEX match - case sensitive
      # location ~ /greet[0-9] {
      #     return 200 'hello from NGINX "/greet" location - REGEX MATCH.';
      # }

      # REGEX match -case insensitive
      # location ^* /greet[0-9] {
      #     return 200 'hello from NGINX "/greet" location - REGEX MATCH INSENSITIVE.';
      #     }

      location /inspect {
        return 200 '$host\n$uri\n$args\nname: $arg_name';
      }

      location /is_weekend {
        return 200 '$weekend';
      }

      location /logo {
        return 307 /thumb.png;
      }
    }
}

```
