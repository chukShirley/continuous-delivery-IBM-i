## Install and Configuring PHPCI

* Note: Have your MySQL admin credentials handy. Default values are:  
    username: root  
    password: (blank)  
You can set the password by following [these instructions](http://rodflohr.com/set-the-zenddbi-mysql-root-user-password)

### Installation  
1. Create database and user profile for phpci in ZendDBi      
    ```bash
    $ /usr/local/mysql/bin/mysql -u root -p[myRootPass]
    $ CREATE DATABASE phpci;
    $ use mysql; GRANT ALL PRIVILEGES ON *.* to ‘phpci’@’localhost’ IDENTIFIED by ‘[myPhpCiPass]’ WITH GRANT OPTION; FLUSH PRIVILEGES;
    ```  
2. Ensure all the GitHub ip addresses are reachable from the IBM i server.  
3. Ensure port availability for PHPCI from GitHub’s servers  
4. Verify that GitHub can make http requests to the following URL:
http://<myServer>:<myVhostPort>/settings/github-callback
Have Git installed (installation tutorial here)
...etc.

Installing PHPCI
Follow instructions here: https://www.phptesting.org/wiki/Installing-PHPCI

SSH into IBM i
Make sure /usr/local/zendsvr6/bin is added to $PATH when connecting
Create /home/[myUserName]/.profile with contents
PATH=$PATH:/usr/local/zendsvr6/bin
export PATH
Change directories to /www (or wherever else)
cd /www
Run the following commands
php-cli ./console phpci:install


Make sure /www/phpci/PHPCI/config.yml has the proper url configured:
phpci:
    url: 'http://[myServerName]:[myPortNumber]'

### Start the phpci daemon  
1. [Enter PASE environment](enter-pase-environment.md)  
2. Navigate to phpci directory  
    ```bash
    $ cd /www/phpci
    ```  
3. Start the daemon  
    ```bash
    $ nohup php-cli ./daemonise phpci:daemonise >/dev/null 2>&1  &
    ```
    
    
    


Updating PHPCI
Enter PASE (via QP2TERM or ssh)
CALL QP2TERM
ssh sbcint.sk.bluecross.ca
Navigate to PHPCI directory
cd /www/phpci
Kill the build process
php-cli ./console phpci:daemon stop
Make sure composer.phar is in $PATH
composer -h
Run Composer update command
composer update


PHPCI Daemon
Start daemon and send output to shell
ssh chuk@sbcint.sask.bluecross.ca
cd /www/phpci
php-cli daemonise phpci:daemonise
Start daemon “no hangup”
nohup php-cli ./daemonise phpci:daemonise >/dev/null 2>&1  &
Kill “no hangup” process
php-cli ./console phpci:daemon stop