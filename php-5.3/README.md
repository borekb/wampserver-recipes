# How to install PHP 5.3.29 on WampServer 2.5 32-bit


**Important: this only works on 32-bit WampServer, not 64-bit**


 1. Download [php-5.3.29-Win32-VC9-x86.zip](http://windows.php.net/downloads/releases/php-5.3.29-Win32-VC9-x86.zip) from [windows.php.net](http://windows.php.net/download/#php-5.3)
     - It must be the **thread safe version**
 2. Create new folder `C:\wamp\bin\php\php5.3.29`
 3. Extract the downloaded ZIP there
 4. Copy the file `wampserver.conf` from `php5.5.12` to `php5.3.29`
 5. Copy `php.ini-development` to `php.ini` in the `php5.3.29` folder
 6. Do the following edits in the `php.ini` file:

    Replace:

    `;error_log = php_errors.log` => `error_log = C:/wamp/logs/php_error.log`
    `register_argc_argv = Off` => `register_argc_argv = On`
    `; extension_dir = "./"` => `extension_dir = "C:/wamp/bin/php/php5.3.29/ext/"`
    `;upload_tmp_dir =` => `upload_tmp_dir = "C:/wamp/tmp"`
    `;date.timezone =` => your timezone, e.g. `date.timezone = Europe/Paris`
    `mysql.default_port =` => `mysql.default_port = 3306`
    `;session.save_path = "/tmp"` => `session.save_path = "C:/wamp/tmp"`

    Replace the whole block between

        ; Be sure to appropriately set the extension_dir directive.

    and

        ;;;;;;;;;;;;;;;;;;;
        ; Module Settings ;

    with:

        ; Be sure to appropriately set the extension_dir directive.
        ;
        extension=php_bz2.dll
        ;extension=php_curl.dll
        extension=php_fileinfo.dll
        extension=php_gd2.dll
        extension=php_gettext.dll
        ;extension=php_gmp.dll
        ;extension=php_intl.dll
        ;extension=php_imap.dll
        ;extension=php_interbase.dll
        ;extension=php_ldap.dll
        extension=php_mbstring.dll
        extension=php_exif.dll  ; Must be after mbstring as it depends on it
        extension=php_mysql.dll
        extension=php_mysqli.dll
        ;extension=php_oci8.dll  ; Use with Oracle 10gR2 Instant Client
        ;extension=php_oci8_11g.dll  ; Use with Oracle 11g Instant Client
        ;extension=php_openssl.dll
        ;extension=php_pdo_firebird.dll
        ;extension=php_pdo_mssql.dll
        extension=php_pdo_mysql.dll
        ;extension=php_pdo_oci.dll
        ;extension=php_pdo_odbc.dll
        ;extension=php_pdo_pgsql.dll
        extension=php_pdo_sqlite.dll
        ;extension=php_pgsql.dll
        ;extension=php_phar.dll
        ;extension=php_pspell.dll
        extension=php_shmop.dll
        
        ; The MIBS data available in the PHP distribution must be installed. 
        ; See [www.php.net] 
        ;extension=php_snmp.dll
        
        extension=php_soap.dll
        extension=php_sockets.dll
        extension=php_sqlite.dll
        ;extension=php_sqlite3.dll
        ;extension=php_sybase_ct.dll
        ;extension=php_tidy.dll
        extension=php_xmlrpc.dll
        extension=php_xsl.dll
        ;extension=php_zip.dll
        
        ;;;;;;;;;;;;;;;;;;;
        ; Module Settings ;

 7. Copy this `php5.3.29/php.ini` to `php5.3.29/phpForApache.ini`
 8. Run WampServer
 9. Switch to PHP 5.3.29

## References

 - [Forum post that is often linked to](http://forum.wampserver.com/read.php?1,124128)