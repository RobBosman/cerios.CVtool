# CVtool dev-env setup
download & install [WampServer 2.x](http://www.wampserver.com/en/) in C:\Progs\Wamp\ \
download & install Flash Builder 4.x\
download & install Flash player (debug version)\
download & install NetBeans for PHP\
download & install MariaDB\
download & install HeidiSQL\
download & install FileZilla Client

## server:
GIT-checkout https://github.com/RobBosman/cerios.CVtoolServer-PHP into `${CVTOOL_ROOT}`\
GIT-checkout https://github.com/RobBosman/bransom.RestServer-PHP into `${BRANSOM_ROOT}`\
NetBeans: import "BransomRestServer", "CVtoolServer"\
edit `C:\Progs\Wamp\bin\apache\Apache2.2.17\conf\httpd.conf`:
* "Listen 80" => "Listen 9080"
* "ServerName localhost:80" => "ServerName localhost:9080"

start WampServer

## FLEX client:
GIT-checkout https://github.com/RobBosman/cerios.CVtoolClient-Flex\
GIT-checkout  https://github.com/RobBosman/bransom.RestClient-Flex\
FlashBuilder: import "FlexRestClient", "CVtoolClient"

* copy `${BRANSOM_ROOT}\IDE\BransomRestServer\REST\.htpassword` to `C:\Progs\Wamp\www\bransom\REST\`
* copy `${BRANSOM_ROOT}\IDE\BransomRestServer\*.*` to `C:\Progs\Wamp\www\bransom\`
* copy `${CVTOOL_ROOT}\IDE\CVtoolServer\*.*` to `C:\Progs\Wamp\www\cvtool\`

copy `C:\Progs\Wamp\www\bransom\config\TEMPLATE-config.ini` to `config.ini`\
edit `C:\Progs\Wamp\www\bransom\config\config.ini`:
- hostName = localhost
- userName = root
- password = 
- schema-for-app.cvtool = deb31080_cvtool

WampServer: enable PHP extensions "php_curl", "php_xsl"\
WampServer: enable Apache modules "headers_module", "rewrite_module"\
open URL http://localhost:9080/bransom
* edit httpd.conf: "Allow from 127.0.0.1" => "Allow from sop-bosmar-1.kadaster.local"
* open http://sop-bosmar-1.kadaster.local:9080/bransom/index.php

extract `${CVTOOL_ROOT}\database\deb31080_cvtool-*.zip`\
MariaDB:
* create empty schema "deb31080_cvtool" (or drop all existing tables)
* SET GLOBAL NET_WRITE_TIMEOUT=500;
* SET GLOBAL MAX_ALLOWED_PACKET=1073741824;
* run deb31080_cvtool-*.sql

open http://localhost:9080/cvtool\
FileZilla: cerios.nl deb31080 /domains/ita.cerios.nl/private_html/
