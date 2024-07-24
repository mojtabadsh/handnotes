
## How to add Lua and [Heath Check](https://github.com/yaoweibin/nginx_upstream_check_module) module to Nginx
# **Install Lua**

```
wget http://luajit.org/download/LuaJIT-2.0.5.tar.gz 
tar -zxvf LuaJIT-2.0.5.tar.gz
cd LuaJIT-2.0.5
make && make install PREFIX=/usr/local/LuaJIT
```

## **Add to /etc/profile**

```
export LUAJIT_LIB=/usr/local/LuaJIT/lib 
export LUAJIT_INC=/usr/local/LuaJIT/include/luajit-2.0
source /etc/profile
```


## **Download NGX_ devel_ Kit module**

```
wget https://github.com/simpl/ngx_devel_kit/archive/v0.3.0.tar.gz
```
NDK ([nginx](https://developpaper.com/tag/nginx/ "View all posts in nginx") development kit) module is a module to expand the core functions of nginx server, and the third-party module development can be implemented quickly based on it. NDK provides functions and macros to handle some basic tasks to reduce the amount of code developed by third-party modules

## **Download Lua Nginx module module**
```
wget https://github.com/openresty/lua-nginx-module/archive/v0.10.9rc7.tar.gz 
```
Lua nginx module module enables Lua to run directly in nginx


## Install Heath Check Module
```
wget http://github.com/yaoweibin/nginx_upstream_check_module
```
## Install Nginx 
```
 wget http//nginx.org/download/nginx-1.20.1.tar.gz
tar -xzvf nginx-1.20.1.tar.gz
cd nginx-1.20.1/
patch -p1  < /your/path/nginx_upstream_check_module/check_1.20.1+.patch 
```


## Enter nginx original directory:
set your Nginx and modules path
```
./configure --user=www --group=www --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module --with-http_gzip_static_module --with-http_sub_module --with-http_v2_module --add-module=/your/path/lua-nginx-module-0.10.9rc7 --add-module=/your/path/ngx_devel_kit-0.3.0 --add-module=/your/path/nginx_upstream_check_module
```
### Next Step
```
make
```
## The compilation error should be that the Lua environment variable is not correct.

```
solve:
echo "/usr/local/LuaJIT/lib" >> /etc/ld.so.conf
ldconfig
```
## Last Step
```
make install

If Nginx is installed, run `make upgrade` 
```

## Lua Test

```
server {
    location /lua {
        default_type 'text/html';
        content_by_lua '
        ngx.say("hello, lua!")';
    }
}
```

# Health Check Test 
```
http {
        upstream cluster {
            # simple round-robin
            server 192.168.0.1:80;
            server 192.168.0.2:80;

            check interval=5000 rise=1 fall=3 timeout=4000;

            #check interval=3000 rise=2 fall=5 timeout=1000 type=ssl_hello;

            #check interval=3000 rise=2 fall=5 timeout=1000 type=http;
            #check_http_send "HEAD / HTTP/1.0\r\n\r\n";
            #check_http_expect_alive http_2xx http_3xx;
        }

        server {
            listen 80;
            location / {
                proxy_pass http://cluster;
            }
        }
    }

