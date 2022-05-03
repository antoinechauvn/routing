# routing

## Définitions 
`Convergence` : Le processus nécessaire à tous les routeurs d'un interréseau pour mettre à jour leurs tables de routage et créer une vue cohérente du réseau, en utilisant les meilleurs chemins possibles. Aucune donnée utilisateur n'est transmise pendant la convergence.

`Route par défaut` : une entrée de route "standard" dans une table de routage qui est utilisée comme première option. Tous les paquets envoyés par un appareil seront d'abord envoyés à la route par défaut. Si cela échoue, il essaiera des itinéraires alternatifs.

`Route statique` : une route permanente entrée manuellement dans une table de routage. Cette route restera dans le tableau, même si le lien tombe en panne. Il ne peut être effacé que manuellement.

`Route dynamique` : une entrée de route qui est mise à jour dynamiquement (automatiquement) à mesure que des modifications se produisent sur le réseau. Les routes dynamiques sont fondamentalement l'opposé des routes statiques.

# Approfondissement des protocoles de routage
![image](https://user-images.githubusercontent.com/83721477/166429844-5d6a3a30-08e3-4b58-aa22-246f37937880.png)

Les protocoles de passerelle extérieure ( EGP ) se trouvent entre les systèmes autonomes<br>
Les protocoles de passerelle intérieure ( IGP ) se trouvent dans les systèmes autonomes
![image](https://user-images.githubusercontent.com/83721477/166430025-8c92f085-9a8d-4abc-a9cf-872f8014b55e.png)


## Qu'est-ce qu'un protocole de routage?
```
Le protocole de routage est le mécanisme par lequel des chemins sont sélectionnés dans un réseau pour acheminer les données d'un
expéditeur jusqu'à un ou plusieurs destinataires
```

En fonction du nombre de destinataires et de la manière de délivrer le message, on distingue :
* unicast, qui consiste à acheminer les données vers une seule destination déterminée,
<img src="https://user-images.githubusercontent.com/83721477/166416539-81c06acf-455c-47b8-bd61-a9e780bda4a4.png" width=50% height=50%>

* broadcast qui consiste à diffuser les données à toutes les machines,
<img src="https://user-images.githubusercontent.com/83721477/166416582-137d9fe4-06b2-41f1-aef3-fed3393888e6.png" width=50% height=50%>

* multicast qui consiste à délivrer le message à l'ensemble des machines manifestant un intérêt pour un groupe,
<img src="https://user-images.githubusercontent.com/83721477/166416598-75fde3bb-4154-4d97-8a44-2028dc07c8a5.png" width=50% height=50%>

* anycast qui consiste à délivrer les données à n'importe quel membre d'un groupe, mais généralement le plus proche, au sein du réseau.
<img src="https://user-images.githubusercontent.com/83721477/166416623-5fb5ecf0-5133-4d81-b33c-5571cab39155.png" width=50% height=50%>


## Protocoles de routage à vecteur de distance
Les protocoles de routage à vecteur de distance utilisent des diffusions fréquentes (255.255.255.255 ou FF:FF:FF:FF)<br>
de l'intégralité de leur table de routage toutes les 30 secondes. sur toutes leurs interfaces afin de communiquer avec leurs voisins.<br>
Plus les tables de routage sont volumineuses, plus il y a de diffusions.<br>
Cette méthodologie limite considérablement la taille du réseau sur lequel Distance Vector peut être utilisé.<br>
Le routage à vecteur de distance est ainsi nommé car il implique deux facteurs : la distance et le vecteur<br>
Les informations de routage ne sont échangées qu'entre voisins directement connectés.<br>
Cela signifie qu'un routeur sait de quel voisin une route a été apprise, mais il ne sait pas où ce voisin a appris la route<br>
Un routeur ne peut pas voir au-delà de ses propres voisins.<br>
Cet aspect du routage à vecteur de distance est parfois appelé "routage par rumeur"

Les protocoles à vecteur de distance visualisent les réseaux en termes de routeurs adjacents et de nombre de sauts,<br>
ce qui se trouve également être la métrique utilisée.<br>
Le nombre de `sauts` (hop) (max de 15 pour RIP, 16 est considéré comme inaccessible et 255 pour IGMP), augmentera d'un à chaque fois que le paquet transite par un routeur.

###Exemple:

![image](https://user-images.githubusercontent.com/83721477/166432197-98345227-5a67-4f00-92be-a11d84ae9927.png)

* Les nombres que vous voyez sur le côté droit des interfaces sont les "nombres de sauts" qui, comme mentionné, est la métrique que les protocoles de vecteur de distance utilisent pour suivre à quelle distance se trouve un réseau.
* Puisque ces 2 réseaux sont connectés directement à l'interface du routeur, ils auront une valeur de 0 dans l'entrée de la table du routeur. La même règle s'applique à chaque routeur de notre exemple.
* Rappelez-vous que nous avons "juste allumer les routeurs", donc le réseau converge maintenant et cela signifie qu'aucune donnée n'est transmise.
* Quand je dis "pas de données", je veux dire les données de n'importe quel ordinateur ou serveur qui pourrait être sur l'un des réseaux.
* Pendant ce temps de "convergence", le seul type de données transmis entre les routeurs est celui qui leur permet de remplir leurs tables de routage et après cela, les routeurs transmettront tous les autres types de données entre eux.
C'est pourquoi un temps de convergence rapide est un grand avantage. <br>
L'un des problèmes avec RIP est qu'il a un temps de convergence lent.

![image](https://user-images.githubusercontent.com/83721477/166432297-d7935aaa-dcf1-4799-8b6f-e61f6898fb96.png)

Expliquons le schéma ci-dessus :

Dans l'image ci-dessus, le réseau est dit "convergé", c'est-à-dire que tous les routeurs du réseau ont rempli leur table de routage et sont parfaitement au courant des réseaux qu'ils peuvent contacter. Puisque le réseau est maintenant convergé, les ordinateurs de n'importe lequel des réseaux ci-dessus peuvent entrer en contact les uns avec les autres.

Encore une fois, en regardant l'une des tables de routage, vous remarquerez l'adresse réseau avec l'interface de sortie sur la droite et à côté se trouve le nombre de sauts vers ce réseau. N'oubliez pas que RIP ne comptera que jusqu'à 15 sauts, après quoi le paquet est rejeté (sur le saut 16).

Chaque routeur diffusera l'intégralité de sa table de routage toutes les 30 secondes.

Le routage basé sur le vecteur de distance peut causer beaucoup de problèmes lorsque les liaisons montent et descendent, cela peut entraîner des boucles infinies et peut également désynchroniser le réseau.

Des boucles de routage peuvent se produire lorsque chaque routeur n'est pas mis à jour à peu près au même moment.

Solutions:

SPLIT HORIZON
Fonctionne sur le principe qu'il n'est jamais utile de renvoyer des informations sur un routeur à la destination d'où provient le paquet d'origine. Donc si par exemple je t'ai raconté une blague, ça ne sert à rien que tu me répètes cette blague !

Dans notre exemple, cela aurait empêché le routeur A d'envoyer les informations mises à jour qu'il a reçues du routeur B au routeur B.

ROUTE POISONING
Alternative à l'horizon partagé, lorsqu'un routeur reçoit des informations sur une route d'un réseau particulier, le routeur annonce la route vers ce réseau avec la métrique de 16, indiquant que la destination est inaccessible.

Dans notre exemple, cela signifie que lorsque le réseau 5 tombe en panne, le routeur E initie l'empoisonnement du routeur en saisissant une entrée de table pour le réseau 5 sous la forme 16, ce qui signifie essentiellement qu'il est inaccessible. De cette façon, le routeur D n'est pas susceptible de recevoir des mises à jour incorrectes concernant la route vers le réseau 5. Lorsque le routeur D reçoit un empoisonnement du routeur du routeur E, il envoie une mise à jour appelée poison reverse au routeur E. Cela garantit que toutes les routes sur le segment a reçu les informations d'itinéraire empoisonné.

L'empoisonnement de route, utilisé avec les hold-down (voir section ci-dessous) accélérera certainement le temps de convergence car les routeurs voisins n'ont pas à attendre 30 secondes avant d'annoncer la route empoisonnée.

https://www.youtube.com/watch?v=40b1bM_y0Ng

## Protocoles de routage à état de lien
Le routage à état de liens, nécessite que tous les routeurs connaissent les chemins accessibles par tous les autres routeurs du réseau. Les informations d'état des liens sont inondées dans tout le domaine d'état des liens (une zone dans OSPF ou IS-IS) pour garantir que tous les routeurs possèdent une copie synchronisée de la base de données d'état des liens de la zone. À partir de cette base de données commune, chaque routeur construit sa propre arborescence relative des plus courts chemins, avec lui-même comme racine, pour toutes les routes connues.

### OSPF
OSPF est un protocole de routage sans classe , ce qui signifie que dans ses mises à jour, il inclut le sous-réseau de chaque route qu'il connaît, activant ainsi des masques de sous-réseau de longueur variable. Avec des masques de sous-réseau de longueur variable, un réseau IP peut être divisé en plusieurs sous-réseaux de différentes tailles. Cela offre aux administrateurs réseau une flexibilité de configuration réseau supplémentaire. Ces mises à jour sont multidiffusées à des adresses spécifiques (224.0.0.5 et 224.0.0.6).

Voici une liste des paquets OSPF les plus fréquemment utilisés :

* **Link State Advertisement** (LSA) : Le principal moyen de communication entre les routeurs OSPF, c'est le paquet qui transporte toutes les informations fondamentales sur la topologie et qui est inondé entre les zones pour effectuer différentes fonctions, il existe 11 types de paquets LSA.

* **Link State DataBase** (LSDB) : le paquet LSDB contient toutes les informations mises à jour sur l'état des liens échangées sur le réseau, et tous les routeurs d'une même zone ont une LSDB identique , et lorsque deux routeurs forment une nouvelle contiguïté voisine, ils synchronisent leur LSDB pour être entièrement adjacents .

* **Requête d'état de liaison** (LSR) : une fois que la contiguïté du voisin est formée et que la LSDB est échangée, les routeurs voisins peuvent localiser une information LSDB manquante, ils envoient ensuite un paquet de demande pour réclamer la pièce manquante, les voisins reçoivent ce paquet et répondent avec LSU .

* **Link State Update** (LSU) : Un paquet de réponse envoie une information spécifique LSDB demandée par un voisin OSPF via un paquet LSR .

* **Accusé de réception d'état de liaison** (LSAcK) : le routeur qui envoie le paquet LSR confirme la réception du LSU du voisin en envoyant un paquet de confirmation accusant réception des LSU demandés .


https://www.youtube.com/watch?v=sDnIRhiolp8

Exemple:<br>
![image](https://user-images.githubusercontent.com/83721477/166420552-750dc45a-0760-414e-a6b4-366caafe90e7.png)


![image](https://user-images.githubusercontent.com/83721477/166420147-b68e6c67-2e51-4203-8b88-cd237bd951c0.png)
