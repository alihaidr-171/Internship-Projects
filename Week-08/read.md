# Week 8 – Routing & Inter-VLAN Routing

## Overview

In Week 8, I worked on **static routing, dynamic routing, VLANs, trunking, and inter-VLAN routing** using Cisco Packet Tracer.

The existing Week 5 enterprise topology was extended to demonstrate **Router-on-a-Stick inter-VLAN routing** using 802.1Q trunking and router subinterfaces.

## VLAN Configuration

| Department | Switch | VLAN |
| ---------- | ------ | ---: |
| Sales      | SW1    |   10 |
| IT         | SW3    |   20 |
| Finance    | SW4    |   30 |
| HR         | SW2    |   40 |

## Static Routing

Static routing requires routes to be manually configured by the administrator.

### Advantages

* Simple for small networks
* Easy to control
* Low resource usage

### Disadvantages

* Manual configuration
* Difficult to maintain in large networks
* Does not automatically adapt to topology changes

## Dynamic Routing

Dynamic routing uses routing protocols to automatically exchange routing information.

Common examples include:

* OSPF
* EIGRP
* RIP
* BGP

For larger enterprise networks, **dynamic routing such as OSPF** is commonly used because it provides better scalability and automatic route updates.

## Inter-VLAN Routing

Inter-VLAN routing allows devices in different VLANs to communicate through a Layer 3 device.

For this implementation:

* **VLAN 20 → IT**
* **VLAN 30 → Finance**

Router 2 was configured using **Router-on-a-Stick**.

```text
PC – VLAN 20
      |
     SW3
      |
  802.1Q Trunk
      |
     R2
   /     \
VLAN 20  VLAN 30
   |        |
 IT PC    Finance PC
```

## Router Configuration

### VLAN 20

```cisco
interface fa0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
```

### VLAN 30

```cisco
interface fa0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
```

## Trunk Configuration

SW3 Fa0/1 was configured as a trunk toward Router 2:

```cisco
interface fa0/1
 switchport mode trunk
 switchport trunk allowed vlan 20,30
```

## Verification

### Check VLANs

```cisco
show vlan brief
```

### Check trunk

```cisco
show interfaces trunk
```

### Check router subinterfaces

```cisco
show ip interface brief
```

Expected:

```text
Fa0/0.20    192.168.20.1    up    up
Fa0/0.30    192.168.30.1    up    up
```

### Check routing table

```cisco
show ip route
```

## Connectivity Test

A PC in VLAN 20 was able to ping a PC in VLAN 30:

```text
ping 192.168.30.10
```

Successful communication confirmed that **inter-VLAN routing was functioning correctly**.

## Key Learning

This week improved my practical understanding of:

* Static routing
* Dynamic routing
* VLAN segmentation
* 802.1Q trunking
* Router-on-a-Stick
* Inter-VLAN routing
* Cisco IOS verification and troubleshooting

## Conclusion

Week 8 focused on applying routing concepts in an enterprise network environment. The existing VLAN topology was extended with trunking and router subinterfaces, and successful connectivity between different VLANs verified the implementation.

