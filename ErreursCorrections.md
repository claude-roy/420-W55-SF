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