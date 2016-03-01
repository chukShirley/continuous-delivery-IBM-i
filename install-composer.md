## Install Composer

* [Enter PASE environment](enter-pase-environment.md)  
* Navigate to installation destination. I have chosen `/usr/local/zendsvr6/bin` here, but you could just as easily use `/usr/bin` or the like.  
```sh
$ cd /usr/local/zendsvr6/bin
```  
* Run installation script  
```sh
$ php-cli -r “readfile('https://getcomposer.org/installer');” | php-cli
```  
__Note: If running this statement produces an OpenSSL error message, follow [these instructions](setting-up-ssl-peer-certificates.md)__  
* For future convenience you can rename composer.phar to composer (optional)  
```sh
$ mv /usr/local/zendsvr6/bin/composer.phar /usr/local/zendsvr6/bin/composer
```  

Composer uses https or ssh to clone a project’s dependencies. Https is faster since it downloads a zipped copy of the repository, 
but if you want Composer to use https you have to generate a GitHub token as described here.  
* [Generate a personal access token on GitHub](generate-github-access-token.md)    
* [Enter PASE](enter-pase-environment.md)  
* Add the token to the composer configuration
```sh
$ composer config -g github-oauth.github.com <oauthtoken>
```


## Removing Composer
Should you wish to remove Composer you can do so by simply deleting the phar file
```sh
$ rm /usr/local/zendsvr6/bin/composer
```