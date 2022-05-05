# routing

## Définitions 
`Convergence` : Le processus nécessaire à tous les routeurs d'un interréseau pour mettre à jour leurs tables de routage et créer une vue cohérente du réseau, en utilisant les meilleurs chemins possibles. Aucune donnée utilisateur n'est transmise pendant la convergence.

`Route par défaut` : Lorsqu'il n'y a pas de route pour un réseau dans la table de routage, le traffic est acheminer via la route par défaut. En l'absence de route par défaut, le routeur éliminera un paquet dont la destination n'est pas connue.

`Route statique` : Une route statique est une route configurer manuellement dans la table de routage. Cette route restera dans le tableau.

`Route dynamique` : une entrée de route qui est mise à jour dynamiquement (automatiquement) à mesure que des modifications se produisent sur le réseau. Les routes dynamiques sont fondamentalement l'opposé des routes statiques.

# Approfondissement des protocoles de routage

## Qu'est-ce qu'un protocole de routage?
```
Le protocole de routage est le mécanisme par lequel des chemins sont sélectionnés dans un réseau pour
acheminer les données d'un expéditeur jusqu'à un ou plusieurs destinataires
```

## Déterminer comment un routeur prend une décision de transfert
### 1. Métrique

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

### 2. Distance administrative
La distance administrative est une condition de départage qui est utilisée lorsqu'il existe deux routes candidates ou plus de même longueur mais apprises via des protocoles de routage différents. Une seule version de ces routes vers le même réseau sera installée dans la table de routage.

La distance administrative est une valeur numérique préconfigurée de la `fiabilité d'une source d'informations de routage`.<br>
Les protocoles les plus préférés ont des numéros de distance administrative plus petits.

