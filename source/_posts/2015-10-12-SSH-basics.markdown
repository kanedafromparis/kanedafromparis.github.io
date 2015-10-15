---
layout: "post"
title: "Les bases de SSH"
date: 2014-10-12 19:53:12
comments: false
categories:
- bash
- run
- security
- bases
---
{% img left half images/blog_run.svg 150 120 "Main theme is run" "Run" %}
---
# L'acces à distance
Dans mon blog précédent, on s'est rappeler combien la ligne de commande était un outil pratique et que cela permettait d'automatiser des opérations. L'informatique c'est l'automatisation par nature.
La ligne de commande permets donc d'enchainer mes taches. C'est une abstraction en cela que votre ligne de commande peux être éxécuté à distance.
Historiquement via telnet, mais le flux réseau en telnet n'est pas chiffrer, on préfère donc utiliser SSH (Secure Shell).

# SSH
## Clés publiques
Pour que l'échange soit privé, il y a un méchanisme de chiffrement assymetrique, avec un cles priver et une clés public. Lorsque vous vous connecter à un serveur ayant un service SSH, vous faites un échange de clés. La clés du serveur sera associté à son IP dans ~/.ssh/known_hosts.

```Bash
# ssh jvaljeanuser@example.com
Host key not found from the list of known hosts.
Are you sure you want to continue connecting (yes/no)? yes
Host 'example.com' added to the list of known hosts.
user@example.com's password: *******
```

Par defaut, un serveur ssh authorise l'authentification par mot de passe. C'est pratique, mais finalement pas tellement sécurisé.
*Pizza73* et *01022008* sont des mots de passes facile à bruteforcer. Il est donc préférable de préférer une authentification par clés.

## configuration OpenSSH
Pour mieux sécuriser son serveur on va :

 - interdire l'utilisateur root
 - interdire l'authentification par mots de passe
 - definir l'emplacement des clés authorisé
 - ajoute la clés de l'utilisateur jvaljean (on vera comment la créer plus loin)

NB : Cette commande est proposé en root, mais elle passe avec sudo.

** ATTENTION ** après cela on ne peut plus ce connecter avec l'utilistateur root 

{% codeblock lang:bash %}
TIMESLOT=`date +%Y-%m-%d-%H-%M-%S` && cp /etc/ssh/sshd_config /etc/ssh/sshd_config.$TIMESLOT && \
sed -i -e "s/PermitRootLogin yes/PermitRootLogin no/g" /etc/ssh/sshd_config && \
sed -i -e "s/#PasswordAuthentication yes/PasswordAuthentication no/g" /etc/ssh/sshd_config && \ 
/etc/init.d/sshd restart && \
mkdir /home/jvaljean/.ssh/ &&
echo "ssh-rsa X2xEB3NzaC1yc2EX2xEDAQABAAACAQCb3AG5VnbDxV3iEdLd+gqVQsOWlx7rg9K4O7i8S8SK5j1/tMq8LWTpzQToEUvOU1Psgmk/rMG2y1qvk1J3xN5s+YIeisXznMgdAF7qotGFbZy1Z6qd/hDqnmu0WbxBBvQOwe4CeyQ75heGx5HXiD2Sshv5sLemNcST4UwNx/TWBpcH0y5oGH1j+3opexQM4enOkJCFAvkBiwJ/oS4v55Qdd59dXHFr9h9TP5MYSa6H/x4BJjumAr9Epd1zTaiuR475cIjmSfB5cHDTqFv82FKCLl2IBnNc2rw5ISXASqcOblbRayEq73kEvd8zuyeA9cAT3Eq9Y3j15mAsjjwQEHx31lt6iLFIZaaCmTYtN3g7F6hv2VnVAJGmwc6cD7Ow9HGo4pLSjxqUwYdVLKSz/5ftT/bltK0S6DicNrGhCRBFUIL7EKO5pqrUa5bKQfHyiKCv8WvjK97KUpqKlCDE+Q7mpJZ3VnFz2WYVczS0sP39g9L976QJ80EfjllwD3cvfduhNhCFoVd3cLVWyxIUW7b/gpfulLgKhT4LabQdWx5445LHMiUSnGql3gH/P/bn5d/5YWvDZTo3YICE0fSEq--4qhQbXRdIPZQxf+yTbIB91ZuXtJrPRLYUWdfXCYsl4ktrqAlfycopOD5vjeNtvWyUhHg3zFVIb+Hzb7e3ZIzYtQ== jvaljean@example.com" >> /home/jvaljean/.ssh/authorized_keys &&
chown -Rv jvaljean:jvaljean /home/jvaljean/.ssh/ && \
chmod u+rw,og-rwx /home/jvaljean/.ssh/authorized_keys
{% endcodeblock %}

