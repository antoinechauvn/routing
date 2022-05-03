# routing

# Approfondissement des protocoles de routage
![image](https://user-images.githubusercontent.com/83721477/166416048-00d682e9-a067-48c5-989b-576b7855d5d8.png)

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
Les protocoles de routage à état de lien utilisent un algorithme beaucoup plus efficace. Ici, les routeurs construisent leurs tables de routage, en fonction du coût des différentes liaisons !
OSPF et ISIS sont des protocoles de routage à état de lien. Ils ont l’avantage d’avoir une convergence très rapide.


## Protocoles de routage à vecteur de distance
Les protocoles de routage à vecteur de distances permettent de construire des tables de routages sans aucune vision globale du réseau.
Le terme « vecteur » vient du faite, que le protocole manipule des tableaux vers les autres nœuds du réseau.
Ce sont des équipements qui ont plusieurs cartes réseau, dont chacune est reliée à un réseau différent.

Et le mot « distance » est le nombre de sauts qui lui permet d’atteindre les autres routeurs !
IGRP et RIP sont des protocoles de routage à vecteur de distance.

![image](https://user-images.githubusercontent.com/83721477/166242283-596a4756-9b32-42ec-b034-8a8838ef1cca.png)
![image](https://user-images.githubusercontent.com/83721477/166243346-d4a9c947-5cad-4922-a1d0-1d5a156be1f4.png)
![image](https://user-images.githubusercontent.com/83721477/166244056-2b7dd423-2815-4b56-9326-8d46bd2d01ec.png)
![image](https://user-images.githubusercontent.com/83721477/166244476-308d920a-1be6-4751-858c-681282e9b3d4.png)

![image](https://user-images.githubusercontent.com/83721477/166249510-229a94a1-c2b9-4cd9-9b3a-508fc4f710cf.png)
![image](https://user-images.githubusercontent.com/83721477/166253024-7ea7cf8a-9f4f-424a-ab12-547f9498c97e.png)
![image](https://user-images.githubusercontent.com/83721477/166261568-6d6be8af-0e72-46a3-98fe-8bf61eb90683.png)
