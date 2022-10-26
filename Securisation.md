Table des matières : 

- [Introduction](#introduction)
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
  - [Session, token et cookies :](#session-token-et-cookies-)
  - [La sécurisation technique : API, Javascript, ...](#la-sécurisation-technique--api-javascript-)
  - [La journalisation :](#la-journalisation-)
  - [Sécurisation en phase finale et en maintenance.](#sécurisation-en-phase-finale-et-en-maintenance)
- [Conclusion :](#conclusion-)

Proposition de stratégie de sécurisation d'une application web pour la mission locale du Valenciennois. 

# Introduction
## Que veut dire sécuriser un système ? 

Sécuriser un système, c'est le protéger de toute attaque. Chacune des applications web se doit d'être sécurisée. La plupart des attaques ne sont plus ciblées mais systématisées et automatisées (attaques "aléatoires" des applications où transitent des informations). 

Il est bien connu quInternet est un  système permettant la communication de plusieurs appareils (ordinateurs, serveurs, ...). Afin de communiquer, ces appareils s'échangent des paquets. Ces paquets doivent être sécurisés en terme de : 
- **Confidentialité**: Lorsqu'on pense sécurité informatique, on pense au cryptage des données. Il s'agit ici de chiffrer un message de façon à ce qu'un tiers attaquant ne soit pas en mesure de comprendre le message.
- **Authenticité**: Lorsque deux utilisateurs s'envoient des messages, il est important de vérifier que chacun des parti est bien celui qu'il prétend être afin de ne pas envoyer un message à un mauvais destinataire attaquant. 
- **Intégrité**: Le message envoyé doit être conservé dans sa totalité et ne pas être modifié en chemin par un attaquant. 
Nous verrons que ces trois points constituent les pilliers de toute stratégie de sécurisation.

L'entièreté de notre proposition de stratégie de sécurisation aura pour but de faire respecter ces trois pilliers de l'échange d'information. 

## Se prémunir, mais contre quelles attaques ? 

Avant d'aller plus loin dans la prévention, passons en revue les différentes attaques les plus souvent rencontrées afin de comprendre en amont les menaces qui pourront concerner notre application : 
- **Compromission des ressources** : La compromission des données est une attaque directe de l'intégrité du site ayant pour objectif de modifier les données de celui-ci. L'attaquant peut faire le choix de modifier les informations publiques du site, ou celui-ci peut tendre un piège aux visiteurs avec un processus nommé _watering hole_.
- **Vol de données** : Le vol des données est une attaque de la confidentialité des données (la plupart du temps utilisateur). L'attaquant vole des informations confidentielles le plus souvent afin de s'en servir. Nous reviendrons sur les différents types d'attaques en résultant. 
- **Déni de service** : Un attaquant tente de rendre inaccessible un site ou un service, la plupart du temps en le surchargeant comme avec l'attaque _DDOS_.

Ces familles d'attaques donnent lieu à ces attaques les plus courantes : 
- **Cross-Site-Scripting (XSS)**: C'est une injection de code dans le site puis dans le serveur. L'utilisateur est incité à faire ne requête web au nom d'un attaquant. 
- **Injection attacks (SQLI)**: Injection de commandes SQL afin de tester l'accès aux données 
- **CSRF** : C'est un type d'attaque par l'intermédiaire de l'utilisateur. La cible de l'attaque est l'application. Il est hautement conseillé pour un site officiel et institutionnel de se prévenir de ce type d'attaques. 

Le passage en revue de ces grandes familles d'attaques nous paraît un point nécessaire afin d'introduire ces termes qui seront utilisés tout au long de ce guide.

## Comment sécuriser une application ? 

vant la présentation de notre stratégie de sécurisation spécifique, arrêtons-nous une fois de plus sur les processus généraux permettant le respect de l'intégrité, de l'authenticité et de la confidentialité des échanges sur internet. 

1) **La défense en profondeur**: Une application, en particulier Web, utilise différents services pour son fonctionnement. Ces différents sevices qui composent le système sont autant de sous-systèmes à sécuriser individuellement. 
2) **Le moindre privilège**: Le moindre privilège permet de filtrer qui parmi les utilisateurs aura accès à certaines données. Plusieurs niveaux d'accès peuvent être définis parmi plusieurs sous-systèmes. 
3) **La réduction de la surface d'attaque**: La réduction de la surface d'attaque passe par la minimisation des données exposées, mais également par les bonnes pratiques de sauvegarde des données sensibles. Cette approche est complémentaire du moindre privilège. 

# Stratégie de sécurisation : 

## La protection navigateur : 

L'application en construction a pour but final d'être utilisée via Internet. Ce type d'application nécssite l'utilisation d'un navigateur afin d'accèder aux ressources souhaitées. Un premier niveau de protection par le navigateur peut être ici soulevé.

L'utilisation d'un navigateur pour utiliser notre application garantie l'utilisation du protocole **SOP** _Same Origin olicy_ : Ce protocole garantie que deux sites séparés ne peuvent pas communiquer entre eux (sur deux onglets par exemple). C'est une sécurité qui est globale et présente par défaut. 

Pour notre application, il peut être intéressant de contourner cette directive en définissant des **CORS** _Cross Origin Ressource Sharing. Ceux-ci permettent d'autoriser la communication de certains pages spécifiques.  

Enfin, afin de s'assurer le respect du principe de réduction de la surface d'attaque, notre application devra restreindre les données accessibles aux autres pages à l'aide des **CSP** _Constent Secure Policy_. 

Pour finir, la mise en place de **SRI** pour _Subressource integrity_ est également fortement conseillée afin de vérifier l'authenticité des ressources aux sources externes. Pour simplifier, lorsqu'une ressource extérieure est terminée, celle-ci est passée dans une fonction dite de hashage qui produit une certaine empreinte. Cette empreinte est mise à disposition afin que le navigateur compare l'exactitude de celle-ci avec la ressource reçue. SDi l'empreinte n'a pas changé, les éléments du fichier n'ont pas été modifiés par un attaquant malveillant. 

## Les protocoles de protection de l'échange de donnée : 

Notre application, comme toute application disponible sur navigateur, implique un échange de donnée entre la partie "client" (côté utilisateur) et la partie "serveur" (serveur physique, en quelque sorte la base de donnée). Cet échange entre un serveur et un client se fait par l'intermédiaire de paquets d'information selon le protocole **HTTP** pour _Hyper text transfer protocol_. 

Ce protocole HTTP est très limité car il ne permet pas le chiffrement des données qui transitent. Pour ce faire, il est absolument nécessaire pour notre application de mettre en place le protocole de chiffrement **TLS** qui chiffrera chaque paquet, rendant difficile (mais pas impossible) sa lecture par un potentiel attaquant. La mise en place de ce protocole est toutefois une grande défense contre les attaques de type _Man-In-The-Middle_. La mise en place du protocole TLS couplé au HTTP permet d'obtenir la certification **HTTPS**. 

Afin d'approfondir la sécurisation de l'échange des données, il est également nécessaire de mettre en place des **CTL** pour _Certificate transparency logs_, qui sont des certificats de transparence permettant l'obtention des sigles de sécurité. Leur durée limitée nécessite une certaine maintenance dans la ré-actuzalisation de ceux-ci. 

En parrallèle, le **HSTS** pour _HTTP Strict Transport Security_ doit être mis en place afin de rediriger toute communication HTTP vers une communicatiopn HTTPS. Une attention particulière doit toutefois être portée à la première visite de l'utilisateur sur notre application car ce protocole n'est pas encore mis en place. 

Ces protocoles de sécurité sont indispensable à toute navigation Internet, elles sont de plus une clé pour le référencement sur les moteurs de recherche. 

Il est à retenir que ces défenses sont nécessaires à la sécurisation d'une application mais non suffisantes. C'est pourquoi d'autres vulnérabilités sont à prendre en compte.  


## Hachage, salage et Mots de passe : 

Lorsque l'on parle de sécurisation des donnée, nous souhaitons rendre illisible l'information à tout potentiel attaquant. Pour cela, un message en clair est chiffré à l'aide de ce que l'on appelle une fonction de hachage. 
Cette **fonction de hachage** est une fonction mathématique qui fragmente et modifie un message de façon à ce que le résultat obtenu ne permette pas le retour au message originel. 

Néanmoins, la fonction de hachage comporte une faille: lorsque l'algorithme clé est trouvé, tout _input_ est facilement déchiffrable par l'attaquant. C'est pour cette raison qu'il est indispensable d'y adjoindre un salage. 
Le salage est une opération qui consiste à ajouter des mots ou des caractères aléatoires à l'empreinte obtenue par le hachage. Le tout est haché une nouvelle fois de façon à ce que l'on ne puisse plus trouver le messsage original, sauf si l'on connait le sel utilisé. 

Afin d'effectuer un salage efficace, nous ne connaitrons pas le processus de salage utilisé, seul le logiciel **BCrypt** qui sera utilisé connaîtra le salage utilisé. 


## La sanitisation : 

La sanitisation des données correspond au nettoyage de celle-ci. 
Nettoyer les données que l'utilisateur entre est une étape nécessaire dans tout programme informatique et doit faire partie intégrante de notre stratégie de sécurisation. 

Toute demande de donnée passe par un formulaire. Ce formulaire est donc une porte d'entrée privilégiée vers nos données stockées côté serveur. 
Sans nettoyage des données, un utilisateur malveillant peut se servir de cette entrée afin de s'introduire dans notre application ou forcer celle-ci à afficher des informations sensibles. 

Plusieurs niveau de traitement des données sont disonibles : 
- Contrôle HTML : Ajout d'attributs HTML lors de la conception : size, min, max, required, ...
- Contrôle JS côté client: Reg-Exp, strict-mode, ... 
- Traitement de la donnée en arrière-plan : Désactivation et assainissement des potentielles injections SQL et Javascript.
  
Pour notre stratégie de sécurisation, la sanitisation se doit d'intervenir côté client et côté serveur, d'une part afin de fluidifier l'expérience utilisateur d'un utilisateur maladroit ne respectant pas les conditions, d'autre part pour empêcher en profondeur (avec un traitement plus lourd et donc plus long), un attaquant d'injecter de mauvaises commandes. Cette défense est particulièrement utile envers l'injection SQL et la XSS. 


## L'accès aux données : 

Dans le but de répondre au cahier des charge donné, mais également dans le but d'appliquer le principe de moindre privilège, une stratégie **RBAC** est à développer. 
RBAC veut dire _Role Based Access Control_, ce qui veut dire que les droits d'accès ne sont pas les mêmes selon le rôle qu'occupe l'utilisateur. 

Différents niveaux d'accès sont demandés dans le cahier des charges, en accord avec celkui-ci nous proposons la hiérarchie suivante : 
- _Root_ : Correspond à un accès total à l'application, du code source aux bases de données. Nous proposons de limiter à un seul le profil Root.
- _Administrateur_ : Accès complet aux données hors code source et configurations techniques. Accès global au site depuis une interface administrateur. Possibilité d'ajout, modification ou suppression des informations concernant les modérateurs, rédacteurs et utilisateurs 
- _Modérateur_ : Réservé aux employés : Droit rédactionnel : Validation , ajout et modification des contenus rédactionnels présents sur le site. 
- _Rédacteur_ : Proposition de contenu rédactionnel, nous proposons de réserver ce rôle aux entreprises partenaires. Les propositions sont soumises aux modérateurs avant publication. 
- _Utilisateur_ : Public cible de l'application, les utilisateurs

## L'authentification : 

L'authentification permet de vérifgier qu'une poersonne est bien celle qu'elle prétend être. 
Différentes stratégies d'authentification sont possibles. 

Les sensibilités des données en jeu étant différentes en fonction des rôles utilisateur, différents niveaux d'authentification sont nécessaires. 

Pour les utilisateurs 
  
## Session, token et cookies : 

Lorsque l'authentification est réussie et dans la continuité du RBAC proposé, l'utilisateur est conduit sur une certaine session. La session est un lieu dans l'application dans lequel l'utilisateur est limité en action et interaction avec l'application. 
Il est également possible de définir un temps de session à l'issue duquel l'utilisateur devra de nouveau s'authentifier. 

Dans le cas de notre application et afin de conserver une expérience utilisateur fluide, nous décidons de définir un temps de session long d'une journée. Ce choix arbitraire semble un compromis entre une expérience utilisateur fluide en laissant le temps de procéder aux démarches en jeu, tout en garantissant une certaine sécurité. De plus, notre application ne met pas en jeu de données sensibles concernant les utilisateurs, permettant ainsi un temps de session plus long. 

## La sécurisation technique : API, Javascript, ... 


## La journalisation : 

Une autre protection concevable est la journalisation. Celle-ci consiste à tenir un journal horodaté de toutes les actions se déroulant sur notre serveur. 
Il est en particulier utile dans le cadre de notre stratégie de sécurisation d'enregister les événements liés aux facteurs d'authentification afin de détecter les tentatives d'authentification frauduleuses (répétion de l'entrée dans le cadre d'une attaque par force brute). 

## Sécurisation en phase finale et en maintenance. 

Une fois notre application déployée, il est possible d'effectuer un audit PASSI, en particulier en cas de traitement d'informations sensibles. Cette certification **PASSI** pour _Prestataire d'audit des systèmes d'information_ est une certification permettanty d'attester de la fiabilité de l'application, mais également de la bonne formation du personnel. 

Un **Bug Bounty** est également envisageable, sous réserve de pouvoir offrir aux hackeurs éthiques une récompense attrayante. 


# Conclusion : 

Le développement d'une application passe par plusieurs étapes. Il est important de garder en tête qu'un processus de sécurisation est un processus global et englobant la totalité de la vie de l'application, du design à la maintenance. 

Des différentes stratégies de travail disponibles, nous pensons que la méthodologie Agile et en particulier Scrum permettra de maximiser la sécurisation de l'application en découpant sa production en de petits cycles dans lesquels la sécurité peut être évaluée et ré-évaluée de façon automatisée et systématique.