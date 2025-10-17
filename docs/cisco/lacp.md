# LACP (Link agregation control protocol) 

## configuration LACP
```
Switch(config)#interface range fa 0/1-2
Switch(config-if-range)# channel group 1 mode active
Switch(config-if-range)# exit
```
**Dans le cas si on veut la modifier on liaison trunk**

```
Switch(config)#interface port-channel 1
Switch(config-if)#switch port mode trunk
Switch(config-if)#switch port trunk allowed vlan 10,20
```

## Depannage LACP

```
Switch#show etherchannel summry
```


