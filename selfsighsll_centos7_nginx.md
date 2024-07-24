# Create a Self-Signed SSL Certificate for Nginx on CentOS 7

## Step 1 — Create the SSL Certificate

```bash
$ sudo mkdir /etc/ssl/private
$ sudo chmod 700 /etc/ssl/private
```

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/private/nginx-selfsigned.crt


Output
Country Name (2 letter code) [XX]:US
State or Province Name (full name) []:Example
Locality Name (eg, city) [Default City]:Example 
Organization Name (eg, company) [Default Company Ltd]:Example Inc
Organizational Unit Name (eg, section) []:Example Dept
Common Name (eg, your name or your server's hostname) []:your_domain_or_ip
Email Address []:webmaster@example.com
```

Both of the files you created will be placed in the appropriate subdirectories of the `/etc/ssl` directory.

As you are using OpenSSL, you should also create a strong Diffie-Hellman group, which is used in negotiating [Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) with clients.

You can do this by typing:

```bash
$ openssl dhparam -out /etc/ssl/private/dhparam.pem 2048
```

## Step 2 — Configure Nginx to Use SSL

```bash
$ vim /etc/nginx/conf.d/ssl.conf

server {
    listen 443 http2 ssl;
    listen [::]:443 http2 ssl;

    server_name backoffice-test.datx.ir;

    ssl_certificate /etc/ssl/private/nginx-selfsigned.crt;
    ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;
    ssl_dhparam /etc/ssl/private/dhparam.pem;
}
```



Refrence

https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-on-centos-7