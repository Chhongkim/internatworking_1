# VLAN, EtherChannel & Inter-VLAN Routing Lab

## 1. Configure Hostname, Secret Password and VLANs

### S1
```
Switch>enable
Switch#configure terminal
Switch(config)#hostname YourName_S1
YourName_S1(config)#enable secret YourName
YourName_S1(config)#vlan 10
YourName_S1(config-vlan)#name Student
YourName_S1(config-vlan)#vlan 20
YourName_S1(config-vlan)#name Lecture
YourName_S1(config-vlan)#vlan 30
YourName_S1(config-vlan)#name IT
YourName_S1(config-vlan)#end
YourName_S1#write
```

### S2
```
Switch>enable
Switch#configure terminal
Switch(config)#hostname YourName_S2
YourName_S2(config)#enable secret YourName
YourName_S2(config)#vlan 10
YourName_S2(config-vlan)#name Student
YourName_S2(config-vlan)#vlan 20
YourName_S2(config-vlan)#name Lecture
YourName_S2(config-vlan)#vlan 30
YourName_S2(config-vlan)#name IT
YourName_S2(config-vlan)#end
YourName_S2#write
```

### S3
```
Switch>enable
Switch#configure terminal
Switch(config)#hostname YourName_S3
YourName_S3(config)#enable secret YourName
YourName_S3(config)#vlan 10
YourName_S3(config-vlan)#name Student
YourName_S3(config-vlan)#vlan 20
YourName_S3(config-vlan)#name Lecture
YourName_S3(config-vlan)#vlan 30
YourName_S3(config-vlan)#name IT
YourName_S3(config-vlan)#end
YourName_S3#write
```

### R1
```
Router>enable
Router#configure terminal
Router(config)#hostname YourName_R1
YourName_R1(config)#enable secret YourName
YourName_R1(config)#end
YourName_R1#write
```

## 2. Configure EtherChannel

### S1 (LACP to S2 + PAgP to S3)
```
YourName_S1>enable
YourName_S1#configure terminal
YourName_S1(config)#interface range fa0/1 - 2
YourName_S1(config-if-range)#channel-group 1 mode active
YourName_S1(config-if-range)#exit
YourName_S1(config)#interface range fa0/3 - 4
YourName_S1(config-if-range)#channel-group 2 mode desirable
YourName_S1(config-if-range)#end
YourName_S1#write
```

### S2 (LACP to S1 + LACP to S3)
```
YourName_S2>enable
YourName_S2#configure terminal
YourName_S2(config)#interface range fa0/1 - 2
YourName_S2(config-if-range)#channel-group 1 mode active
YourName_S2(config-if-range)#exit
YourName_S2(config)#interface range fa0/5 - 6
YourName_S2(config-if-range)#channel-group 3 mode active
YourName_S2(config-if-range)#end
YourName_S2#write
```

### S3 (PAgP to S1 + LACP to S2)
```
YourName_S3>enable
YourName_S3#configure terminal
YourName_S3(config)#interface range fa0/3 - 4
YourName_S3(config-if-range)#channel-group 2 mode desirable
YourName_S3(config-if-range)#exit
YourName_S3(config)#interface range fa0/5 - 6
YourName_S3(config-if-range)#channel-group 3 mode active
YourName_S3(config-if-range)#end
YourName_S3#write
```

## 3. Configure Trunking

### S1 (Port-Channels + Port to Router)
```
YourName_S1>enable
YourName_S1#configure terminal
YourName_S1(config)#interface port-channel 1
YourName_S1(config-if)#switchport mode trunk
YourName_S1(config-if)#switchport trunk allowed vlan 10,20,30
YourName_S1(config-if)#exit
YourName_S1(config)#interface port-channel 2
YourName_S1(config-if)#switchport mode trunk
YourName_S1(config-if)#switchport trunk allowed vlan 10,20,30
YourName_S1(config-if)#exit
YourName_S1(config)#interface gig0/1
YourName_S1(config-if)#switchport mode trunk
YourName_S1(config-if)#switchport trunk allowed vlan 10,20,30
YourName_S1(config-if)#end
YourName_S1#write
```

### S2
```
YourName_S2>enable
YourName_S2#configure terminal
YourName_S2(config)#interface port-channel 1
YourName_S2(config-if)#switchport mode trunk
YourName_S2(config-if)#switchport trunk allowed vlan 10,20,30
YourName_S2(config-if)#exit
YourName_S2(config)#interface port-channel 3
YourName_S2(config-if)#switchport mode trunk
YourName_S2(config-if)#switchport trunk allowed vlan 10,20,30
YourName_S2(config-if)#end
YourName_S2#write
```

### S3
```
YourName_S3>enable
YourName_S3#configure terminal
YourName_S3(config)#interface port-channel 2
YourName_S3(config-if)#switchport mode trunk
YourName_S3(config-if)#switchport trunk allowed vlan 10,20,30
YourName_S3(config-if)#exit
YourName_S3(config)#interface port-channel 3
YourName_S3(config-if)#switchport mode trunk
YourName_S3(config-if)#switchport trunk allowed vlan 10,20,30
YourName_S3(config-if)#end
YourName_S3#write
```

## 4. Configure Access Ports (PortFast + BPDU Guard)

### S2
```
YourName_S2>enable
YourName_S2#configure terminal
YourName_S2(config)#interface fa0/7
YourName_S2(config-if)#switchport mode access
YourName_S2(config-if)#switchport access vlan 10
YourName_S2(config-if)#spanning-tree portfast
YourName_S2(config-if)#spanning-tree bpduguard enable
YourName_S2(config-if)#exit
YourName_S2(config)#interface fa0/8
YourName_S2(config-if)#switchport mode access
YourName_S2(config-if)#switchport access vlan 20
YourName_S2(config-if)#spanning-tree portfast
YourName_S2(config-if)#spanning-tree bpduguard enable
YourName_S2(config-if)#exit
YourName_S2(config)#interface fa0/9
YourName_S2(config-if)#switchport mode access
YourName_S2(config-if)#switchport access vlan 30
YourName_S2(config-if)#spanning-tree portfast
YourName_S2(config-if)#spanning-tree bpduguard enable
YourName_S2(config-if)#end
YourName_S2#write
```

