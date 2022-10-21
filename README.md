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

![security]()  
![generate_key]()