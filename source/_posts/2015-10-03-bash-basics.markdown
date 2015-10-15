---
layout: "post"
title: "Les bases de la ligne de commandes"
date: 2014-10-03 19:53:12
comments: false
categories:
- bash
- run
- bases
---

{% img left half images/blog_run.svg 150 120 "Main theme is run" "Run" %}
---
## Les terminaux
Un terminal est un accès à un ordinateur central. Sous linux, on parle de terminal pour l'invite de commande. L'avantage de l'invite de commande à mon avis est que c'est :

  - simple
  - relativement standard (malgrès des querel d'interpréteur que je développerais pas maintenant)
  - faible cout et faible cout réseaux
  - facilement automatisable

  
C'est pourquoi c'est sans doute l'outil de base qu'il faut apprendre à maitriser en priorité. IMHO (A mon humble avis), c'est l'essence de l'informatique, le reste devrais découller natuellement dans l'optique baisc de ce simplifier la vie.

### l'enchainement des commandes
Mes commandes de prédilections sont :

  - *cat* et *zcat* pour afficher les contenus d'un fichier
  - *less* et *zless* pour afficher les contenu d'un fichier de façon interactive
  - *grep* pour trouver un morceau de texte dans un long fichier
  - *tail* pour afficher la fin d'un fichier texte
  - *tail -f* pour afficher la fin d'un fichier texte et 
  - *mnon* qui affiche l'activité de mon serveur
  - *sed* pour remplacer un morceau de text par un autre
  - *sudo* pour passer des commandes avec des droits particulier (souvant root)

Je pense qu'il faut comprendre l'idée des "pipe" (tuyaux) en priorité. L'idée est que votre travail est un flux, comme un cours d'eau ou un courant électrique et que vous avez la possibilité de brancher les entrées et les sorties de vos commandes les unes aux autres.

Les branchements sont :

 - ";" juste pour enchainer les commandes
 - "&&" pour enchainer les commandes uniquement tant que les commandes s'exécute sans problème
 - "|" prendre en entrée la sortie d'une autre commandes
 - "<" donne en entrée la sortie d'une autre commandes
 - ">" ecrire et remplacer le contenu de la commande dans un fichier 
 - ">>" ecrire ajouter le contenu de la commande dans un fichier

En pratique, pour mettre à jour un fichier de configuration par exemple après avoir créer commander un nouveau serveur, vous voulez :

{% codeblock %}
 - créer l'utilisateur jvaljean 
 - créer un group admin
 - mettre jvaljean dans ce groupe
 - donner les droits de sudo aux memebre de ce groupe 
 - interdire l'acces à votre machine à l'utilisateur root depuis SSH
{% endcodeblock %}

Vous pouvez taper les commandes suivantes : 
{% codeblock lang:bash %}
adduser jvaljean
groupadd admin
adduser jvaljean admin
nano /etc/sudoers (et ajouter "%admin    ALL=(ALL) ALL" à la fin du fichier)
nano /etc/ssh/sshd_config (et remplacer "PermitRootLogin yes" par "PermitRootLogin no") 
relancer le service /etc/init.d/ssh restart
{% endcodeblock %}

ou bien faire :
{% codeblock lang:bash %}
adduser jvaljean && groupadd admin && adduser jvaljean admin && echo "%admin    ALL=(ALL) ALL" >> /etc/sudoers && sudo sed -i -e 's/PermitRootLogin yes/PermitRootLogin no/g' /etc/ssh/sshd_config && sleep 2 && sudo /etc/init.d/ssh restart
{% endcodeblock %}

Pour connaitre les IP qui se sont connecter à votre serveur le "30 aout entre 6 et 7 vous pouvez :
{% codeblock %}
récupere votre fichier de log (scp ou ftp)
l'importer sur excel
utiliser les filtres pour optenir l'information
{% endcodeblock %}

ou faire :

{% codeblock lang:bash %}
zcat /var/log/apache2/xwiki-access.log | grep "30/Aug/2015:06" | \
awk '{split($0,a," "); print a[1]}' | sort -nu 
{% endcodeblock %}

ou pour connaitre la navigation de 192.168.1.10 le 30 Août 2015
{% codeblock lang:bash %}
zcat /var/log/apache2/xwiki-access.log.6.gz | grep "30/Aug/2015" | \
grep "192.168.1.10" | awk '{split($0,a," "); print a[4] a[6] a[7]}'
{% endcodeblock %}


Et puis pour s'envoyer par mail le nombre d'IP connecter en une journée en une commande
{% codeblock lang:bash %}
TIMESLOT=`date +%d/%b/%Y` && \
NIP=$(cat /var/log/apache2/xwiki-access.log | grep "$TIMESLOT" | \
awk '{split($0,a," "); print a[1]}' | sort -nu | wc -l) \
&& echo "$NIP on $TIMESLOT" | mail -s "$NIP on $TIMESLOT" jvaljean@gmail.org
{% endcodeblock %}

## Conclusion
Propablement rien de nouveau sous le soleil je pense, mais je me suis dit qu'il était préférable d'avoir parler de ces bases, avant de passer à la suite.