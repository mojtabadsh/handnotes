# ss5 on CentOS7 

## Install dependency

### Centos

```bash
$ yum install epel-release -y
$ yum groupinstall 'Development Tools' -y
$ yum install gcc automake autoconf libtool make yum-utils wget -y
$ yum install pam-devel openldap-devel openssl-devel -y
```

### Ubuntu

```bash
$ apt-get install gcc pam-dev libpam0g-dev
$ apt-get install libldap2-dev
$ apt-get install openssl
$ apt-get install libssl-dev
$ apt-get install make
```



## Download the source code, compile and install

```bash
$ cd /usr/local/src/
$ wget https://jaist.dl.sourceforge.net/project/ss5/ss5/3.8.9-8/ss5-3.8.9-8.tar.gz
$ tar xzf ss5-3.8.9-8.tar.gz
$ cd ss5-3.8.9
$ ./configure
$ make
$ make install
```



## Modify the configuration file

#### append in end of file in `/etc/opt/ss5/ss5.conf`

```ini
auth  0.0.0.0/0 - u
permit u 0.0.0.0/0 - 0.0.0.0/0  - - - - -
```



## Create user  in `/etc/opt/ss5/ss5.passw`

```ini
user1 password1
user2 password2
```



## customize the proxy port default is 1080

#### method 1 **Edit `/etc/sysconfig/ss5`** 

```ini
SS5_OPTS=" -u root -b 0.0.0.0:10080"
```

#### Method 2: Run SS5 as root and change the port to 10080 (default port is 1080)

Add the following line in `/etc/init.d/ss5`

```ini
export SS5_SOCKS_PORT=10080
export SS5_SOCKS_USER=root
```

### Add executable permissions to the bash file/etc/rc.d/init.d/ss5

```bash
$ chmod 755 /etc/rc.d/init.d/ss5
```

## Systemd Service

```bash
$ systemctl status ss5.service
$ systemctl stop ss5.service
$ systemctl start ss5.service
$ systemctl restart ss5.service
```

## check listen port

```bash
$ netstat -an | grep 10080
```

## View log

more 	`/var/log/ss5/ss5.log`	 Add ss5 to automatic startup (optional)

```bash
$ chkconfig --add ss5
$ chkconfig --level 345 ss5 on
```