# Cisco-Packett-Tracer-Tips-Vlan
Vlan/Inter Vlan (3 vlan : Ressource Humaine, Commerciaux, Comptabilité)


![Vlan](https://user-images.githubusercontent.com/22075822/153721643-c9c533dc-4eff-4828-b82d-c0aaad3388e1.JPG)

Vlan : est un type de réseau local virtuel, il regroupe de manière logique et indépendante un ensemble de machines informatiques, on peut en avoir plusieurs qui cohabitent sur le même commutateur réseau.

## Procédure Vlan
Afin de pouvoir ségmenter votre réseau dans divers Groupe/Service grâce à des Vlan et un routage Inter Vlan, je vais vous lister les étapes, les commandes, et le résultat obtenue.
### Création des Vlan
Il faudra vous placer sur chaque commutateur réseau et tapez les commandes suivantes :

``` conf t 
vlan 10 
name Ressource Humaine
``` 
``` conf t
vlan 20
name Commerciaux
```
``` conf t
vlan 30
name Comptabilité
```
Vous avez donc bien vos 3 Vlan qui ont été ajoutés dans la topologie de votre réseau.
### Configuration des interfaces par rapport au Vlan associé
Sur notre topologie actuelle nous auront donc 3 switchs, vous allez sur votre 1er switch qui sera associé au Vlan 10 (Ressource Humaine).
``` conf t
int fa0/11 (int = interface) 
switchport mode access (force le port à être un port d'accès tandis que tout périphérique branché sur ce port ne pourra communiquer qu'avec d'autres périphériques qui se trouvent dans le même Vlan)
switchport mode access vlan 10
no sh (permet d'activer l'int)
```
Il faudra donc répéter cette action , en mettant bien le Vlan correspondant sur vos 2 switchs restants.
``` conf t
int fa0/18
switchport mode access
switchport mode access vlan 20
no sh
```
``` conf t 
int fa0/6
switchport mode access
switchport mode access vlan 30
no sh 
```
Pour vérifier ci nos Vlan sont bien associée au switch voulus, vous pouvez tapez cette commande
```
sh vlan brief (Show vlan brief : ceux qui va répertorié toute vos Vlan active)
```
Nous avons donc bien associé nos Vlan a nos switchs nous pouvons passez l'étapes suivantes.
### Mode Trunk
Le protocole Trunk va vous permettre de faire communiquer vos Vlan entres-elles, car pour le moment la vlan 10 ne peut pas communiquer entre les autres poste de cette même Vlan et pareil pour les deux autres Vlan. Pour ce faire allez sur le switch 1 par exemple il faudra dans tout les cas refaire la même manipulation sur les 3 switches de notre topologie.
``` conf t
int fa0/1
switchport mode trunk
no sh
```
Après avoir tapez cette commande sur les 3 switches nous pourront donc faire un ping entre deux postes de deux Vlan associé (PC2 peut ping PC5) mais la Vlan 10 ne peut pas communiquer avec la Vlan 20 et la Vlan 30 et inversement.
### Adressage IP
Vous pouvez passer a l'adressage IP sur les postes comme sur la photo ci dessus.

# Félécitation vos Vlan sont opérationnels


Ce n'est pas fini, si vous souhaitez que vos Vlan puissent communiquer entres elles il faudra faire un routage inter-Vlan
Nous allons donc ajouter un routeur a notre topologie, ne pas oublier d'allumé le routeur.
Se placer sur le switchs qui sera en contact du routeur et activer le routage IP :
``` conf t
routage ip
no sh
```
## Création des Subnet (Sous-réseaux)
Il faudras crée 3 sous-réseaux car nous avons 3 Vlan, qui nous serviront de passerelle (Gateway) entre nos postes clients.
Se placez sur le routeur : 
``` conf t 
int fa0/0.1 (Vlan 10)
conf t 
int fa0/0.2 (Vlan 20)
conf t
int fa0/0.3 (Vlan 30)
```
3 nouveaux sous-réseaux seront donc disponible a partir de maintenant, si vous passez votre souris sur routeurs vous verrez bien vous 3 sous réseaux.
## Encapsulation DOT1Q
Nous allons configurer notre routeur afin qu'il serve de jonction pour nos Vlan, pour que nos Vlan puissent communiquer entres-elles.
Pour ce faire nous allons encapsulé nos Vlan dans les sous-réseaux que l'on vient de crées :
``` conf t
int fa0/0.1
encapsulation dot1Q vlan 10
no sh
```
``` conf t
int fa0/0.2
encapsulation dot1Q vlan 20
no sh
```
``` conf t
int fa0/0.3
encapsulation dot1Q vlan 30
no sh
```
L'encapsulation de nos 3 Vlan dans nos sous-réseaux a normalement fonctionné.

## Adresse IP des sous interfaces
Nous allons crée des adresse IP pour les sous-réseaux que l'on viens de crée sur le routeur, pour ce faire allez sur le routeur :
``` conf t
int fa0/0.1
ip add ""."".""."" (Mettre les adresses par rapport a votre topologie)
no sh 
```
Répéter la commande sur les 3 sous-réseaux que l'on a crées
``` conf t
int fa0/0.2
ip add ""."".""."".
no sh
```
``` conf t
int fa0/0.3
ip add "".""."".""
no sh
```
Ces adresses que l'on vient de crée va nous servir de gateway à mette dans la configuration des poste clients afin de pouvoir faire des ping entre tout les postes de toute les Vlan disponible

### Voilà fini ;)

Vous devriez si vous avez suivis toutes ces étapes, avoir une topologie ressemblant a ceci :
![Ok vlan](https://user-images.githubusercontent.com/22075822/153722984-dbd118d4-49f5-4d73-be1c-64d7a31598f5.JPG)
