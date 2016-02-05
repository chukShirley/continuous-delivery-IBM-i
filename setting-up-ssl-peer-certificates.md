* [Enter PASE environment](enter-pase-environment.md)
* Create directory to contain SSL peer certificate (e.g., "phpsslcert")
```sh
$ mkdir /phpsslcert
```
* Download ssl certificate into that directory
```sh
$ php -r "readfile('http://curl.haxx.se/ca/cacert.pem');" > /phpsslcert/cert.pem
```
* Point php.ini sslcafile to new certificate
From IBM i command line:
```
EDTF STMF('/usr/local/zendsvr6/etc/php.ini')
    ```