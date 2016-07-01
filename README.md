# 我的开发环境编译参数
记录我使用的nginx、php、mysql运行时配置

### 下载文件
```bash
wget http://cdn.endaosi.com/installer/nginx-1.9.3.tar.gz
wget http://cdn.endaosi.com/installer/mysql-5.6.26.tar.gz
wget http://cdn.endaosi.com/installer/php-5.6.12.tar.gz

```

### 基础操作
 - 新建用户
```bash
sudo useradd -s /sbin/nologin -M www
sudo useradd -s /sbin/nologin -M mysql
```
 - 包
```bash

sudo yum -y install epel-release

sudo yum -y install \
wget curl curl-devel pcre-devel zip unzip \
file flex freetype-devel gmp-devel icu kernel-devel \
gcc gcc-c++ make cmake cpp autoconf automake \
gd glibc-devel glib2-devel gettext-devel bison bzip2 bzip2-devel \
libmcrypt-devel libtool-libs libjpeg-devel libpng-devel libxslt \
libxslt-devel libxml2 libxml2-devel libidn-devel libcap-devel \
libtool-ltdl-devel libc-client-devel libicu libicu-devel lynx \
zlib-devel crontabs diffutils elinks e2fsprogs-devel expat-devel \
libaio patch mlocate ncurses-devel readline readline-devel vim-minimal \
sendmail pam-devel pcre pcre-devel openldap openldap-devel openssl \
openssl-devel perl-DBD-MySQL

```

### php5.6配置

```bash
./configure  \
--prefix=/usr/local/php-5.6 \
--enable-fpm \
--with-config-file-path=/usr/local/php-5.6/etc \
--disable-fileinfo \
--with-mhash \
--with-curl \
--with-mcrypt \
--with-openssl \
--with-pdo-mysql \
--with-mysql \
--with-mysqli \
--enable-json \
--enable-mbstring \
--enable-pcntl \
--enable-session \
--enable-sockets \
--enable-tokenizer \
--enable-zip \
--with-gd \
--with-zlib-dir \
 --with-png-dir \
 --with-jpeg-dir \
--with-freetype-dir=/usr/ \
 --with-gettext
 
make

sudo make install

#设置全局变量
sudo ln -s /usr/local/php-5.6/bin/php /usr/local/bin/php
```

内存小于256M的话加上
```bash
--disable-fileinfo
```
内存更小的话使用yum二进制安装
```bash

#Centos 5.X
rpm -Uvh http://mirror.webtatic.com/yum/el5/latest.rpm
#CentOs 6.x
rpm -Uvh http://mirror.webtatic.com/yum/el6/latest.rpm
#CentOs 7.X
rpm -Uvh https://mirror.webtatic.com/yum/el7/epel-release.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

yum install epel-release
yum install libmcrypt libmcrypt-devel mcrypt mhash
yum install php56w php56w-cli php56w-common php56w-gd php56w-ldap php56w-mbstring php56w-mcrypt php56w-xmlwriter php56w-mysql php56w-pdo php56w-pear php56w-intl php56w-devel
```


### Mysql配置
```bash
cmake \
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DMYSQL_DATADIR=/usr/local/mysql/data \
-DMYSQL_UNIX_ADDR=/tmp/mysql.sock \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci \
-DWITH_EXTRA_CHARSETS=complex \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_READLINE=1 \
-DENABLED_LOCAL_INFILE=1 \
-DWITH_PARTITION_STORAGE_ENGINE=1 \
-DWITH_FEDERATED_STORAGE_ENGINE=1 \
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \
-DWITH_MYISAM_STORAGE_ENGINE=1 \
-DWITH_EMBEDDED_SERVER=1

make

sudo make install

```
导入数据库

```bash
sudo /usr/local/mysql/scripts/mysql_install_db --defaults-file=/etc/my.cnf --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data --user=mysql
```
配置mysql root用户的密码为'root'
```bash
sudo /usr/local/mysql/bin/mysql -e "grant all privileges on *.* to root@'localhost' identified by \"root\" with grant option;"
```
### nginx配置
```bash
./configure --with-http_ssl_module
make
sudo make install
```

### composer
```bash
curl -sS https://getcomposer.org/installer | php 
or
php -r "readfile('https://getcomposer.org/installer');" | php
mv composer.phar /usr/local/bin/composer

```
