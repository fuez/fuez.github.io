---
title: informix
date: 2017-06-13T11:48:45+08:00
lastmod: 2017-06-13T11:48:45+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---


## How to query data on informix mongo collection?
Like this: _select data::json from system_users;_
## [How to check informix sqli trace?](http://www-01.ibm.com/support/docview.wss?uid=swg21104625)
```
export SQLIDEBUG=2:/tmp/sqlitrace
#And then check out trace file with sqliprint like this:
sqliprint /tmp/sqlitrace...
```
## How to check table schema?
```
SELECT tabname, colno, colname  
FROM systables a, syscolumns b 
WHERE a.tabid = b.tabid 
and tabname = "iot_devicedata"
ORDER BY colno;
```

## Useful trouble shooting commands
    + `onstat -osi` : to view summary os basic information related to informix
    + `onstat -g cpu`:Print CPU info for all threads
    + `onstat -g glo`: Print MT global information
    + `onstat -g ath`: Print all threads
    + `onstat -g mem`:Print pool statistics
    + `onstat -g sql`:Print SQL information
    + `onstat -g ses`:Print session information
## How to export current onconfig file in formalized format?
`onmode -we <full path>`, and import by `onmode -wi <full path>`
## [How to kill an outstanding session?](https://www.ibm.com/support/knowledgecenter/SSGU8G_11.70.0/com.ibm.adref.doc/ids_adr_0442.htm)
First run `onstat -u` and find the session held by someone, where session id is just left of user name, and then run `onmode -z <sid>` to close the session.

## How to add a default value for datetime type?
```
ALTER TABLE django_admin_log MODIFY ( action_time datetime year to fraction(5) NOT NULL DEFAULT DATETIME(2016-05-25 00:26:23.00000) year to fraction(5)); 
```
## How to show table schema?
Use _dbschema_ tool, like this:
`dbschema -t abc -d test`

## How to fix storagepool error?
When error happens, storagepool won't work even if you purge errors and add new. I fixed this by restarting primary server, and then add a new storage pool entry. 

## How to connect to remote informix via mongo driver?
like this: `mongo 172.16.2.7:27017/admin -u cdsadmin -p zxcasd123`
Optionally, you can also add _--authenticationDatabase admin_ to specify another auth db other than _admin_

## How to show running service instance?
`onstat -g dis`

