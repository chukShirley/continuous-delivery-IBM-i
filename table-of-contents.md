## Topics covered

* [Introduction](introduction.md)
* Continuous Delivery concepts  
* Tools used
    * [Zend Server with ZendDBi](http://www.zend.com/en/products/server/downloads#IBM%20i)
    * [Composer](http://getcomposer.org)
    * [phpci](https://www.phptesting.org)
    * [Git](https://git-scm.com) / [GitHub](https://github.com)
    * 5250 session or SSH connection to IBM i
* [Step-by-step guide](step-by-step-guide.md)

### Prerequisites
Minimum PHP version: 5.3.8
Zend Server
ZendDBi



Configuring your project/build
Access PHPCI admin UI in your web browser:
http://[myServerName]:[myPortNumber]
It will redirect you to:
http://[myServerName]:[myPortNumber]/session/login
Enter the credentials that you created when you ran the “php ./console phpci:install” command
Click AdminOptions->Add Project
Fill out the form and click “Save Project”
Example:
	Where is your project hosted?
		GitHub
	Repository Name/URL(Remote) or Path(Local)
		https://github.com/zendtech/IbmiToolkit
	Project Title
		IBM i Toolkit
	PHPCI build config for this project
		phpci.yml
	Default branch name
		master
Add phpci.yml to the root of your project, and format it like this:
https://www.phptesting.org/wiki/Adding-PHPCI-Support-to-Your-Projects

	


Remove PHPCI
Enter PASE via QP2TERM or ssh
CALL QP2TERM
ssh sbcint.sk.bluecross.ca
Delete the /www/phpci directory
rm -r /www/phpci
Delete the phpci database in ZendDBi
Delete the phpci user profile in ZendDBi
Remove the Apache virtual host from configuration





Deployment
Run ./build/deploy.sh
This script checks if the GitHub repository contains any changes since the last deployment. If so it runs a Phing build, which is defined in ./phing-deploy.xml.


Setting up connection from GitHub to IBM i
If you would like for your builds to be triggered automatically when pull requests are created on GitHub then you’ll need to grant GitHub’s ip addresses access to PHPCI running on your server. The IP address range that GitHub uses (and thus the ones you’ll want to whitelist) are 192.30.252.0/22 (source: https://help.github.com/articles/what-ip-addresses-does-github-use-that-i-should-whitelist/). 

Set up a cron job to do a git pull. If the content changes, that would trigger Phing to carry out whatever deployment tasks.
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


Enabling Journaling for Transactions for Doctrine Migration Tests
If you are running Doctrine tests and migrations that utilize transactions it will be necessary to enable commitment control and journaling to support this on the library where tables are to be created.

To start journaling and automatically apply journaling to any object created in the library/schema execute the following commands:

Create the Journal Library:

CRTLIB LIB(OCAPCIJRN) TEXT('OCAP CI Journal Library')

Create the Journal Receiver object:

CRTJRNRCV JRNRCV(OCAPCIJRN/JRNL) Text('Journal Receiver for OCAP CI Build Lib SBC_OCAPCI')

Create the Journal object:

CRTJRN JRN(OCAPCIJRN/JRNL) JRNRCV(OCAPCIJRN/JRNL) Text('Journal for OCAP CI Build Lib SBC_OCAPCI')

Start Journaling and apply the automatic journaling for objects created in the CI library:

STRJRNLIB LIB(SBC_OCAPCI) JRN(OCAPCIJRN/JRNL) INHRULES((*FILE *ALLOPR *INCLUDE *AFTER *NONE) (*DTAARA *CREATE)) 

Troubleshooting Other Errors

Error
Solution
An exception occurred while executing 'ALTER TABLE FORM_SCREEN DROP
COLUMN code': SQLSTATE=57014 SQLCODE=-952 Processing of the SQL statement ended. Reason code 10.
The recommendation is to change the way the message is replied automatically. A couple of ways to do this. As we discussed on the phone here are the steps for the solution we're proposing for the SBCINT system:

- COPY the QDFTJOBD to a new one called WEBUSERJD Job Description. Using WRKJOBD select QDFTJOBD with option 3 and specify the name of the new job description to be created from it.

- Change the new WEBUSRJD Job Description to use *SYSRPYL for the. Using CHGJOBD edit the value of the parameter. INQMSGRPY(*SYSRPYL)

- Change the WEBUSER to use a new WEBUSERJD Job Description

- Add the CPA32B2 message code with an automatic reply of 'I'. Execute the the ADDRPYLE command: ADDRPYLE  SEQNBR(1)  MSGID(CPA32B2)  RPY('I')



















Notes
Include screenshots, etc. in final document
