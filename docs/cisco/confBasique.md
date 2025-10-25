# Configuration d'un équipement Cisco :

## 1.Accés à distance sécurisé 

pour verifier si l'équipment prend en charge ssh l'équipement dopit reconnaitre cette commande : 

```
S1# show ip ssh
```

On configure le nom de domaine : 

```
S1(config)# ip domain-name test.local
```

On génere les paires RSA :

```
S1(config)# crypto key generate rsa
```

On configure l'utilisateur : 
```
S1(config)# username admin secret cisco
```

On configure les lignes de vty :
```
S1(config)# line vty 0 15 // pour configurer toutes les ligne ça dépend de l'équipement
S1(config-line)# transport input ssh
S1(config-line)# login local  //pour exiger une auth local en se basant sur la bdd local
S1(config-line)# exit
```

On active la version 2 : 
```
S1(config)# ip ssh version 2
```
---

## 2.Enregistrer ou supprimer la configuration 

### Enregistrement 
```
S1# copy running-config startup-config
```


### Suppression

---
## 3.Switch

### Séquence de démarrage d'un switch
le switch charge un programme de mise sous-tension **POST** stocké dans le ROM , ensuite le switch execute le bootloader (innitialisation de bas niveau du CPU, initialise des registres, le FS) et apres le boot loader charge le OS et donne le controle a l'OS.

Ppour voir sur fichier IOS on est : 
```
S1(config)# show boot
```


### configuration de base

Changement de nom :
```
S1(config)# hostname switch1
```

Sécuriser la ligne console avec un mdp 'cisco' : 
```
S1(config)# line console 0 
R1(config-if)# password cisco
R1(config-if)# login
R1(config-if)# exit
```

Ajout d'une banner : 
```
S1(config)# banner motd #authorized access only !#
```

### Port d'un switch

---
## 4.Routeur

### Configuation des interfaces
```
R1(config)# interface Fa0/0
R1(config-if)# ip address <@ IP> <mask>
R1(config-if)# description link to network 1 //si on veut ajuter une description
R1(config-if)# no shut
```

### Loopback interface


### Vérification 

Afficher un résumé des interfaces : 
```
R1# show ip interface brief
```

```
R1# show running-config interface <interface-name>
```

Afficher la table de routage : 
```
R1# show ip route
```