Maintenant, l'utilisateur root ne peux plus ce connecter, l'utilisateur jvaljean peux se connecter uniquement avec sa clés public "ssh-rsa X2x..."

## creation de la clé

Pour crée sa cles il faut choisir : 

- sa taille : *4 096* (d'après l'ansii donne un durée de vie acceptable ici : http://www.ssi.gouv.fr/uploads/2015/01/RGS_v-2-0_B1.pdf)
- l'algorithme : RSA (par défaut)
- l'emplacement de la clés (Et oui, on peux et devrais avoir plusieurs clés, on y reviendra plus loin, sinon c'est id_rsa)
- un mot de passe pour utiliser la clés (evitons pizza82)

{% codeblock lang:bash %}
ssh-keygen -t rsa -b 4096 -C "jvaljean@example.com"
{% endcodeblock %}
ou
{% codeblock lang:bash %}
ssh-keygen -t rsa -b 4096 -C "jvaljean@example.com" -f ~/.ssh/id_mongroupedeserver_algo
{% endcodeblock %}

Après on peux se connecter au serveur ayant notre clés avec la commande ssh
Rappel : 
{% codeblock lang:bash %}
ssh example.com
{% endcodeblock %}
ou
{% codeblock lang:bash %}
ssh -p 22 -i ~/.ssh/id_mongroupedeserver_algo jvaljean@example.com
{% endcodeblock %} 
## le port-mapping 
Une fonction très pratique de SSH est le port mapping il est possible de mapper un port de votre client local à celui du serveur distant. Cela permets de vous contecter à distance sur vos serveur comme si vouys etiez dans votre réseau. on 
peut par exemple mapper le port 8080 d'un tomcat sur sont 8888 local comme ceci
 
 
{% codeblock lang:bash %}
ssh -p 22 -i ~/.ssh/id_mongroupedeserver_algo jvaljean@example.com -L 127.0.0.1:8888:example.com:8080
{% endcodeblock %} 

on pourra par la suite acceder depuis son client web local sur http://127.0.0.1:8888 comme si nous etions sur le réseau local de la machine

## ~/.ssh/config
Nous l'avons vu précédement, la cles d'authenfication est précieuse, mais il est parfois nécessaire de la copier sur d'autre machine, elle peu être corromput ou obsolète. Enfin, certain groupe de serveur peuvent nécessité des sécurité différentes.
Pour cela, il est conseiller d'avoir plusieurs cles. Pour éviter de taper des commande trop longue, il est possible de définir les commandes "par defaut, par host dans "~/.ssh/config" sous la forme :

- Host : les serveurs utilisant cette configuration (liste séparé d'un espace)
-       Hostname : le nom du serveur qui appel celui annocé par le client
        User : le nom de l'utilisateur a utiliser
	      IdentityFile ~/.ssh/id_github_mb35_rsa
	      
{% codeblock lang:bash %}
Host example.com
        Hostname jvaljean@example.com
        User jvaljean@example.com
        LocalForward 8888 127.0.0.1:8080
	      IdentityFile ~/.ssh/id_mongroupedeserver_algo
	      
{% endcodeblock %} 

## Conclusion
SSH est l'outil de base du sysadmin et du très tendance DevOps. IMHO (a mon humble avis) "~/.ssh/config" n'est pas suffissement connu. 