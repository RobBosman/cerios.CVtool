# Cerios CVtool dev-env setup
### prepare
download & install [Wnmp 3.2](https://github.com/wnmp/wnmp) in C:\Progs\Wnmp\ \
download & install Flash Builder 4.6 (plus crack)\
download & install NetBeans for PHP\
download & install [HeidiSQL](https://www.heidisql.com/) (portable version is OK)\
GIT-checkout:
* https://github.com/RobBosman/cerios.CVtoolServer-PHP into `${CVTOOL_ROOT}`
* https://github.com/RobBosman/bransom.RestServer-PHP into `${BRANSOM_ROOT}`
* https://github.com/RobBosman/cerios.CVtoolClient-Flex
* https://github.com/RobBosman/bransom.RestClient-Flex
### server
edit the _HTTP Server_ section in file `C:\Progs\Wnmp\conf\nginx.conf`:
* change `listen 80` into `listen 9080`
* replace `|swf)` with `|woff|ttf)` to disable caching of SWF files
* add this to the _HTTP Server_ section:
```
    ## @@@ Cerios.cvtool
    location ~/bransom {
        index bransom.php;
        location ~/REST {
            rewrite ^(.*)$ /bransom/REST/rest.php;
        }
    }
    location ~/cvtool {
        index CVtool.html;
    }
```
edit `C:\Progs\Wnmp\php\php.ini` and replace `;extension=xsl` with `extension=xsl`\
start Wnmp server (Nginx, MariaDB and PHP)
### database
MariaDB (port=3306, name='root', password='' or 'password'):
```
SET GLOBAL NET_READ_TIMEOUT=500;
SET GLOBAL NET_WRITE_TIMEOUT=500;
SET GLOBAL MAX_ALLOWED_PACKET=1073741824;
DROP DATABASE IF EXISTS `deb31080_cv`;
```
extract `${CVTOOL_ROOT}\database\deb31080_cv-*.zip` (password='cvtoolcvtool')\
use HeidiSQL to import `deb31080_cvtool-*.sql`
### server-side IDE
NetBeans: import _bransom.RestServer-PHP_ and _cerios.CVtoolServer-PHP_\
edit Properties/Sources of project _bransom.RestServer-PHP_ to copy files from Sources Folder to `C:\Progs\Wnmp\html\bransom`\
edit Properties/Sources of project _cerios.CVtoolServer-PHP_ to copy files from Sources Folder to `C:\Progs\Wnmp\html\cvtool`
### client-side IDE
FlashBuilder: import _FlexRestClient_ and _CVtoolClient_
### run
copy `C:\Progs\Wnmp\html\bransom\config\TEMPLATE-config.ini` to `config.ini`\
edit `C:\Progs\Wnmp\html\bransom\config\config.ini`:
```
[db]
hostName = localhost
userName = root
password = password
schema-for-app.cvtool = deb31080_cvtool

[xml]
namespaceUri-for-app.cv = http://ns.bransom.nl/cerios/cv/v20110401

[url]
context-root-for-app.cv = cvtool

[auth]
authentication-model-for-app.cv = OpenIDConnect
```
open http://localhost:9080/cvtool
