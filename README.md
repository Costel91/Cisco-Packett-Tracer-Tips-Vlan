# Cisco-Packett-Tracer-Tips-Vlan
Vlan/Inter Vlan (3 vlan : Ressource Humaine, Commerciaux, Comptabilité)

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
Sur notre topologie actuelle nous auront donc 3 switchs, vous allez sur votre 1er switch qui sera associé au Vlan 10 (Ressource Humaine)
```
