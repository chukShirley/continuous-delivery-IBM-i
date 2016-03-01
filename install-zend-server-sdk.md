## Install Zend Server SDK

[Zs-client](https://github.com/zend-patterns/ZendServerSDK) is a tool that enables developers to easily interact with the Zend Server API.

Installing and Configuring Zs-client (for deployment)
Add Web API key for deployment
Zend Server web admin -> Administration -> Web API
Click “Add Key” button
Key Name: deployment
Key Owner: developer
If wget isn’t available on IBM i
On local PC:
Download the phar from https://github.com/zend-patterns/ZendServerSDK/raw/master/bin/zs-client.phar
Transfer zs-client.phar to IBM i
cd /path/to/downloads
ftp [server ip]
cd /usr/local/zendsvr6/bin
put zs-client.phar
ssh to IBM i
ssh user@sbcint.sk.bluecross.ca
Navigate to Zend Server 6 binaries directory
cd /usr/local/zendsvr6/bin
Add target
php-cli zs-client addTarget --target="sbcint-zend-server" --zskey="deployment" --zssecret="db60aa680ad2fd36a200c2436216ee8f864808830c4cfa3cd62fb18362ac0e79" --zsurl="http://127.0.0.1:10081"
Problem: This doesn’t create /home/user/.zsapi.ini, seems to work if you manually create /home/user/.zsapi.ini first.
