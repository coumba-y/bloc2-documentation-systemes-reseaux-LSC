# Bloc 2 - Administration des services - Situation 0 - Introduction aux services à mettre en place

**Compte rendu rédigé par** : BIDANESSY Coumba
**Formation** : BTS SIO 2e année - Option SISR  
**Établissement** : Lycée Paul-Louis Courier, Tours  

[toc]

## Segmentation VLSM

| **Intitulé des services** | **Nombre d'hôtes par service** |
|---------------------------|-------------------------------:| 
| Production                        | 120                         |
| Clients                      | 32                           |
| Administr sys et réseaux                       | 6                           |



Adressage pour clients (vlan 10) : 

- Masque : /26
- Adresse réseau : 192.168.1.128
- Adresse broadcast : 192.168.1.191
- Hôtes : 192.168.1.129 à 192.168.1.190
- Passerelle : 192.168.1.190



Adressage pour production (vlan 51): 

- Masque : /25
- Adresse réseau : 192.168.1.0
- Adresse broadcast : 192.168.1.127
- Hôtes : 192.168.1.1 à 192.168.1.126
- Passerelle : 192.168.1.126

Adressage pour administration systèmes et réseaux (vlan 20): 

- Masque : /28
- Adresse réseau : 192.168.1.192
- Adresse broadcast : 192.168.1.207
- Hôtes : 192.168.1.193 à 192.168.1.206
- Passerelle : 192.168.1.206

Adressage pour Bastion (vlan 57):

- Masque : /29 
- Adresse réseau : 192.168.1.208
- Adresse broadcast : 192.168.1.215
- Hôtes : 192.168.1.209 à 192.168.1.214
- Passerelle : 192.168.1.214

| **Intitulé des services** | **Nombre d'hôtes par service** |
|---------------------------|-------------------------------:|
| Production | 120 |
| Clients | 32 |
| Administr sys et réseaux | 6 |
| Bastions | 6 |


## Table de routage

Commutateur niveau 3 : 

| Type | Réseau destination | Masque          | Passerelle    | Adresse IP interface locale * |
| ---- | ------------------ | --------------- | ------------- | ----------------------------- |
| C    | 192.168.1.128        | /26   | 192.168.1.190| 192.168.1.190               |
| C    | 192.168.1.192     | /28 | 192.168.1.206  | 192.168.1.206                |
| C    | 192.168.1.0 | /25 | 192.168.1.126 | 192.168.1.126                 |
| C    | 192.168.11.248    | /29 | 192.168.11.253  | 192.168.11.253               |
| S    | 0.0.0.0    | /0 | 192.168.11.254  | 192.168.11.253                 |



Pare-feu : 

| Type | Réseau destination | Masque          | Passerelle    | Adresse IP interface locale * |
| ---- | ------------------ | --------------- | ------------- | ----------------------------- |
| C    | 192.36.1.0        | /24   | 192.36.1.254| 192.36.1.254              |
| C    | 192.36.253.0     | /24 | 192.36.253.10  | 192.36.253.10                  |
| C    | 192.168.11.248    | /29 | 192.168.11.254  | 192.168.11.254                |           |
|S| 192.168.1.0|/23|192.36.253.254|192.36.253.10
|S| 0.0.0.0|/0|192.168.11.253|192.168.11.254


Table NAT : 

| **Avant translation (NAT)** | **Avant translation (NAT)** | **Avant translation (NAT)** | **Avant translation (NAT)** | **Après translation (NAT)** | **Après translation (NAT)** | **Après translation (NAT)** | **Après translation (NAT)** |
|:---------------------------:|:---------------------------:|:---------------------------:|:---------------------------:|:---------------------------:|:---------------------------:|:---------------------------:|:---------------------------:|
| **IP source** | **Port source** | **IP destination** | **Port destination** | **IP source** | **Port source** | **IP destination** | **Port destination** |
| 192.168.1.0/24 | * | * | * | 192.36.253.10 | * | * | * |
| 192.168.11.248/29 | * | * | * | 192.36.253.10/24 | * | * | * |