## How to uninstall an informix database server?
Refer to [Uninstalling an Informix database server installation (UNIX and Linux)][2]
```
$INFORMIXDIR/uninstall/uninstall_server/uninstallserver -i console
```
## How to stop a server instance?
Refere to [The onclean utility](https://www-01.ibm.com/support/knowledgecenter/SSGU8G_12.1.0/com.ibm.adref.doc/ids_adr_1073.htm)

## How to check configuration?
`oncheck -pe`

## How to get detail of chunk info?
`onstat -d`

## How to check current informix server status?
`onstat -`

## How to create physical log file?
`onspaces -c -P plog -p /opt/informix/storage/plog -o 0 -s 262144`

## Good query tool for informix
check out [RazorSQL](https://razorsql.com/download_mac.html), but only for 30 day trial.

## How to shutdown informix server instance?
`onmode -ky`
if that does not work, then try `onclean`

## How to check informix error code?
type `infxmsg <message Id>` or better `finderr <error code>`

## How to manually remove shared memory?
`ipcs -m | awk '$1 ~ /0x[0-9a-e]/ { print $2}' | xargs -I {} ipcrm -m {}`
## How to manually remove shared semaphores?
`ipcs -s | awk '$1 ~ /0x[0-9a-e]/ { print $2 }' | xargs ipcrm sem`

## How to get out hanging of "onpsm ..." command?
Remove old catalog files in __PSM_CATALOG_PATH__

## Sample iot rest query?
```
curl --user cdsadmin:zxcasd123  http://172.16.2.7:8080/sysmaster/sysconfigs?fields={{cf_name:1,cf_effective:1}}&query={{cf_name:'DBSERVERNAME' }}

curl --user cdsadmin:zxcasd123  http://172.16.2.7:8080/sysmaster/sysconfigs?query={{cf_name:'DBSERVERNAME'}}

```

## How to output query to a file?
`OUTPUT to a.txt SELECT * FROM systables;`


## How to check database locales?
query: `select * from sysmaster:sysdbslocale;`
http://www.querytool.com/help/981.htm

## How to add more chunk to a dbspace from storage pool?
```
EXECUTE FUNCTION task("create chunk from storagepool",
 "space_name", "size",);
```

refer to: http://www.ibm.com/support/knowledgecenter/SSGU8G_12.1.0/com.ibm.admin.doc/ids_admin_1375.htm


### Touble shooting
## Fail to load libifsql.so
```
Mar 29 19:03:01 host-192-168-33-86 collectd[1547]: Unhandled python exception in importing module: ImportError: libifsql.so: cannot open shared object file: No such file or direct
Mar 29 19:03:01 host-192-168-33-86 collectd[1547]: python plugin: Error importing module "informix_mon_collectd".
```
Fix might be to export LD_LIBRARY_PATH: http://www-01.ibm.com/support/docview.wss?uid=swg21154118

## Install informixdb python module.

After install clientsdk3.5, and export INFORMIXDIR path, when run `pip install informixdb`, you will meet the following error on mac:
```
    _informixdb.ec:65:10: fatal error: 'values.h' file not found
    #include <values.h>
             ^
    1 error generated.
    error: command 'clang' failed with exit status 1
```
The root cause and fix is:
```
The standard header file for all this data is limits.h. To fix your problem, you could just create a soft link called values.h that points to limits.h.
```
https://macosx.com/threads/values-h.50056/
And to find the default include path on mac: `echo "" | gcc -xc - -v -E`

Example:
```
sudo ln -sf /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/../lib/clang/7.3.0/include/limits.h  /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/include/values.h
```


### Reference:
## [informix environment variables](https://www.ibm.com/support/knowledgecenter/SSGU8G_12.1.0/com.ibm.sqls.doc/ids_sqs_1140.htm)
## [informix downloads](http://www-01.ibm.com/software/data/informix/downloads.html)
## [client sdk for MacOs](https://www-01.ibm.com/marketing/iwm/tnd/preconfig.jsp?id=2009-05-15+15%3A35%3A49.289783R&S_TACT=&S_CMP=)

Note: Install with `-javahome your_JRE_path (such as /Library/Java/Home for mac)` to fix issue: `could not load wizard specified in /wizard.inf 104`

After installation, add INFORMIXDIR environment variable to bash profile:
`export INFORMIXDIR='/usr/local/clientsdk3.5'` where /usr/local/clientsdk3.5 is the production root you chosed, and optionally also add $INFORMIXDIR/bin to system path
## [Expand transaction capabilities with savepoints in Informix Dynamic Server (IDS)] (http://www.ibm.com/developerworks/data/library/techarticle/dm-0903idssavepoints/)

## [Upgrade Informix high availability clusters online](http://www.ibm.com/developerworks/data/library/techarticle/dm-1012rollingupgrade/)

## [onstat - Server Status Line](http://www.oninit.com/onstat/)

## [International Informix Users Group][1]

## [informixdb python module manual](http://informixdb.sourceforge.net/manual.html)
## [Exploring the Sysmaster Database](http://www.informix.com.ua/articles/sysmast/sysmast.htm)

## [informix data types](http://wiki.ispirer.com/sqlways/informix/data-types)
## [Configuring connectivity between Informix database servers and IBM Data Server clients](https://www.ibm.com/support/knowledgecenter/SSGU8G_12.1.0/com.ibm.admin.doc/ids_admin_0207.htm)
Eanble DRDA by adding line like the following to sqlhosts file:
```
 drda_1        drsoctcp   host_1     drda_port_1
```
and might also need to update onconfig for this, like asked [here](http://members.iiug.org/forums/ids/index.cgi/read/15556)
IBM DB for python is [here](https://github.com/ibmdb/python-ibmdb)
 
[1]:http://www.iiug.org/forums/ids/index.cgi/read/30940
[2]:https://www-01.ibm.com/support/knowledgecenter/SSGU8G_11.70.0/com.ibm.igul.doc/ids_in_089x.htm
