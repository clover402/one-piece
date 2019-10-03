## add a new version of php
1. Download the new source of php
2. put the code to /path_to_wamp/bin/php/php-7.2.23(7.2.23 is the new version of php)
3. copy the 3 file from old verion of php folder to  the folder above: php.ini, phpForApache.ini, wampserver.conf
4. update php.ini and phpForApache.ini and set the version to new one
5. update wampmanager.ini in /path_to_wamp:
* add new line "Type: item; Caption: "5.6.19"; Action: multi; Actions:switchPhp7.2.23" (7.2.23 is the new version of php) before 
"Type: item; Caption: "5.6.19"; Action: multi; Actions:switchPhp7.0.4"(7.0.4 is last php version of original config)
* add swithPhp7.2.23 config before switchPhp7.0.4 config, all the configs are the same, only following is diffrent: switchPhpVersion.php 7.2.23
6. delete php.ini in path_to_wamp/bin/apache/apache/apache2.2.11/bin(2.2.11 is the version of apache, maybe is diffrent with yours ), restart wamp
7. switch to new php version
