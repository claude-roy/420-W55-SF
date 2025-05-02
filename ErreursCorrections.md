# Erreurs et corrections

### Erreur avec Metasploit et postgres

Si vous avez le message d'erreur :  

***WARNING:  database "msf" has a collation version mismatch***

Vous pouvez essayer les commandes suivantes :  

~~~bash
sudo -u postgres psql -U postgres -d msf  
REINDEX DATABASE msf;  
ALTER DATABASE msf REFRESH COLLATION VERSION;  
quit;  
~~~

### Erreur de BD dans Mutillidae de Metasploitable 2

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