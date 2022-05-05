# routing

## Définitions 
`Convergence` : Le processus nécessaire à tous les routeurs d'un interréseau pour mettre à jour leurs tables de routage et créer une vue cohérente du réseau, en utilisant les meilleurs chemins possibles. Aucune donnée utilisateur n'est transmise pendant la convergence.

`Route par défaut` : Lorsqu'il n'y a pas de route pour un réseau dans la table de routage, le traffic est acheminer via la route par défaut. En l'absence de route par défaut, le routeur éliminera un paquet dont la destination n'est pas connue.

`Route statique` : Une route statique est une route configurer manuellement dans la table de routage. Cette route restera dans le tableau.

`Route dynamique` : une entrée de route qui est mise à jour dynamiquement (automatiquement) à mesure que des modifications se produisent sur le réseau. Les routes dynamiques sont fondamentalement l'opposé des routes statiques.

# Approfondissement des protocoles de routage
![image](https://user-images.githubusercontent.com/83721477/166429844-5d6a3a30-08e3-4b58-aa22-246f37937880.png)

Les protocoles de passerelle extérieure ( EGP ) se trouvent entre les systèmes autonomes<br>
Les protocoles de passerelle intérieure ( IGP ) se trouvent dans les systèmes autonomes
![image](https://user-images.githubusercontent.com/83721477/166430025-8c92f085-9a8d-4abc-a9cf-872f8014b55e.png)

## Qu'est-ce qu'un protocole de routage?
```
Le protocole de routage est le mécanisme par lequel des chemins sont sélectionnés dans un réseau pour
acheminer les données d'un expéditeur jusqu'à un ou plusieurs destinataires
```

## Déterminer comment un routeur prend une décision de transfert
### Métrique

Les protocoles de routage dynamique calculent et utilisent une valeur numérique pour décrire le coût d'un chemin vers une destination.<br>
Ce nombre est appelé une `métrique` et il est spécifique à chaque protocole de routage.<br>
Les valeurs métriques de deux protocoles de routage différents ne sont pas comparées entre elles.<br>
Tous les protocoles de routage utilisent des propriétés différentes du chemin ou utilisent des calculs différents.<br><br>
Par exemple, certains protocoles utilisent une métrique simple comme le nombre de routeurs ou de sauts qu'un paquet doit traverser pour atteindre le réseau distant.<br>
Certains autres protocoles peuvent utiliser la bande passante comme coût de chemin.

#### Ce tableau répertorie les différents protocoles de routage et la métrique qu'ils utilisent.
![image](https://user-images.githubusercontent.com/83721477/166452273-d2f93ba5-9fca-412c-b45a-e1baaaa63b84.png)

#### Sélection d'une route à l'aide de la métrique du protocole de routage
![image](https://user-images.githubusercontent.com/83721477/166449098-67e1cfbf-823a-45b8-a574-278d97d20c8d.png)

### Distance administrative
La distance administrative est une condition de départage qui est utilisée lorsqu'il existe deux routes candidates ou plus de même longueur mais apprises via des protocoles de routage différents. Une seule version de ces routes vers le même réseau sera installée dans la table de routage.

La distance administrative est une valeur numérique préconfigurée de la `fiabilité d'une source d'informations de routage`.<br>
Les protocoles les plus préférés ont des numéros de distance administrative plus petits.

#### Distances administratives par défaut
![image](https://user-images.githubusercontent.com/83721477/166452673-a0ae8900-a7ec-4dd0-a2f4-3b3e86f1d7e6.png)
![image](https://user-images.githubusercontent.com/83721477/166452736-82c79f37-4c45-43bf-8133-a2dca30c1952.png)

#### Sélection d'un itinéraire en fonction de la distance administrative
![image](https://user-images.githubusercontent.com/83721477/166451961-d3968860-7301-4fb4-8ea4-a37399232c40.png)

### Match le plus long

La correspondance la plus longue fait référence au processus d'identification de la route vers le réseau le plus spécifique auquel le paquet correspond.
<br><br>
**La route la plus spécifique est une route hôte avec une longueur de préfixe de 32 (ou un masque de sous-réseau de 255.255.255.255).**
<br><br>
Par exemple, 192.168.100.25/32 est une route hôte et les paquets envoyés à cet hôte spécifique suivront toujours cette route.
<br>
La différence importante entre la correspondance la plus longue et les deux autres étapes est que le routeur compare deux routes différentes, l'une étant un sur-ensemble d'une autre.
<br>
Les deux réseaux apparaîtront dans la table de routage.
<br>
Une telle situation existe souvent lorsqu'un récapitulatif est effectué dans le réseau, c'est-à-dire le processus de combinaison de plusieurs routes en une seule.

#### Sélection du meilleur itinéraire en fonction de la correspondance la plus longue (donc la plus précise)
![image](https://user-images.githubusercontent.com/83721477/166452014-de5cca83-37fc-468d-9889-5d0172e43263.png)


## Protocoles de routage à vecteur de distance
```
Les protocoles de routage à vecteur de distance utilisent des diffusions fréquentes
(255.255.255.255 ou FF:FF:FF:FF) de l'intégralité de leur table de routage toutes les 30 secondes.
sur toutes leurs interfaces afin de communiquer avec leurs voisins. Plus les tables de routage
sont volumineuses, plus il y a de diffusions. Cette méthodologie limite considérablement la taille
du réseau sur lequel Distance Vector peut être utilisé.
```

#### Le routage à vecteur de distance est ainsi nommé car il implique deux facteurs : la distance et le vecteur<br><br>

* Les informations de routage ne sont échangées qu'entre voisins directement connectés.
* Cela signifie qu'un routeur sait de quel voisin une route a été apprise, mais il ne sait pas où ce voisin a appris la route
* Un routeur ne peut pas voir au-delà de ses propres voisins.
* Cet aspect du routage à vecteur de distance est parfois appelé `routage par rumeur`

Les protocoles à vecteur de distance visualisent les réseaux en termes de routeurs adjacents et de nombre de sauts,<br>
ce qui se trouve également être la métrique utilisée.<br>
Le nombre de `sauts` (hop) (max de 15 pour RIP, 16 est considéré comme inaccessible et 255 pour IGMP), augmentera d'un à chaque fois que le paquet transite par un routeur.

### Exemple:

![image](https://user-images.githubusercontent.com/83721477/166432197-98345227-5a67-4f00-92be-a11d84ae9927.png)

* Les nombres que vous voyez sur le côté droit des interfaces sont les `nombres de sauts` qui, comme mentionné, est la métrique que les protocoles de vecteur de distance utilisent pour suivre à quelle distance se trouve un réseau.
* Puisque ces 2 réseaux sont connectés directement à l'interface du routeur, ils auront une valeur de 0 dans l'entrée de la table du routeur. La même règle s'applique à chaque routeur de notre exemple.
* Rappelez-vous que nous avons "juste allumer les routeurs", donc le réseau converge maintenant et cela signifie qu'aucune donnée n'est transmise.
* Quand je dis "pas de données", je veux dire les données de n'importe quel ordinateur ou serveur qui pourrait être sur l'un des réseaux.
* Pendant ce temps de `convergence`, le seul type de données transmis entre les routeurs est celui qui leur permet de remplir leurs tables de routage et après cela, les routeurs transmettront tous les autres types de données entre eux.
C'est pourquoi un temps de convergence rapide est un grand avantage. <br>
L'un des problèmes avec RIP est qu'il a un temps de convergence lent.

![image](https://user-images.githubusercontent.com/83721477/166432297-d7935aaa-dcf1-4799-8b6f-e61f6898fb96.png)

Expliquons le schéma ci-dessus :

* Dans l'image ci-dessus, le réseau est dit `convergé`, c'est-à-dire que tous les routeurs du réseau ont rempli leur table de routage et sont parfaitement au courant des réseaux qu'ils peuvent contacter.
* Puisque le réseau est maintenant convergé, les ordinateurs de n'importe lequel des réseaux ci-dessus peuvent entrer en contact les uns avec les autres.
* Encore une fois, en regardant l'une des tables de routage, vous remarquez l'adresse réseau avec l'interface de sortie sur la droite et à côté se trouve le nombre de sauts vers ce réseau. 

**N'oubliez pas que RIP ne comptera que jusqu'à 15 sauts, après quoi le paquet est rejeté (sur le saut 16).**

* Chaque routeur diffusera l'intégralité de sa table de routage toutes les 30 secondes.
* Le routage basé sur le vecteur de distance peut causer beaucoup de problèmes lorsque les liaisons montent et descendent, cela peut entraîner des boucles infinies et peut également désynchroniser le réseau.
Des boucles de routage peuvent se produire lorsque chaque routeur n'est pas mis à jour à peu près au même moment.

### Solutions:

## SPLIT HORIZON
Fonctionne sur le principe qu'il n'est jamais utile de renvoyer des informations sur un routeur à la destination d'où provient le paquet d'origine. Donc si par exemple je t'ai raconté une blague, ça ne sert à rien que tu me répètes cette blague !

Dans notre exemple, cela aurait empêché le routeur A d'envoyer les informations mises à jour qu'il a reçues du routeur B au routeur B.

## ROUTE POISONING
Alternative à l'horizon partagé, lorsqu'un routeur reçoit des informations sur une route d'un réseau particulier, le routeur annonce la route vers ce réseau avec la métrique de 16, indiquant que la destination est inaccessible.

Dans notre exemple, cela signifie que lorsque le réseau 5 tombe en panne, le routeur E initie l'empoisonnement du routeur en saisissant une entrée de table pour le réseau 5 sous la forme 16, ce qui signifie essentiellement qu'il est inaccessible. De cette façon, le routeur D n'est pas susceptible de recevoir des mises à jour incorrectes concernant la route vers le réseau 5. Lorsque le routeur D reçoit un empoisonnement du routeur du routeur E, il envoie une mise à jour appelée poison reverse au routeur E. Cela garantit que toutes les routes sur le segment a reçu les informations d'itinéraire empoisonné.

L'empoisonnement de route, utilisé avec les hold-down (voir section ci-dessous) accélérera certainement le temps de convergence car les routeurs voisins n'ont pas à attendre 30 secondes avant d'annoncer la route empoisonnée.


### EIGRP
Le protocole de routage dynamique propriétaire EIGRP est la solution préférée dans les infrastructures Cisco Systems. EIGRP est un protocole de routage dynamique intérieur hautement fonctionnel. Il converge très rapidement et il est multi-protocoles IPv4/IPv6. Il permet de contrôler finement la métrique de manière à influencer les entrées de la table de routage. EIGRP est alors capable de répartir la charge de trafic sur des liaisons à coûts inégaux.

https://www.youtube.com/watch?v=40b1bM_y0Ng

## Protocoles de routage à état de lien
```
Le routage à état de liens, nécessite que tous les routeurs connaissent les chemins accessibles par tous les autres
routeurs du réseau. Les informations d'état des liens sont inondées dans tout le domaine d'état des liens
(une zone dans OSPF ou IS-IS) pour garantir que tous les routeurs possèdent une copie synchronisée de la base de
données d'état des liens de la zone. À partir de cette base de données commune, chaque routeur construit sa propre
arborescence relative des plus courts chemins, avec lui-même comme racine, pour toutes les routes connues.
```

### OSPF
OSPF est un protocole de routage sans classe , ce qui signifie que dans ses mises à jour, il inclut le sous-réseau de chaque route qu'il connaît, activant ainsi des masques de sous-réseau de longueur variable. Avec des masques de sous-réseau de longueur variable, un réseau IP peut être divisé en plusieurs sous-réseaux de différentes tailles. Cela offre aux administrateurs réseau une flexibilité de configuration réseau supplémentaire. Ces mises à jour sont multidiffusées à des adresses spécifiques (224.0.0.5 et 224.0.0.6).

#### Voici une liste des paquets OSPF les plus fréquemment utilisés :

* **Link State Advertisement** (LSA) : Le principal moyen de communication entre les routeurs OSPF, c'est le paquet qui transporte toutes les informations fondamentales sur la topologie et qui est inondé entre les zones pour effectuer différentes fonctions, il existe 11 types de paquets LSA.

* **Link State DataBase** (LSDB) : le paquet LSDB contient toutes les informations mises à jour sur l'état des liens échangées sur le réseau, et tous les routeurs d'une même zone ont une LSDB identique , et lorsque deux routeurs forment une nouvelle contiguïté voisine, ils synchronisent leur LSDB pour être entièrement adjacents .

* **Requête d'état de liaison** (LSR) : une fois que la contiguïté du voisin est formée et que la LSDB est échangée, les routeurs voisins peuvent localiser une information LSDB manquante, ils envoient ensuite un paquet de demande pour réclamer la pièce manquante, les voisins reçoivent ce paquet et répondent avec LSU .

* **Link State Update** (LSU) : Un paquet de réponse envoie une information spécifique LSDB demandée par un voisin OSPF via un paquet LSR .

* **Accusé de réception d'état de liaison** (LSAcK) : le routeur qui envoie le paquet LSR confirme la réception du LSU du voisin en envoyant un paquet de confirmation accusant réception des LSU demandés .


https://www.youtube.com/watch?v=sDnIRhiolp8

### Exemple:<br>
![image](https://user-images.githubusercontent.com/83721477/166420552-750dc45a-0760-414e-a6b4-366caafe90e7.png)

<hr>

![image](https://user-images.githubusercontent.com/83721477/166420147-b68e6c67-2e51-4203-8b88-cd237bd951c0.png)
