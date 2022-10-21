# Brief 5 : DNS, TLS et plus si affinité  

## Créer un sous-domaine :  

Sur l'interface web de Gandi : je retrouve mon nom de domaine dans la liste et je suis le lien :  

![domain](https://github.com/Simplon-AlainCaupin/brief5/blob/main/IMG/gandi_domain.png?raw=true)  

Sur la page suivante : "add record" :  

![addrecord](https://github.com/Simplon-AlainCaupin/brief5/blob/main/IMG/add_record.png?raw=true)  

et faire l'enregistrement comme suit :  

![dnsrecord](https://github.com/Simplon-AlainCaupin/brief5/blob/main/IMG/add_dns_record.png?raw=true)  

Je vérifie que la page est bien accessible à l'adresse "http://voteapp.alain-cpn.space" (quand le resource group et l'application est déployée depuis son container)  

## Créer un certificat :  

Prérequis (travail sous Windows 10)  
wsl avec une distri linux (Ubuntu 22.04)  
Python3, pip et certbot  
Sur la distribution wsl, python3 et pip étant déjà installés,

Installation des prérequis :  
```
sudo apt update
sudo apt -y install certbot
```

### Utiliser Certbot pour créer un certificat en utilisant le challenge DNS :  

Depuis le site de Gandi, dans les paramètres utilisateur, partie "login and security options" :  

![security](https://github.com/Simplon-AlainCaupin/brief5/blob/main/IMG/gandi_security.png?raw=true)  

La clé est déjà générée mais il est possible d'en générer une nouvelle, sous couvert de supprimer la précédente :  

![generate_key](https://github.com/Simplon-AlainCaupin/brief5/blob/main/IMG/add_new_key.png?raw=true)  

Depuis la console WSL : créer un fichier gandi.ini contenant la clé générée ci desssus :  

![gandini](https://github.com/Simplon-AlainCaupin/brief5/blob/main/IMG/gandi_ini.png?raw=true)

Puis utiliser "certbot" pour valider le certificat via ce fichier ini :  

![certbot_cmd](https://github.com/Simplon-AlainCaupin/brief5/blob/main/IMG/certbot_command.png?raw=true)  
```
certbot certonly --authenticator dns-gandi --dns-gandi-credentials ./gandi.ini -d voteapp.alain-cpn.space
```

## Charger le certificat dans Azure Application Gateway :  

Se positionner dans le répertoire où les fichiers du certificat ont été générés :  

```
/etc/letsencrypt/live/voteapp.alain-cpn.space
```  

Concaténer les fichiers suivants :  

```
cat cert.pem privkey.pem > certificat.pem
```  
j'ai ensuite déplacé le fichier en sortie dans le "home" de mon user admin :  
```  
mv certificat.pem /home/alain/gandi/
```
puis modifié les droits d'accès afin de pouvoir le charger dans Azure :  
```
chmod 666 ./certificat.pxf
```  

Conversion au bon format (pkcs12) via openssl :  

```
openssl pkcs12 -export -out certificat.pfx -in certificat.pem
```  
Depuis l'interface Azure, sur le service "Application Gateway" :  
(pas de screens, resource group supprimé)  
Changer le port frontend vers le port 443 pour le https,  
Créer un listener en https + ajout du fichier certificat récupéré depuis wsl :  

