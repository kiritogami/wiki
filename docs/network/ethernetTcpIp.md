## Ethernet

Le rôle de la couche 2 est de relier les machines qui sont dans le même réseau local.
Parmi les protocoles de la couche 2, on trouve  les deux plus utilisés : 
- WIFI
- Ethernet 

### Format d'une trame Ethernet 


``` 
+--------------------------------------------------------+
| MAC DEST | MAC SRC | Protocole Couche 3 | Data |  CRC  |
+--------------------------------------------------------+
```
- Le protocole de la couche 3 est codé sur 2 octets et le CRC sur 4 octets.
- La taille minimale d'une trame ethernet est 64 octets.
- La taille maximale d'une trame ethernet est 1518 octets.

## IP

Le rôle de la couche 3 est de relier les différentes réseaux.

### Format d'une trame IPv4

```
+-------------------------------------------------------------+
|  Ver.  |  HLEN  |  TOS   |    Longueur Totale (16 bits)     |
+-----+------+---------+--------------------------------------+
|  Identifiant (16 bits)   | Flags |    Décalage Fragment     |
+-------------------------------------------------------------+
| TTL | Protocole couche 4 |             Checksum             |
+-----------------------+-------------------------------------+
|                 Adresse Source IP                           |
+-------------------------------------------------------------+
|                 Adresse Destination IP                      |
+-------------------------------------------------------------+
| Options (Variable : 0 à 40 octets) |  Padding (Rembourrage) |
+------------------------------------+------------------------+
|                         Données                             |
+-------------------------------------------------------------+
```

## TCP

Le rôle de la couche 4 est de faire diagloguer les applicatif

### Format d'un segment TCP 

```
+-------------------------------------------------------------+
|        port source       |         Port destination         |
+-----+------+---------+--------------------------------------+
|                nombre de sequence                           |
+-------------------------------------------------------------+
|                     nombre ack                              |
+-------------------------------------------------------------+
|          flags           |          windows                 |
+-------------------------------------------------------------+
|        Checksum          |        Urgent Pointer            |  
+-------------------------------------------------------------+
| Options (Variable : 0 à 40 octets) |  Padding (Rembourrage) |
+------------------------------------+------------------------+
|                         Données                             |
+-------------------------------------------------------------+
```


