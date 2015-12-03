---
layout: "post"
title: "Devoxx BE 2015"
date: 2015-10-16 21:53:12
comments: false
categories:
- Devoxx
- java
- bases
---
{% img left half images/blog_java.svg 150 120 "Main theme is Dev" "dev" %}
---

[Ce blog à été rédiger et hébergé pour IPPON](http://blog.ippon.fr/2015/11/19/devoxx-be-2015/)


J’ai eu la chance d’assister cette année encore à Devoxx Belgique édition 2015.

Pour ceux qui ne connaissent pas, Devoxx Belgique (ex-Javapolis) est un évènement créé pour réunir les développeurs et évangélistes du monde entier. Lancé en 2001 par Stephan Janssen, il compte environ 3 500 personnes, 200 speakers et plus de 250 conférences.

Nouveauté de l’édition 2015, les vidéos sont désormais (et déjà) disponibles sur Youtube (Parleys allant être coupé).

Ce qu’il faut retenir
Une petite synthèse de ce que j’ai pu en retenir :

* Les microservices ont le vent en poupe, avec les couples Docker / Kubernetes en tête de pont. La tendance pousse à découper correctement ses activités métiers. Indirectement, la technologie (NodeJS, Java EE, Spring, etc.) n’est plus un débat dans un mode ouvert interagissant via des APIs REST et des objets JSON. Le mouvement DevOps s’harmonise naturellement avec ce type d’architecture.
* Java 9 : Jigsaw va être un challenge important qu’il faudra prendre en compte au plus vite.
* L’asynchronisme, AngularJS et le Big Data poursuivent leurs avancés.
Et puis un peu de cocorico pour IPPON avec la présentation de JHipster par Matt Raible.

Par le hasard des salles trop pleines, j’ai également constaté que les technologies tournant autour du BPMN avaient bien avancé et pourraient être maintenant intéressant à creuser.

#Stands
##Sur les stands (espace exposants) et de façon non exhaustive :

Oracle présentait les objets connectés et l’impression 3D qui était saisissants.
Pivotal présentait CloudFoundry, un outil de cloud facilitant les déploiements et la maintenance (voir les articles sur le blog Ippon).
Red Hat faisait la promotion de son offre Openshift, un outil de cloud orienté DevOps.
Samsung avait un focus fort sur les mobiles et son outil Knox de gestion de flotte/sécurité.
JetBrains présentait la version 15 d’IntelliJ IDEA.
Microsoft offrait du Azure et un rapprochement avec OpenShift.
Plusieurs sociétés de service local étaient également présentes.
Conférences
Un zoom sur les présentations auxquelles j’ai assisté (par thème).

#Java
##Keynotes: Java 9 – Make way for modules (Mark Reinhold)
C’est une présentation que tout le monde devrait voir, car elle présente l’impact de Jigsaw dans la JVM. Jigsaw va changer beaucoup de choses sur le modèle de sécurité, de déploiement, mais aussi sur les formats et l’utilisation de la JVM. La disparition du classpath est quelque chose qui va probablement perturber nos habitudes de développement et surtout de diagnostic. L’outil d’analyse de dépendances jdeps est indiscutablement à faire tourner dans tous nos Jenkins, parce qu’il est préférable d’anticiper au plus tôt cette évolution.

#Project Jigsaw: Under the Hood (Mark Reinhold & Alan Bateman)
Mark Reinhold nous présente Jigsaw en trois parties : l’accessibilité, les différents modules et les layers. Le sujet est important car il s’agit du coeur du problème de Jigsaw de permettre un modèle de sécurité correct sans perdre l’historique et les outils de builds. On y apprend entre autres la transitivité des modules et la sécurisation des classloaders à travers de nouveaux classloaders : les layers. Il faut prendre le temps de regarder cette présentation, Java 9 va arriver bientôt et ce changement est important.

#Methodology & DevOps
##Distributed Load Testing with Kubernetes (Amanda Waite)
Une très bonne démonstration pratique du système de gestion des ressources d’un cluster. Amanda Waite nous présente avec pédagogie Kubernetes, sa gestion de ressources, de provisioning, etc. La présentation est sans doute à voir en introduction aux notions de cluster.

##Developing and deploying Java-based microservices in Kubernetes (Ray Tsang)
Une jolie présentation de Kubernetes pour la gestion des microservices et leur simplicité de déploiement. Ray Tsang développe en live une application Spring Boot qu’il déploie sur GCE (Google Compute Engine). Il ajoute un bug dans l’application pour que celle-ci prenne toutes les ressources du serveur et montre comment Docker permet de circonscrire correctement ce problème.

##7 Ways to Hack Your Brain to Write Fluently (Dan Allen)
Dan Allen nous parle des difficultés des travaux de rédaction et nous donne 7 “bonnes pratiques” pour se faciliter la tâche. La présentation est claire et agréable. Si vous avez du mal à blogger, prenez le temps de la regarder. C’est en effet plein de bon sens et de bonnes astuces. (Write in plain text, Answer a question, Sentence-per-line, Draft in comments, Power thesaurus, Visualize progress, Couch read)

##Process-driven applications: let BPM do (some of) your work (Kris Verlaenen)
Cette présentation aurait sa place dans les Tools in Action, car Kris Verlaenen nous présente jBPM et sa facilité d’utilisation pour créer facilement des applications permettant de traiter des cas métiers. C’était intéressant, mais je ne suis pas certain qu’il soit nécessaire de la regarder, sauf pour découvrir jBPM.

##Open Source Workflows, Business Rules and Case Management live and in action – with Camunda BPM (Bernd Rücker)
Cette présentation présente Camunda, un éditeur BPM. La présentation m’a donné envie de redonner sa chance au BPMN. Je ne sais pas si je pourrais m’y consacrer, mais en tout cas, il m’a semblé en suivant cette présentation que Camunda pouvait offrir un gain de productivité intéressant pour intégrer rapidement des processus métiers. Son intégration à Maven m’a bien plu. Bref, la présentation est intéressante pour ceux qui s’intéressent à la modélisation et qui estiment qu’on arrivera peut être un jour à donner ce type d’outil au métier.

##Architecting for an Agile Journey that is as good as the End (Van Bruggen Henrik)
Cette présentation était sur la transformation digitale de ING. J’étais curieux et cette présentation aura sans doute été ma bonne surprise. Le speaker commence par une présentation “de consultant” présentant les différentes révolutions industrielles, leurs impacts en termes social et productivité. Puis il parle de la révolution agile/devops d’ING qui démarre en 2009 et qui n’est pas finie. Il insiste sur l’importance de rester ouvert sur les évolutions, sur les langages. Il enchaîne sur l’idée qu’il recrute des personnes qui s’adaptent et apprennent en permanence. Il poursuit en présentant l’immutabilité des PECS (producer extends / consumer super) d’Effective Java pour démontrer qu’apprendre Scala permet de mieux appréhender cette évolution Java. Honnêtement, un bon moment est à passer dans cette vidéo.

##Microservices and Modularity or the difference between treatment and cure! (Milen Dyankov)
Une deuxième surprise agréable, c’était Liferay, avec qui il fallait que je fasse connaissance. Milen Dyankov a fait une démonstration avec brio qu’il fallait prendre de la distance avec les tendances et a présenté un refactoring de l’application Java EE de référence Duke’s Forest en mode microservice. Il en a profité pour intégrer ces microservices dans Liferay. J’ai trouvé que la démonstration était bien faite.

#Server Side Java
##Arquillian Cube: Production Near Unit Tests Against Docker Images (Andy Gumbrecht)
Andy Gumbrecht présentait Arquillian et Arquillian Cube. Arquillian est un outil de tests d’intégration Java EE permettant d’intégrer le lancement d’un serveur Java EE dans JUnit pour faire des tests d’intégration de la façon la plus complète possible. Il en profite pour donner l’astuce d’utiliser TomEE, qui permet de lancer plusieurs de ces tests en parallèle, contrairement aux autres serveurs d’application. Je ne connaissais pas. Je pense que l’outil va trouver sa place chez moi. Ensuite, il présente Arquillian Cube qui est une extension d’Arquillian permettant l’intégration de Docker pour l’exécution de ses tests d’intégration.

##Architecture & Security
#$HOME Sweet $HOME (Xavier Mertens)
C’est une présentation introductive qui couvre correctement les problématiques de la sécurité dans l’IoT (Internet of Things). Mais elle est restée à mon avis trop généraliste et trop en surface des points abordés. J’aurais apprécié entrer plus dans les détails des protocoles et des failles. Mais ce n’était pas le but de la présentation et nous sommes restés dans une vision globale. Cependant, si vous n’avez pas conscience du nombre incroyable de failles et de risques qu’apporte l’IoT, c’est une excellente introduction à la sécurité.

#Cloud & BigData
##Real World Use Cases for Tachyon, a memory-centric distributed storage system (Haoyuan Li)
Cette présentation était faite en remote, ce qui était assez perturbant. Mais cela m’a permis de faire la connaissance de Tachyon, un outil pour faciliter les problématiques (de temps) d’accès aux filesystems entre autres pour HDFS.

##IOT, timeseries and prediction with Android, Cassandra and Spark (Amira LAKHAL)
Une très belle présentation de machine learning et d’IoT. Amira Lakhal, nous présentait un cas très intéressant de machine learning. Il s’agit à partir des remontées de l’accéléromètre (x, y et z) d’un smartphone de déduire ce que fait son utilisateur. Nous passons par toutes les étapes de réflexion pour la collecte de données avec Cassandra et son exploitation avec Spark. C’est très pédagogique. Je conseille cette présentation.

Et aussi…
Il y a aussi eu des présentations dont j’ai entendu le plus grand bien :

*L’ensemble de la série Java 9 :
** Prepare for JDK 9! par Mark Reinhold/Alan Bateman
** Introduction to Modular Development par Mark Reinhold/Alan Bateman
** Advanced Modular Development par Mark Reinhold/Alan Bateman

Auxquels, on peut ajouter :

*Asynchronous programming in Java 8: how to use CompletableFuture par José Paumard
*Plugin Gradle, take the control of the build!  par Eyal LEZMY
*Knowledge is Power: Getting out of trouble by understanding Git par Steve Smith
*CDI 2.0 is coming par Antoine Sabot-Durand/José Paumard
*Deep Learning for common mortals par Sam Bessalah
*The never-ending REST API design debate  par Guillaume Laforge
*The Twelve Factor app: Best Practices for Java Deployment par Joe Kutner
*Core Design Principles for Software Developers et Get a Taste of Lambdas and Get Addicted to Streams par Venkat Subramaniam
Mais je suis loin d’avoir tout vu, on va continuer à creuser un peu…