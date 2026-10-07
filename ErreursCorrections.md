# Erreurs et corrections

## Erreur dans msfconsol
### L'erreur  

```The supplied module name is ambiguous: uninitialized constant HTTP```

This error, "The supplied module name is ambiguous: uninitialized constant HTTP," indicates a broken loading order in Metasploit, specifically affecting modules under exploits/*/http/ when running on Ruby 3.3+. It is a known bug causing HTTP::CookieJar to fail because its subclass is loaded before the base class  

### La solution

To fix this issue immediately, you need to edit the ```http_cookie_jar.rb``` file to swap the order of two lines.

1. Open the affected file in a terminal editor (like nano) with sudo privileges:

```bash
sudo nano /usr/share/metasploit-framework/lib/msf/core/exploit/remote/http/http_cookie_jar.rb
```  

2. Locate the following lines :

```ruby
require 'http/cookie_jar/hash_store' # subclass first
require 'http/cookie_jar' # base class second
```  

3. Swap them so the base class loads first:

```ruby
require 'http/cookie_jar' # base class first
require 'http/cookie_jar/hash_store' # subclass second
```  

4. Save and exit (Ctrl+O, Enter, Ctrl+X).  

5. Restart ```msfconsole```.

### Une autre solution possible  

Il est également possible d'essayer la commande ```reload_all``` à l'intérieur de msfconsole.  

## Erreur avec Metasploit et postgres

Si vous avez le message d'erreur :  

***WARNING:  database "msf" has a collation version mismatch***

Vous pouvez essayer les commandes suivantes :  

~~~bash
sudo -u postgres psql -U postgres -d msf  
REINDEX DATABASE msf;  
ALTER DATABASE msf REFRESH COLLATION VERSION;  
quit;  
~~~

## Erreur de BD dans Mutillidae de Metasploitable 2

Vous devez modifier la base de données de Mutillidae de **metaploit** à **owasp10**.  

Éditez le fichier `/var/www/multillidae/conhfig.inc` pour changer la variable `$dbname`à owasp10.

**Configuration originale.**  

~~~config
<?php
        /* NOTE: On Samurai, the $dbpass password is "samurai" rather than blank */

        $dbhost = 'localhost';
        $dbuser = 'root';
        $dbpass = '';
        $dbname = 'metasploit';
?>

~~~

**Configuration finale.**  

~~~config
<?php
        /* NOTE: On Samurai, the $dbpass password is "samurai" rather than blank */

        $dbhost = 'localhost';
        $dbuser = 'root';
        $dbpass = '';
        $dbname = 'owasp10';
?>

~~~

Vous devez après relancer le service Apache2 : `sudo /etc/init.d/apache2 reload`.