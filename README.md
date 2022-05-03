# routing

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

## Protocoles de routage à vecteur de distance
**Routing by Rumor**
```
Le routage à vecteur de distance est ainsi nommé car il implique deux facteurs : la distance , ou métrique, d'une destination,
et le vecteur , ou direction à prendre pour s'y rendre. Les informations de routage ne sont échangées qu'entre voisins
directement connectés. Cela signifie qu'un routeur sait de quel voisin une route a été apprise, mais il ne sait pas où ce voisin
a appris la route ; un routeur ne peut pas voir au-delà de ses propres voisins. Cet aspect du routage à vecteur de distance est
parfois appelé "routage par rumeur". Des mesures telles que l'horizon partagé et l'inversion du poison sont utilisées pour éviter
les boucles de routage.

```

https://www.youtube.com/watch?v=40b1bM_y0Ng

![image](https://user-images.githubusercontent.com/83721477/166420147-b68e6c67-2e51-4203-8b88-cd237bd951c0.png)
