Table des matières : 

- [Introduction](#introduction)
  - [Securizing a system, what does it mean?](#securizing-a-system-what-does-it-mean)
  - [Protecting.. but against what ?](#protecting-but-against-what-)
- [Introduction](#introduction-1)
  - [Que veut dire sécuriser un système ?](#que-veut-dire-sécuriser-un-système-)
  - [Se prémunir, mais contre quelles attaques ?](#se-prémunir-mais-contre-quelles-attaques-)
  - [Comment sécuriser une application ?](#comment-sécuriser-une-application-)
- [Stratégie de sécurisation :](#stratégie-de-sécurisation-)
  - [La protection navigateur :](#la-protection-navigateur-)
  - [Les protocoles de protection de l'échange de donnée :](#les-protocoles-de-protection-de-léchange-de-donnée-)
  - [Hachage, salage et Mots de passe :](#hachage-salage-et-mots-de-passe-)
  - [La sanitisation :](#la-sanitisation-)
  - [L'accès aux données :](#laccès-aux-données-)
  - [L'authentification :](#lauthentification-)
  - [Session et token :](#session-et-token-)
  - [La sécurisation des API](#la-sécurisation-des-api)
  - [La journalisation :](#la-journalisation-)
  - [Sécurisation en phase finale et en maintenance.](#sécurisation-en-phase-finale-et-en-maintenance)
- [Conclusion :](#conclusion-)

Proposition de stratégie de sécurisation d'une application web pour la mission locale du Valenciennois. 

# Introduction
## Securizing a system, what does it mean? 

Securizing a system, is protecting it in advance from attacks. Each web application must be securised. These days, most of the attacks doesn't focus on a specific website but are systematised et automated (the attacks are random and just focus data transfers).

It is well known that Internet is a system that allowed several computers to communicate (computers, servers, ..). In a way to communicate, this devices exchanges packets. This packets must be securised in term of: 
- **Confidentiality**: Assuring confidentiallity is assure that no attackers can take notice of our data. When we think about Informatic Security, we can think about crypting data. Crypting data is a way to prevent attackers to read our datas. 
- **Authenticity**: When two users are sending messages to each other, it is important to verify that each of them truly is who he pretend to be. Verifying authenticity can be resume as verifying identity. 
- **Integrity**: The sent message must be receive by the receptor in it wholeness and must not have been modified by a potential attacker. 
We will see that these three points constitute the essence of our security strategy. Moreover, our entire strategy policy aim to ensure theses points. 

## Protecting.. but against what ? 

First of all, let's take a look at the different must commons types of menace we will encounter in a way to understand the different attacks we will face to: 
- **ressource compromisation**: The compromisation of our datea is a direct attack against our application's integrity with the objective of modifying our website. The attacker can modify the public informations (by displaying a personnal message for example), or trying to trap users with a process named _watering hole_.
- **Data theft** : Data thefting is an attack of our data confidentiallity (most of the time for the user data). The attacker is stealing confidential informations most of the time to use or sell them. 
- **Denial of service**: In the denial of service, tha attacker want our application to break down. To do that, he will try to overkill our app by sendig it a lot of requests. 

From theses menace families, it exists lots of attacks, here are some: 
- **Cross-Site-Scripting**: The **XSS** attack is an attack where the attacker is trying to inject some malicious code in our application. 
- **SQLi**: 
- **CSRF**

# Introduction
## Que veut dire sécuriser un système ? 

Sécuriser un système, c'est le protéger de toute attaque. Chacune des applications web se doit d'être sécurisée. La plupart des attaques ne sont plus ciblées mais systématisées et automatisées (attaques "aléatoires" des applications où transitent des informations). 

Il est bien connu qu'Internet est un  système permettant la communication de plusieurs appareils (ordinateurs, serveurs, ...). Afin de communiquer, ces appareils s'échangent des paquets. Ces paquets doivent être sécurisés en terme de : 
- **Confidentialité**: Lorsqu'on pense sécurité informatique, on pense au chiffrement des données. Il s'agit ici de chiffrer un message de façon à ce qu'un tiers attaquant ne soit pas en mesure de comprendre le message.
- **Authenticité**: Lorsque deux utilisateurs s'envoient des messages, il est important de vérifier que chacun des parti est bien celui qu'il prétend être afin de ne pas envoyer un message à un mauvais destinataire attaquant. 
- **Intégrité**: Le message envoyé doit être conservé dans sa totalité et ne pas être modifié en chemin par un attaquant.  
Nous verrons que ces trois points constituent les pilliers de toute stratégie de sécurisation.

L'entièreté de notre proposition de stratégie de sécurisation aura pour but de faire respecter ces trois pilliers de l'échange d'information. 

## Se prémunir, mais contre quelles attaques ? 

Avant d'aller plus loin dans la prévention, passons en revue les différentes menaces les plus souvent rencontrées afin de comprendre en amont les attaques qui pourront concerner notre application : 
- **Compromission des ressources** : La compromission des données est une attaque directe de l'intégrité du site ayant pour objectif de modifier les données de celui-ci. L'attaquant peut faire le choix de modifier les informations publiques du site, ou celui-ci peut tendre un piège aux visiteurs avec un processus nommé _watering hole_.
- **Vol de données** : Le vol des données est une attaque de la confidentialité des données (la plupart du temps utilisateur). L'attaquant vole des informations confidentielles le plus souvent afin de s'en servir. Nous reviendrons sur les différents types d'attaques en résultant. 
- **Déni de service** : Un attaquant tente de rendre inaccessible un site ou un service, la plupart du temps en le surchargeant comme avec l'attaque _DDOS_.

Ces familles de menaces donnent lieu à ces attaques les plus courantes : 
- **Cross-Site-Scripting (XSS)**: C'est une injection de code dans le site puis dans le serveur. L'utilisateur est incité à faire une requête web au nom d'un attaquant. 
- **Injection attacks (SQLI)**: Injection de commandes SQL afin de tester l'accès aux données.
- **CSRF** : C'est un type d'attaque par l'intermédiaire de l'utilisateur. La cible de l'attaque est l'application. Il est hautement conseillé pour un site officiel et institutionnel de se prévenir de ce type d'attaques. 

Le passage en revue de ces grandes familles d'attaques nous paraît un point nécessaire afin d'introduire ces termes qui seront utilisés tout au long de ce guide.

## Comment sécuriser une application ? 

vant la présentation de notre stratégie de sécurisation spécifique, arrêtons-nous une fois de plus sur les processus généraux permettant le respect de l'intégrité, de l'authenticité et de la confidentialité des échanges sur internet. 

1) **La défense en profondeur**: Une application, en particulier Web, utilise différents services pour son fonctionnement. Ces différents sevices qui composent le système sont autant de sous-systèmes à sécuriser individuellement. 
2) **Le moindre privilège**: Le moindre privilège permet de filtrer qui parmi les utilisateurs aura accès à certaines données. Plusieurs niveaux d'accès peuvent être définis parmi plusieurs sous-systèmes. 
3) **La réduction de la surface d'attaque**: La réduction de la surface d'attaque passe par la minimisation des données exposées, mais également par les bonnes pratiques de sauvegarde des données sensibles. Cette approche est complémentaire du moindre privilège. 

All of the present document is based to follow a maximum of the official ANSSI's recommendations. 

# Stratégie de sécurisation : 

## La protection navigateur : 

L'application en construction a pour but final d'être utilisée via Internet. Ce type d'application nécssite l'utilisation d'un navigateur afin d'accèder aux ressources souhaitées. Un premier niveau de protection par le navigateur peut être ici soulevé.

L'utilisation d'un navigateur pour utiliser notre application garantie l'utilisation du protocole **SOP** _Same Origin olicy_ : Ce protocole garantie que deux sites séparés ne peuvent pas communiquer entre eux (sur deux onglets par exemple). C'est une sécurité qui est globale et présente par défaut. 

Pour notre application, il peut être intéressant de contourner cette directive en définissant des **CORS** _Cross Origin Ressource Sharing. Ceux-ci permettent d'autoriser la communication de certains pages spécifiques.  

Enfin, afin de s'assurer le respect du principe de réduction de la surface d'attaque, notre application devra restreindre les données accessibles aux autres pages à l'aide des **CSP** _Constent Secure Policy_. 

Pour finir, la mise en place de **SRI** pour _Subressource integrity_ est également fortement conseillée afin de vérifier l'authenticité des ressources aux sources externes. Pour simplifier, lorsqu'une ressource extérieure est terminée, celle-ci est passée dans une fonction dite de hashage qui produit une certaine empreinte. Cette empreinte est mise à disposition afin que le navigateur compare l'exactitude de celle-ci avec la ressource reçue. Si l'empreinte n'a pas changé, les éléments du fichier n'ont pas été modifiés par un attaquant malveillant. 

## Les protocoles de protection de l'échange de donnée : 

Notre application, comme toute application disponible sur navigateur, implique un échange de donnée entre la partie "client" (côté utilisateur) et la partie "serveur" (serveur physique, en quelque sorte la base de donnée). Cet échange entre un serveur et un client se fait par l'intermédiaire de paquets d'information selon le protocole **HTTP** pour _Hyper Text Transfer Protocol_. 

Ce protocole HTTP est très limité car il ne permet pas le chiffrement des données qui transitent. Pour ce faire, il est absolument nécessaire pour notre application de mettre en place le protocole de chiffrement **TLS** qui chiffrera chaque paquet, rendant difficile (mais pas impossible) sa lecture par un potentiel attaquant. La mise en place de ce protocole est toutefois une grande défense contre les attaques de type _Man-In-The-Middle_. La mise en place du protocole TLS couplé au HTTP permet d'obtenir la certification **HTTPS**. 

Afin d'approfondir la sécurisation de l'échange des données, il est également nécessaire de mettre en place des **CTl** pour _Certificate Transparency logs_, qui sont des certificats de transparence permettant l'obtention des sigles de sécurité. Leur durée limitée nécessite une certaine maintenance dans la ré-actualisation de ceux-ci. 

En parrallèle, le **HSTS** pour _HTTP Strict Transport Security_ doit être mis en place afin de rediriger toute communication HTTP vers une communicatiopn HTTPS. Une attention particulière doit toutefois être portée à la première visite de l'utilisateur sur notre application car ce protocole n'est pas encore mis en place. 

Ces protocoles de sécurité sont indispensable à toute navigation Internet, elles sont de plus une clé pour le référencement sur les moteurs de recherche. 

Il est à retenir que ces défenses sont nécessaires à la sécurisation d'une application mais non suffisantes. C'est pourquoi d'autres vulnérabilités sont à prendre en compte. Dans ce contexte, notre sécurisation d'application s'appuiera forcément sur le procole HTTPS mais ne peut se suffir de celui-ci. 


## Hachage, salage et Mots de passe : 

Lorsque l'on parle de sécurisation des donnée, nous souhaitons rendre illisible l'information à tout potentiel attaquant. Pour cela, un message en clair est chiffré à l'aide de ce que l'on appelle une fonction de hachage. Le reésultat de cette opération de chiffrement est appelé une "empreinte". 
Cette **fonction de hachage** est une fonction mathématique qui fragmente et modifie un message de façon à ce que le résultat obtenu ne permette pas le retour au message originel. 

Concernant l'algorithme utilisé pour le hachage, nous faisons le choix d'utiliser le **SHA 256**, algorithme développé par la NSA. Il est une version réputée pour sa fiabilité, sa précision et sa difficulté de déchiffrement. 

Néanmoins, la fonction de hachage comporte une faille: lorsque l'algorithme clé est trouvé, tout _input_ est facilement déchiffrable par l'attaquant. C'est pour cette raison qu'il est indispensable d'y adjoindre un **salage**. 
Le salage est une opération qui consiste à ajouter des mots ou des caractères aléatoires à l'empreinte obtenue par le hachage. Le tout est haché une nouvelle fois de façon à ce que l'on ne puisse plus trouver le messsage original, sauf si l'on connait le sel utilisé. 

Afin d'effectuer un salage efficace, nous ne connaitrons pas le processus de salage utilisé, seul notre outil **BCrypt** qui sera utilisé sera en mesure de définir le salage utilisé. 

Notre politique concernant la création, le stockage et la gestion des mots de passe peut se résumer ainsi : 
- Pas de mots de passes stockés en clair
- Compte strictement nominatif par utilisateur
- Pas de délai d'expiration des mots de passe côté utilisateur et rédacteur, mais définition d'une durée de validité de 3 mois pour les employés (modérateurs et administrateurs).
- Mise en place d'une demande de Mot de Passe avant chaque opération sensible. 
- Remise du facteur d'authentification (mdp) de façon sécurisée.
- Utilisation du mail pour le transfert d'information concernant le renouvellement du mot de passe.
- Limite de tentative d'authentification. 
- Ne pas afficher d'informations sur les raisons de l'échec d'une authentification. 

L'ensemble des propositions de ce guide (canal sécurisé TLS, authentification, ...) concernent également cette politique de gestion des mots de passe. 
  

## La sanitisation : 

La sanitisation des données correspond au nettoyage et à l'assainissement de celle-ci. 
Nettoyer les données que l'utilisateur entre est une étape nécessaire dans tout programme informatique et doit faire partie intégrante de notre stratégie de sécurisation. 

Toute demande de donnée passe par un _formulaire_. Ce formulaire est donc une porte d'entrée privilégiée vers nos données stockées côté serveur. 
Sans nettoyage des données, un utilisateur malveillant peut se servir de cette entrée afin de s'introduire dans notre application ou forcer celle-ci à afficher des informations sensibles. 

Plusieurs niveau de traitement des données sont disonibles : 
- Contrôle HTML : Ajout d'attributs HTML lors de la conception : size, min, max, required, ...
- Contrôle JS côté client: Reg-Exp, strict-mode, ... 
- Traitement de la donnée en arrière-plan : Désactivation et assainissement des potentielles injections SQL et Javascript.
  
Pour notre stratégie de sécurisation, la sanitisation se doit d'intervenir côté client et côté serveur, d'une part afin de fluidifier l'expérience utilisateur d'un utilisateur maladroit ne respectant pas les conditions, d'autre part pour empêcher en profondeur (avec un traitement plus lourd et donc plus long), un attaquant d'injecter de mauvaises commandes. Cette défense est particulièrement utile envers l'injection SQL et la XSS. 


## L'accès aux données : 

Dans le but de répondre au Product Backlog donné, mais également dans le but d'appliquer le principe de moindre privilège, une stratégie **RBAC** est à développer. 
RBAC veut dire _Role Based Access Control_, ce qui veut dire que les droits d'accès ne sont pas les mêmes selon le rôle qu'occupe l'utilisateur. 

Différents niveaux d'accès sont demandés dans le Product Backlog, en accord avec celui-ci nous proposons la hiérarchie suivante : 
- _Root_ : Correspond à un accès total à l'application, du code source aux bases de données. Nous proposons de limiter à un seul le profil Root.
- _Administrateur_ : Accès complet aux données hors code source et configurations techniques. Accès global au site depuis une interface administrateur. Possibilité d'ajout, modification ou suppression des informations concernant les modérateurs, rédacteurs et utilisateurs 
- _Modérateur_ : Réservé aux employés : Droit rédactionnel : Validation , ajout et modification des contenus rédactionnels présents sur le site. 
- _Rédacteur_ : Proposition de contenu rédactionnel, nous proposons de réserver ce rôle aux entreprises partenaires. Les propositions sont soumises aux modérateurs avant publication. 
- _Utilisateur_ : Public cible de l'application, les utilisateurs profitent des fonctionnalités de l'application avec peu de droits sur la modification des données. 

De façon générale, les données seront stockées selon une politique de sauvegarde stricte. 
De plus, les recommandations liées au _Règlement général sur la protection des données_ sera suivi.


## L'authentification : 

L'authentification permet de vérifier qu'une personne est bien celle qu'elle prétend être. 
Différentes stratégies d'authentification sont possibles. 
C'est une étape nécessaire dans la mise en plce du _RBAC_ et des _sessions_. 
Les sensibilités des données en jeu étant différentes en fonction des rôles utilisateurs, différents niveaux d'authentification sont nécessaires. 

L'authentification se déroule en trois phases : 
- Enregistrement
- Identification
- Authentification
Avant autorisation de l'accès. 

L'authentification est une zone sensible de l'application car elle est vulnérable aux attaques (Force brute, MITM; vol des moyens d'authentifications, ...)

Pour les profils _administrateurs_ et _modérateurs_, il est préférable d'opter pour une authentification forte. Le choix d'une double authentification est discutable car en pratique, le collaborateur doit disposer des deux moyens d'authentification à chaque connexion. Un PIN individuel ou MdP supplémentaire au couple Identifiant/MdP peut être intéressant. Celui-ci peut être basé sur un facteur de connaisance du collaborateur et défini par celui-ci lors de l'ouverture du compte par l'admin. 

Pour les profils _utilisateurs_ , nous mettons en place un couple motd de passe/identifiant. Le choix d'un mot de passe robuste (min 12 caractères, avec caractères spéciaux) sans délai d'expiration nous apparaît un meilleur choix au regard de la fluidité de l'expérience utilisateur. 


## Session et token : 

Lorsque l'authentification est réussie et dans la continuité du RBAC proposé, l'utilisateur est conduit sur une certaine session. La session peut être représenté comme un lieu dans l'application dans lequel l'utilisateur est limité en actions et interactions avec l'application. Il est important de comprendre qu'il ne s'agit ni d'un lieu virtuel ni d'un lieu réel, mais d'un espace défini dans le temps, dont les limites sont posées par les autorisations s'appliquant au profil. 
Il est dans ce contexte important de définir un temps de session à l'issue duquel l'utilisateur devra de nouveau s'authentifier. 

Dans le cas de notre application et afin de conserver une expérience utilisateur fluide, nous décidons de définir un temps de session long d'une journée. Ce choix arbitraire semble un compromis entre une expérience utilisateur fluide en laissant le temps de procéder aux démarches en jeu, tout en garantissant une certaine sécurité. De plus, notre application ne met pas en jeu de données sensibles concernant les utilisateurs, permettant ainsi un temps de session plus long. 

La session est mise en place par l'envoie en mémoire du navigateur côté client d'un cookie de session. 
Le **cookie** un fichier déposé en mémoire du navigateur. Il n'est en principe utilisable que par le site l'ayant déposé. Les cookies sont définis par le protocole HTTP et permettent de stocker des informations concernant le site en mémoire du navigzateur afin d'améliorer la navigation sur celui-ci (préférences, ...). L'information contenue dans le cookie est 

Dans notre cas, afin de faciliter l'expérience utilisateur en maintenant des sessions ouvertes, nous pouvons utiliser les cookies afin de contenir un identifiant de session unique. Lors des navigations futures, le navigateur enverra l'information dans ce cookie lors de la requête. Le serveur reconnaît l'authentification et permet l'accès au service. 

Attention, l'utilisation de cookies nécessite une bonne sécurisation globale de l'application. 


## La sécurisation des API

L'**API** pour _Application Programming Interface_ est une application dans notre application. C'est une partie de notre application (un microservice) qui permet l'accès à la base de donnée. Celle-ci peut constituer une porte d'entrée vers nos informations potentiellement sensibles en augmentant la surface d'attaque de notre application, celles-ci doivent donc être sécurisées. 
Elles sont elles-mêmes sensibles aux attaques courantes WSS, SQLI, MITM. 

L'application de la plupart des protocoles précédemment évoqué, comme l'utilisation d'un protocole HTTPS la définition des profils autorisatisés et l'utilisation de jetons d'identification permettent de sécuriser notre API. 

Ajoutons tout de même à ces protocoles deux recommandations : 
- La définition de quotas de limitation de requête. Sans impacter l'expérience utilisateur (ou admin ou modérateur), cette mesure permet de prévenir la multiplication de requêtes lors d'une attaque. Cette mesure permet également de mettre en place un historique de l'utilisation de l'API. 
- Utilisation d'une API **stateless**

## La journalisation : 

Une autre protection concevable est la **journalisation**. Celle-ci consiste à tenir un journal horodaté de toutes les actions se déroulant sur notre serveur. 
Il est en particulier utile dans le cadre de notre stratégie de sécurisation d'enregister les événements liés aux facteurs d'authentification afin de détecter les tentatives d'authentification frauduleuses (répétion de l'entrée dans le cadre d'une attaque par force brute). 

## Sécurisation en phase finale et en maintenance. 

Une fois notre application déployée, il est possible d'effectuer un audit PASSI, en particulier en cas de traitement d'informations sensibles. Cette certification **PASSI** pour _Prestataire d'audit des systèmes d'information_ est une certification permettanty d'attester de la fiabilité de l'application, mais également de la bonne formation du personnel. 

Un **Bug Bounty** est également envisageable, sous réserve de pouvoir offrir aux hackeurs éthiques une récompense attrayante. 


# Conclusion : 

Le développement d'une application passe par plusieurs étapes. Il est important de garder en tête qu'un processus de sécurisation est un processus global et englobant la totalité de la vie de l'application, du design à la maintenance. 

Des différentes stratégies de travail disponibles, nous pensons que la méthodologie Agile et en particulier Scrum permettra de maximiser la sécurisation de l'application en découpant sa production en de petits cycles dans lesquels la sécurité peut être évaluée et ré-évaluée de façon automatisée et systématique.
