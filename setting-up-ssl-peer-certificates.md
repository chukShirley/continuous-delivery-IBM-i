1. [Enter PASE environment](enter-pase-environment.md)
2. Create directory to contain SSL peer certificate (e.g., "phpsslcert")

    ```bash
    $ mkdir /phpsslcert
    ```

3. Download ssl certificate into that directory
    ```bash
    $ php -r "readfile('http://curl.haxx.se/ca/cacert.pem');" > /phpsslcert/cert.pem
    ```

4. Point php.ini sslcafile to new certificate
    From IBM i command line:
    ```
    EDTF STMF('/usr/local/zendsvr6/etc/php.ini')
    ```