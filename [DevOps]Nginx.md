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

