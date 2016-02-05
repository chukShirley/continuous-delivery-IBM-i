## Add Zend Server Binaries Directory to PATH  

The include path for your user profile is established when you log in to PASE. 
At that moment the shell looks for a file called .profile in /home/<myUserName>. If it exists then it runs the commands contained within. 
* Navigate to your user profile's home directory
```sh
$ cd /home/<myUserName>
```
* Create .profile with contents
```sh
$ echo 'PATH=$PATH:/usr/local/zendsvr6/bin\n export PATH' > .profile
```