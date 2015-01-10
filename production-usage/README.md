# Production Usage

While WAMP stacks are generally not recommended for production usage, after some configuration WampServer can be used for this purpose. Here are the main things to do.


## Install on Windows Server 2012 R2

WampServer is best compatible in its 32-bit version (because of certain PHP extensions, etc.) and on the server side, latest Windows Server is recommended.

Installation steps on Windows Server 2012 R2 (x64):

 1. Install [Visual C++ Redistributable for Visual Studio 2012 Update 4](http://www.microsoft.com/en-gb/download/details.aspx?id=30679), **both x86 and x64 versions**. This is very important, otherwise WampServer will complain about missing MSVCR110.dll and other things.
 2. Install **WampServer 32-bit**

Start WampServer to confirm that everything is working fine.

*Note:* You can leave WampServer running to test all the other changes. It is by default accessible from local computer only so there is no risk of exposing anything.


## Set password for 'root' in MySQL

 1. Go to http://localhost/phpmyadmin
 2. Go to the *Users* tab
 3. Create a strong password for all three root accounts (`root@localhost`, `root@127.0.0.1` and `root@::1`)
 4. Open C:\wamp\apps\\**phpmyadmin4.1.14\config.inc.php** and change the `auth_type` to `cookie`:
    
        $cfg['Servers'][$i]['auth_type'] = 'cookie';
    
    (You could also update the root password directly but in that config file and keep the auto-login `config` value but that's not recommended.)


## Make WampServer start with Windows

 1. Open *Services*
 2. Find **wampapache** and **wampmysqld** services and set their startup type to *Automatic*


## Make your site public

By default, WampServer allows access from local machine only. So for example if you have `C:\wamp\www\example` site accessible as `http://localhost/example` from your server, public requests to http://yourserver/example will return `403 Forbidden`.

WampServer has a *Put Online* button under its tray icon but **don't use that**. WampServer, being primarily a development server, exposes things like phpinfo(), the whole dashboard etc. which you don't want.

You have two options:

 1. Delete everything from `c:\wamp\www` and replace it with your site
 2. Host your site as a Virtual Host

The first option is simpler but the other will offer two major benefits:

 * You will be able to host more sites / domains on the server
 * WampServer stuff will still be available when connected to the server via e.g. remote desktop

so it's probably preferable.

To create a Virtual Host, follow e.g. [these instructions](https://www.virendrachandak.com/techtalk/creating-multiple-virtual-websites-in-wampserver/). You should configure your vhosts so that localhost is accessible from local machine only (the default behavior, if you remember not to put WampServer online) while the production vhost is accessible publicly. 

An example **httpd-vhosts.conf** for one site, `www.example.com` (assumes that you have enabled vhosts in the main `httpd.conf` file):

    # This is the original localhost
    <VirtualHost *:80>
        ServerAdmin admin@localhost
        DocumentRoot "c:/wamp/www/"
        ServerName localhost
        ErrorLog "logs/localhost-error.log"
        CustomLog "logs/localhost-access.log" common
    </VirtualHost>
    
    # This is our example site
    <VirtualHost *:80>
        ServerAdmin admin@example.com
        DocumentRoot "c:/wamp/www/www.example.com/"
        ServerName www.example.com
        ErrorLog "logs/www.example.com-error.log"
        CustomLog "logs/www.example.com-access.log" common
    
        <Directory "c:/wamp/www/www.example.com/">
            AllowOverride all
            Require all granted
        </Directory>
    
    </VirtualHost>

Note the `Require all granted` which is set for the public site only.  

Now, restart WampServer / Apache and confirm that the local and remote access to the site is as intended by visiting the site URL both from local machine and from some other PC.


## Configure DNS etc.

For a public website, you'll want to configure DNS, set up firewall if necessary (for example, Azure Virtual Machines don't allow traffic on port 80 by default) etc.

All that is standard stuff not directly related to WampServer.


## (Optional) Make phpMyAdmin accessible over internet

If you want to manage MySQL remotely, do this:

 1. Open C:\wamp\alias\\**phpmyadmin.conf** in a text editor
 2. Replace `Require local` with `Require all granted`
 3. Restart Apache

