# MySQL REST #

## Description ##
Consumming REST Web Services with MySQL 8 in Ubuntu 20.04.

## Step by step ##
In first step install MySQL Server.

#### 1.- Install libmysqlclient-dev ####
~~~
apt-get install libmysqlclient-dev
~~~

#### 2.- Install build-essential ####
~~~
apt-get install build-essential
~~~

#### 3.- Compile curl-7.21.0 ####
~~~
a) wget https://curl.se/download/archeology/curl-7.21.0.tar.gz
b) tar xvzf curl-7.21.0.tar.gz
c) cd curl-7.21.0
d) ./configure --prefix=/usr
e) make && make install
~~~

#### 4.- Compile mysql-udf-http-1.0 ####
~~~
a) wget https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/mysql-udf-http/mysql-udf-http-1.0.tar.gz
b) tar xvzf mysql-udf-http-1.0.tar.gz
c) cd mysql-udf-http-1.0
d) ./configure --prefix=/usr/local/mysql-udf-http --with-mysql=/usr/bin/mysql_config
e) nano src/mysql-udf-http.h
f) Add this lines at begin file:
	#include <stdbool.h>
	typedef bool my_bool;
g) make && make install
h) Connect to MySQL and gets plugin directory:
	show variables where variable_name like '%plugin%'
i) Copy library to plugin directory
cp /usr/local/mysql-udf-http/lib/mysql-udf-http.so.0.0.0 /usr/lib/mysql/plugin/mysql-udf-http.so
j) Restart MySQL:
	service mysql restart
~~~

#### 5.- Connect to MySQL and create functions ####
~~~
CREATE FUNCTION http_get returns string soname 'mysql-udf-http.so';

CREATE FUNCTION http_post returns string soname 'mysql-udf-http.so';

CREATE FUNCTION http_put returns string soname 'mysql-udf-http.so';

CREATE FUNCTION http_delete returns string soname 'mysql-udf-http.so';
~~~

#### 6.- Test MySQL functions ####
~~~
For test GET:
	SELECT http_get('https://checkip.amazonaws.com/') AS RESPONSE;

For test POST:
	SELECT http_post('https://httpbin.org/post', CONCAT('param=', UUID())) AS RESPONSE;
~~~

## Notes ##
Tested on Ubuntu 20.04 with MySQL 5.7 and MySQL 8.

P.D. Let's go play !!!
