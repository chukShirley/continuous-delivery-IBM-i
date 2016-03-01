## Install and Configuring PHPCI

* Note: Have your MySQL admin credentials handy. Default values are:  
    username: root  
    password: (blank)  
You can set the password by following [these instructions](http://rodflohr.com/set-the-zenddbi-mysql-root-user-password)

### Installation  
* Create database and user profile for phpci in ZendDBi      
```sh
$ /usr/local/mysql/bin/mysql -u root -p[myRootPass]
$ CREATE DATABASE phpci;
$ use mysql; GRANT ALL PRIVILEGES ON *.* to ‘phpci’@’127.0.0.1’ IDENTIFIED by ‘[myPhpCiPass]’ WITH GRANT OPTION; FLUSH PRIVILEGES;
```  
* Ensure all the GitHub IP addresses are reachable from the IBM i server.  
* Ensure port availability for PHPCI from GitHub’s servers  
* Ensure that Git is installed on your server 

PHPCI installation documentation is [here](https://www.phptesting.org/wiki/Installing-PHPCI), but you can follow these steps:  

Change directories to /www (or wherever else you'd like to install PHPCI)
```sh
$ cd /www
```  
  
Clone the phpci repository
```sh
$ git clone https://github.com/Block8/PHPCI.git
```
** Note: You may receive an error here stating **
```
fatal: unable to access 'https://github.com/Block8/PHPCI.git/': SSL certificate problem: unable to get local issuer certificate
```
This error means that Git doesn't know where to find the local certificate authority. You can resolve this problem by
downloading a certificate [as instructed here](setting-up-ssl-peer-certificates.md) and then changing the git ssl
setting to point to the newly downloaded certificate
```sh
$ git config --global http.sslCAInfo /phpsslcert/cert.pem
```
After changing this setting you should be able to run the above `git clone` command successfully.
  
Navigate into the project directory
```sh
$ cd PHPCI
```
  
Install Composer dependencies
```sh
$ composer install
```
  
Run the phpci installer
** Note: This part cannot be executed from `QP2TERM` or `QSH`. You must use an ssh connection from your local terminal 
```sh
$ php-cli ./console phpci:install
```  

Make sure /www/phpci/PHPCI/config.yml has the proper url configured:  
```
phpci:
    url: 'http://<myServerName>:<myPortNumber>'
```  

Verify that GitHub can make http requests to the following URL:
http://<myServer>:<myVhostPort>/settings/github-callback

### Start the phpci daemon  
In order to access the phpci admin web interface you must ensure that the phpci daemon is running
* [Enter PASE environment](enter-pase-environment.md)  
* Navigate to phpci directory  
```sh
$ cd /www/phpci
```  
* Start the daemon  
```sh
$ nohup php-cli ./daemonise phpci:daemonise >/dev/null 2>&1  &
```
    

### Updating PHPCI
* [Enter PASE](enter-pase-environment.md)  
* Navigate to PHPCI directory  
```sh
$ cd /www/phpci
```  
* Kill the build process  
```sh
php-cli ./console phpci:daemon stop
```  
* Make sure composer.phar is in $PATH  
```sh
$ composer -h
```  
* Run Composer update command
```sh
$ composer update
```


### PHPCI Daemon Commands
* [Enter PASE](enter-pase-environment.md)  
* Navigate to phpci root directory  
```sh
$ ssh chuk@sbcint.sask.bluecross.ca
$ cd /www/phpci
```  
* Start daemon and send output to shell    
```sh
$ php-cli daemonise phpci:daemonise
```  
* Start daemon with “no hangup”  
```sh
$ nohup php-cli ./daemonise phpci:daemonise >/dev/null 2>&1  &
```  
* Kill “no hangup” process  
```sh
$ php-cli ./console phpci:daemon stop
```


### Removing PHPCI  
* [Enter PASE](enter-pase-environment.md)    
* Delete the phpci project directory  
```sh
$ rm -r /www/phpci
```  
* Delete the phpci database in ZendDBi
```sh
$ DROP DATABASE phpci
```
* Delete the phpci user profile in ZendDBi
```sh
$ DROP USER 'phpci'@'localhost';
```
* Remove the Apache virtual host from configuration (found in httpd.conf)
