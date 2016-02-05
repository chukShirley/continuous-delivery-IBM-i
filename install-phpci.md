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
$ use mysql; GRANT ALL PRIVILEGES ON *.* to ‘phpci’@’localhost’ IDENTIFIED by ‘[myPhpCiPass]’ WITH GRANT OPTION; FLUSH PRIVILEGES;
```  
* Ensure all the GitHub ip addresses are reachable from the IBM i server.  
* Ensure port availability for PHPCI from GitHub’s servers  
* Verify that GitHub can make http requests to the following URL:
http://<myServer>:<myVhostPort>/settings/github-callback
Have Git installed (installation tutorial here)
...etc.

Installing PHPCI
Follow instructions here: https://www.phptesting.org/wiki/Installing-PHPCI



Change directories to /www (or wherever else)
cd /www
Run the following commands
php-cli ./console phpci:install


Make sure /www/phpci/PHPCI/config.yml has the proper url configured:
phpci:
    url: 'http://[myServerName]:[myPortNumber]'

### Start the phpci daemon  
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
* Delete the phpci user profile in ZendDBi
* Remove the Apache virtual host from configuration
