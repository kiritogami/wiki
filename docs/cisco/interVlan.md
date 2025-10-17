# Configuration VLAN 

## Creation des VLAN

```
Switch> enable
Switch# conf t
Switch(config)# vlan 10
Switch(config-vlan)# name student  // pas obligatoire juste si on veut attribuer un nom
Switch(config-vlan)#end
```

## Attribution de ports

### Comment on affiche la liste des ports et vlan ?

```
Switch# show vlan brief
```
### Configuration

```
Switch# interface Fa0/12
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# no shutdown
Switch(config-if)# end
```

## Trunks

```
Switch(config)# interface Fa0/14
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk native vlan 99
Switch(config-if)# switchport trunk allowed vlan 10,20
Switch(config-if)#end
```

**Pour vérifier**
```
Switch#show interfaces Fa0/12 switchport
```

## Delete VLAN , Trunks , Acces port

**Avant de supprimer un vlan réattribuer d'abord tous les port**
```
Switch(config)# no vlan 20
```


```
Switch(config)# interface Fa0/12
Switch(config-if)# no switchport access vlan 
Switch(config-if)# end 
```

```
Switch(config)# interface Fa0/14
Switch(config-if)# no switchport trunk allowed vlan 
Switch(config-if)# no switchport trunk native vlan
Switch(config-if)# end  
```

## Routage Inter-VLAN (router-on-a-stick)

Pour chaque vlan routable on crée une sous interface

### Création de sous-interface
```
Routeur(config)#interface G0/0/0.10
Routeur(config-subif)#description default gateway for vlan 10
Routeur(config-subif)#encapsulation dot1Q 10
Routeur(config-subif)#ip add 192.168.10.1 255.255.255.0
```

**Activer le port à la fin**
```
Routeur(config)#interface G0/0/0
Routeur(config-if)#switchport mode trunk
Routeur(config-if)#no shut
Routeur(config-if)#end
```