### S3
```
YourName_S3>enable
YourName_S3#configure terminal
YourName_S3(config)#interface fa0/9
YourName_S3(config-if)#switchport mode access
YourName_S3(config-if)#switchport access vlan 10
YourName_S3(config-if)#spanning-tree portfast
YourName_S3(config-if)#spanning-tree bpduguard enable
YourName_S3(config-if)#exit
YourName_S3(config)#interface fa0/8
YourName_S3(config-if)#switchport mode access
YourName_S3(config-if)#switchport access vlan 20
YourName_S3(config-if)#spanning-tree portfast
YourName_S3(config-if)#spanning-tree bpduguard enable
YourName_S3(config-if)#exit
YourName_S3(config)#interface fa0/7
YourName_S3(config-if)#switchport mode access
YourName_S3(config-if)#switchport access vlan 30
YourName_S3(config-if)#spanning-tree portfast
YourName_S3(config-if)#spanning-tree bpduguard enable
YourName_S3(config-if)#end
YourName_S3#write
```

## 5. Configure Router-on-a-Stick

### R1
```
YourName_R1>enable
YourName_R1#configure terminal
YourName_R1(config)#interface gig0/0
YourName_R1(config-if)#no shutdown
YourName_R1(config-if)#exit
YourName_R1(config)#interface gig0/0.10
YourName_R1(config-subif)#encapsulation dot1Q 10
YourName_R1(config-subif)#ip address 192.168.10.1 255.255.255.0
YourName_R1(config-subif)#exit
YourName_R1(config)#interface gig0/0.20
YourName_R1(config-subif)#encapsulation dot1Q 20
YourName_R1(config-subif)#ip address 192.168.20.1 255.255.255.0
YourName_R1(config-subif)#exit
YourName_R1(config)#interface gig0/0.30
YourName_R1(config-subif)#encapsulation dot1Q 30
YourName_R1(config-subif)#ip address 192.168.30.1 255.255.255.0
YourName_R1(config-subif)#end
YourName_R1#write
```

## 6. Configure Rapid-PVST+

### S1 (Root VLAN 10, Backup Root VLAN 20)
```
YourName_S1>enable
YourName_S1#configure terminal
YourName_S1(config)#spanning-tree mode rapid-pvst
YourName_S1(config)#spanning-tree vlan 10 root primary
YourName_S1(config)#spanning-tree vlan 20 root secondary
YourName_S1(config)#end
YourName_S1#write
```

### S2 (Root VLAN 20, Backup Root VLAN 10)
```
YourName_S2>enable
YourName_S2#configure terminal
YourName_S2(config)#spanning-tree mode rapid-pvst
YourName_S2(config)#spanning-tree vlan 20 root primary
YourName_S2(config)#spanning-tree vlan 10 root secondary
YourName_S2(config)#end
YourName_S2#write
```

### S3
```
YourName_S3>enable
YourName_S3#configure terminal
YourName_S3(config)#spanning-tree mode rapid-pvst
YourName_S3(config)#end
YourName_S3#write
```

## 7. Configure End Devices (PCs)

On each PC: **Desktop → IP Configuration → Static**

| PC | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| PC1 | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 |
| PC2 | 192.168.20.11 | 255.255.255.0 | 192.168.20.1 |
| PC3 | 192.168.30.11 | 255.255.255.0 | 192.168.30.1 |
| PC4 | 192.168.10.12 | 255.255.255.0 | 192.168.10.1 |
| PC5 | 192.168.20.12 | 255.255.255.0 | 192.168.20.1 |
| PC6 | 192.168.30.12 | 255.255.255.0 | 192.168.30.1 |

## 8. Verification

```
YourName_S1#show etherchannel summary
YourName_S1#show interfaces trunk
YourName_S1#show vlan brief
YourName_S1#show spanning-tree vlan 10
YourName_S2#show spanning-tree vlan 20
YourName_R1#show ip interface brief
```

Expected results:
- `show etherchannel summary` → Po1, Po2, Po3 flagged **(SU)** with member ports **(P)**
- `show interfaces trunk` → Port-Channels and Gig0/1 trunking with allowed VLANs 10, 20, 30
- `show spanning-tree vlan 10` → S1 displays **"This bridge is the root"**
- `show spanning-tree vlan 20` → S2 displays **"This bridge is the root"**

### Connectivity Tests (from PC1 Command Prompt)
```
C:\>ping 192.168.10.12
C:\>ping 192.168.20.11
C:\>ping 192.168.30.11
```
- Same-VLAN test: PC1 → PC4 must succeed
- Inter-VLAN tests: VLAN 10 → VLAN 20 and VLAN 10 → VLAN 30 must succeed

## Troubleshooting Notes

1. Configure `channel-group` on **both ends** of each link with compatible modes: `active ↔ active` for LACP, `desirable ↔ desirable` for PAgP.
2. If a Port-Channel shows **(SD)** in `show etherchannel summary`, issue `shutdown` then `no shutdown` on the physical interface range to force re-negotiation.
3. The first inter-VLAN ping may time out once due to ARP resolution — always ping at least twice before assuming a configuration error.
4. BPDU Guard will err-disable an access port if a switch is accidentally connected to it. Recover with `shutdown` / `no shutdown` after removing the offending device.