#### Distances administratives par défaut
![image](https://user-images.githubusercontent.com/83721477/166452673-a0ae8900-a7ec-4dd0-a2f4-3b3e86f1d7e6.png)
![image](https://user-images.githubusercontent.com/83721477/166452736-82c79f37-4c45-43bf-8133-a2dca30c1952.png)

#### Sélection d'un itinéraire en fonction de la distance administrative
![image](https://user-images.githubusercontent.com/83721477/166976638-d7c6843f-2dda-4df4-a287-e4ccb01cc92e.png)

### 3. Match le plus long

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

# Les différents protocoles de routages: 

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

## Protocoles de routage à état de lien
```
Les protocoles de routage à état de liaison ont une plus grande visibilité du réseau et sont généralement plus
puissants que les protocoles de routage à vecteur de distance. Un routeur exécutant des protocoles de routage
d'état de liaison a une "base de données" Link-State Database (LSDB)  et a ainsi une meilleure visibilité du réseau ou de la topologie.
À partir de cette base de données commune, chaque routeur construit sa propre arborescence relative des plus
courts chemins, avec lui-même comme racine, pour toutes les routes connues.
```

* Les protocoles de routage d'état de liaison inonderont le réseau avec ce qu'on appelle des LSA ou des annonces d'état de liens.<br>
* Les LSA sont propagés entre tous les routeurs sans être modifiés.
* Tous les routeurs créeront ou rempliront individuellement leur base de données ou Link-State Database (LSDB) qui est la même base de données sur tous les routeurs d'une zone.
* Les routeurs exécutent l'algorithme du chemin le plus court ou `SPF`. OSPF par exemple, est Open Shortest Path First ou en d'autres termes, c'est un standard ouvert qui exécute l'algorithme SPF.

## Notion de Classfull et Classless
* Les protocoles de routage `classful` n’envoient pas le masque de sous-réseau avec leurs mises à jour.<br>**Example: RIPv1, IGRP**
* Et les protocoles de routage `classless` envoient le masque de sous-réseau avec leurs mises à jour.<br>**Example: RIPv2, EIGRP, OSPF, IS-IS et BGP**
*Note: Les protocoles de routage à état de liens sont classless*

## EIGRP
EIGRP est un protocole à vecteur de distance propriétaire Cisco, mais utilise certaines fonctions d’un protocole à état de lien (nous verrons lesquelles). On dit de lui qu’il est hybride.

#### Avatanges de l'EIGRP:
* Convergence rapide, car il utilise l’algorithme DUAL. C’est moteur de calcul, qui permet à EIGRP de garantir des chemins sans boucle et même des itinéraires bis en cas de problème.<br>Et de plus, ces informations sont stockées dans sa propre table de routage.
* Si l'itinéraire principal tombe en panne, alors ce sera, l’itinéraire bis qui sera ajouté immédiatement à sa table de routage.<br>Et si par exemple, il n’y a aucun itinéraire de secours, alors il demandera directement à ses voisins de lui indiquer une nouvelle route.
* Équilibrage de charge, ce qui permet aux administrateurs de mieux répartir le trafic dans leurs réseaux.
* Protocole de routage sans classe : c’est-à-dire qu’il annonce un masque de sous réseau pour chaque destination.<br>Ce qui lui permet de supporter les masques de sous réseau variable, plus connu sous le nom de VLSM.
* Consomme très peu de bande passante, car il ne fait pas de mises à jour périodiques, et qu’il envoie uniquement la ou les routes qui ont été modifiées… contrairement à d’autres protocoles qui eux, envoient toutes leurs tables de routage, et ce, régulièrement.<br>De plus, EIGRP enverra ces modifications, uniquement aux routeurs qui sont concernés par ce changement. Ce qui réduit grandement la consommation de la bande passante. 
Ces mises à jour sont envoyées en multicast à l’aide de l’adresse 224.0.0.10.


https://www.youtube.com/watch?v=40b1bM_y0Ng


## OSPF


#### Voici une liste des paquets OSPF les plus fréquemment utilisés :

* **Link State Advertisement** (LSA) : Le principal moyen de communication entre les routeurs OSPF, c'est le paquet qui transporte toutes les informations fondamentales sur la topologie et qui est inondé entre les zones pour effectuer différentes fonctions, il existe 11 types de paquets LSA.

* **Link State DataBase** (LSDB) : le paquet LSDB contient toutes les informations mises à jour sur l'état des liens échangées sur le réseau, et tous les routeurs d'une même zone ont une LSDB identique , et lorsque deux routeurs forment une nouvelle contiguïté voisine, ils synchronisent leur LSDB pour être entièrement adjacents .

* **Requête d'état de liaison** (LSR) : une fois que la contiguïté du voisin est formée et que la LSDB est échangée, les routeurs voisins peuvent localiser une information LSDB manquante, ils envoient ensuite un paquet de demande pour réclamer la pièce manquante, les voisins reçoivent ce paquet et répondent avec LSU .

* **Link State Update** (LSU) : Un paquet de réponse envoie une information spécifique LSDB demandée par un voisin OSPF via un paquet LSR .

* **Accusé de réception d'état de liaison** (LSAcK) : le routeur qui envoie le paquet LSR confirme la réception du LSU du voisin en envoyant un paquet de confirmation accusant réception des LSU demandés .


https://www.youtube.com/watch?v=sDnIRhiolp8

### Exemple:<br>
![image](https://user-images.githubusercontent.com/83721477/166420552-750dc45a-0760-414e-a6b4-366caafe90e7.png)

# MEMO

![image](https://user-images.githubusercontent.com/83721477/166420147-b68e6c67-2e51-4203-8b88-cd237bd951c0.png)
![image](https://user-images.githubusercontent.com/83721477/166429844-5d6a3a30-08e3-4b58-aa22-246f37937880.png)

* Les protocoles de passerelle extérieure ( EGP ) se trouvent entre les systèmes autonomes<br>
* Les protocoles de passerelle intérieure ( IGP ) se trouvent dans les systèmes autonomes

![image](https://user-images.githubusercontent.com/83721477/166430025-8c92f085-9a8d-4abc-a9cf-872f8014b55e.png)

## Auto-summarization
![image](https://user-images.githubusercontent.com/83721477/166980689-593d51c2-9f1a-4359-bd90-700c68c3881d.png)

Si la fonction de récapitulation des routes est désactivée, le routeur R0 annoncera ces routes telles qu'elles sont. Le routeur R1 recevra ces routes annoncées sur son interface série et les ajoutera à sa table de routage.

![image](https://user-images.githubusercontent.com/83721477/166980847-df5c159c-4d54-44b2-8c4e-8a2c21a9026e.png)


Mais si l'`auto-summarization` est activée, au lieu d'annoncer quatre routes contiguës séparément, le routeur R0 annoncera une seule route résumée à partir de l'interface série. Le routeur R1 recevra également une seule route résumée et ajoutera cette route à sa table de routage.

![image](https://user-images.githubusercontent.com/83721477/166980881-f97b2b47-ecfe-4c64-8d21-5f39546330e6.png)

**Avantages**
* Il réduit la taille des mises à jour de routage.
* Cela réduit la taille de la table de routage
