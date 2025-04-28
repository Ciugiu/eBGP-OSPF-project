# Networking Project Documentation: GNS3 Topology with eBGP, OSPF, and Loopback Communication
## Introduction

[Presentation](https://www.canva.com/design/DAGkbdOKMS0/sYg-WYodXGnHQQgGjsbVlQ/edit?utm_content=DAGkbdOKMS0&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)

This documentation covers the implementation of a network topology consisting of five routers (R1–R5) designed to achieve full reachability, including communication between routers and loopbacks. The project uses eBGP for external routing, OSPF for internal routing, and includes redundancy using BFD as well as BGP-to-OSPF redistribution. The primary goal is to ensure that all routers and loopback interfaces can communicate seamlessly.

### Topology Overview

The network consists of the following components:

- **Routers**: R1, R2, R3, R4, and R5.
    
- **Loopback Interfaces**: Each router has one or more loopback interfaces configured to simulate endpoints within the network.
    
- **Protocols**: eBGP (for external routing) and OSPF (for internal routing) are used.
    
- **BGP-to-OSPF Redistribution**: Ensures connectivity between the two routing domains.
## Network Configuration

### Router Configuration

Each router in the network needs a basic configuration, including: 
- Setting up the router hostname
- Interfaces
- Basic IP addressing
as well as: 
- Configuring eBGP on R1, R2, R3 and R4
- Configuring OSPF on R5 and R3
- Authentication for BGP and OSPF
- Configuring Loopback Interfaces
- BGP-to-OSPF Redistribution
- Redundancy

#### Configuration for Router R1:
```
R1#
R1#**conf t**
R1(config)#**hostname R1**
R1(config)#**int g1/0**
R1(config-interface)#**description B2B to R2**
R1(config-interface)#**ip add 10.1.1.1 255.255.255.252**
R1(config-interface)#**no sh**
R1(config-interface)#**exit**
R1(config)#**int g2/0**
R1(config-interface)#**description B2B to R4**
R1(config-interface)#**ip add 10.1.4.2 255.255.255.252**
R1(config-interface)#**no sh**
R1(config-interface)#**exit**
R1(config)#**int loopback 1**
R1(config-interface)#**ip add 10.1.1.4 255.255.255.255**
R1(config-interface)#**no sh**
R1(config-interface)#**exit**
R1(config)#**route-map SET_LOCAL_PREF_FOR_R2 permit 10**
R1(config-route-map)#**set local-preference 200**
R1(config-route-map)#**exit**
R1(config)#**route-map SET_LOCAL_PREF_FOR_R4 permit 10**
R1(config-route-map)#**set local-preference 100**
R1(config-route-map)#**exit**
R1(config)#**router bgp 1**
R1(config-router)#**neighbor 10.1.1.2 remote-as 2**
R1(config-router)#**neighbor 10.1.1.2 description eBGP to R2**
R1(config-router)#**neighbor 10.1.1.2 password G00Dluck**
R1(config-router)#**network 10.1.1.0 mask 255.255.255.252**
R1(config-router)#**neighbor 10.1.1.2 route-map SET_LOCAL_PREF_FOR_R2 in**
R1(config-router)#**neighbor 10.1.4.1 remote-as 4**
R1(config-router)#**neighbor 10.1.4.1 description eBGP to R4**
R1(config-router)#**neighbor 10.1.4.1 password G00Dluck**
R1(config-router)#**network 10.1.4.0 mask 255.255.255.252**
R1(config-router)#**neighbor 10.1.4.1 route-map SET_LOCAL_PREF_FOR_R4 in**
R1(config-router)#**network 10.1.1.4 mask 255.255.255.255**
R1(config-router)#**bgp router-id 10.1.1.4**
R1(config-router)#**end**
R1#**wr**
R1#
```


#### Configuration for Router R2:
```
R2#
R2#**conf t**
R2(config)#**hostname R2**
R2(config)#**int g1/0**
R2(config-interface)#**description B2B to R1**
R2(config-interface)#**ip add 10.1.1.2 255.255.255.252**
R2(config-interface)#**no sh**
R2(config-interface)#**exit**
R2(config)#**int g2/0**
R2(config-interface)#**description B2B to R3**
R2(config-interface)#**ip add 10.1.2.1 255.255.255.252**
R2(config-interface)#**no sh**
R2(config-interface)#**exit**
R2(config)#**int loopback 2**
R2(config-interface)#**ip add 10.1.2.4 255.255.255.255**
R2(config-interface)#**no sh**
R2(config-interface)#**exit**
R2(config)#**route-map SET_LOCAL_PREF_FOR_R3 permit 10**
R2(config-route-map)#**set local-preference 200**
R2(config-route-map)#**exit**
R2(config)#**route-map SET_LOCAL_PREF_FOR_R1 permit 10**
R2(config-route-map)#**set local-preference 100**
R2(config-route-map)#**exit**
R2(config)#**router bgp 2**
R2(config-router)#**neighbor 10.1.1.1 remote-as 1**
R2(config-router)#**neighbor 10.1.1.1 description eBGP to R1**
R2(config-router)#**neighbor 10.1.1.1 password G00Dluck**
R2(config-router)#**network 10.1.1.0 mask 255.255.255.252**
R2(config-router)#**neighbor 10.1.1.1 route-map SET_LOCAL_PREF_FOR_R1 in**
R2(config-router)#**neighbor 10.1.2.2 remote-as 3**
R2(config-router)#**neighbor 10.1.2.2 description eBGP to R3**
R2(config-router)#**neighbor 10.1.2.2 password G00Dluck**
R2(config-router)#**network 10.1.2.0 mask 255.255.255.252**
R2(config-router)#**neighbor 10.1.2.2 route-map SET_LOCAL_PREF_FOR_R3 in**
R2(config-router)#**network 10.1.2.4 mask 255.255.255.255**
R2(config-router)#**bgp router-id 10.1.2.4**
R2(config-router)#**end**
R2#**wr**
R2#
```

#### Configuration for Router R3:
```
R3#
R3#**conf t**
R3(config)#**hostname R3**
R3(config)#**int g2/0**
R3(config-interface)#**description B2B to R2**
R3(config-interface)#**ip add 10.1.2.2 255.255.255.252**
R3(config-interface)#**no sh**
R3(config-interface)#**exit**
R3(config)#**int g1/0**
R3(config-interface)#**description B2B to R4**
R3(config-interface)#**ip add 10.1.3.1 255.255.255.252**
R3(config-interface)#**no sh**
R3(config-interface)#**exit**
R3(config)#**int loopback 3**
R3(config-interface)#**description loopback BGP-side**
R3(config-interface)#**ip add 10.1.3.4 255.255.255.255**
R3(config-interface)#**no sh**
R3(config-interface)#**exit**
R3(config)#**int g0/0**
R3(config-interface)#**ip add 10.1.5.2 255.255.255.252**
R3(config-interface)#**ip ospf authentication-key G00Dluck**
R3(config-interface)#**ip ospf authentication**
R3(config-interface)#**no sh**
R3(config-interface)#**exit**
R3(config)#**int loopback 6**
R3(config-interface)#**description loopback OSPF-side**
R3(config-interface)#**ip add 10.1.5.5 255.255.255.255**
R3(config-interface)#**no sh**
R3(config-interface)#**exit**
R3(config)#**route-map SET_LOCAL_PREF_FOR_R4 permit 10**
R3(config-route-map)#**set local-preference 200**
R3(config-route-map)#**exit**
R3(config)#**route-map SET_LOCAL_PREF_FOR_R2 permit 10**
R3(config-route-map)#**set local-preference 100**
R3(config-route-map)#**exit**
R3(config)#**router bgp 3**
R3(config-router)#**neighbor 10.1.2.1 remote-as 2**
R3(config-router)#**neighbor 10.1.2.1 description eBGP to R2**
R3(config-router)#**neighbor 10.1.2.1 password G00Dluck**
R3(config-router)#**network 10.1.2.0 mask 255.255.255.252**
R3(config-router)#**neighbor 10.1.2.1 route-map SET_LOCAL_PREF_FOR_R2 in**
R3(config-router)#**neighbor 10.1.3.2 remote-as 4**
R3(config-router)#**neighbor 10.1.3.2 description eBGP to R4**
R3(config-router)#**neighbor 10.1.3.2 password G00Dluck**
R3(config-router)#**network 10.1.3.0 mask 255.255.255.252**
R3(config-router)#**neighbor 10.1.3.2 route-map SET_LOCAL_PREF_FOR_R4 in**
R3(config-router)#**network 10.1.3.4 mask 255.255.255.255**
R3(config-router)#**bgp router-id 10.1.3.4**
R3(config-router)#**redistribute ospf 1**
R3(config-router)#**exit**
R3(config)#**router ospf 1**
R3(config-router)#**network 10.1.5.0 255.255.255.252 area 0**
R3(config-router)#**network 10.1.5.5 255.255.255.255 area 0**
R3(config-router)#**router-id 3.3.3.3**
R3(config-router)#**redistribute bgp 3 subnets metric 1000**
R3(config-router)#**end**
R3#**wr**
R3#
```

#### Configuration for Router R4:
```
R4#
R4#**conf t**
R4(config)#**hostname R4**
R4(config)#**int g1/0**
R4(config-interface)#**description B2B to R3**
R4(config-interface)#**ip add 10.1.3.2 255.255.255.252**
R4(config-interface)#**no sh**
R4(config-interface)#**exit**
R4(config)#**int g2/0**
R4(config-interface)#**description B2B to R4**
R4(config-interface)#**ip add 10.1.4.1 255.255.255.252**
R4(config-interface)#**no sh**
R4(config-interface)#**exit**
R4(config)#**int loopback 4**
R4(config-interface)#**description loopback**
R4(config-interface)#**ip add 10.1.4.4 255.255.255.255**
R4(config-interface)#**no sh**
R4(config-interface)#**exit**
R4(config)#**route-map SET_LOCAL_PREF_FOR_R1 permit 10**
R4(config-route-map)#**set local-preference 200**
R4(config-route-map)#**exit**
R4(config)#**route-map SET_LOCAL_PREF_FOR_R3 permit 10**
R4(config-route-map)#**set local-preference 100**
R4(config-route-map)#**exit**
R4(config)#**router bgp 4**
R4(config-router)#**neighbor 10.1.3.1 remote-as 3**
R4(config-router)#**neighbor 10.1.3.1 description eBGP to R3**
R4(config-router)#**neighbor 10.1.3.1 password G00Dluck**
R4(config-router)#**network 10.1.3.0 mask 255.255.255.252**
R4(config-router)#**neighbor 10.1.3.1 route-map SET_LOCAL_PREF_FOR_R3 in**
R4(config-router)#**neighbor 10.1.4.2 remote-as 1**
R4(config-router)#**neighbor 10.1.4.2 description eBGP to R1**
R4(config-router)#**neighbor 10.1.4.2 password G00Dluck**
R4(config-router)#**network 10.1.4.0 mask 255.255.255.252**
R4(config-router)#**neighbor 10.1.4.2 route-map SET_LOCAL_PREF_FOR_R1 in**
R4(config-router)#**network 10.1.4.4 mask 255.255.255.255**
R4(config-router)#**bgp router-id 10.1.4.4**
R4(config-router)#**end**
R4#**wr**
R4#
```

#### Configuration for Router R5:
```
R5#
R5#**conf t**
R5(config)#**hostname R5**
R5(config)#**int g0/0**
R5(config-interface)#**description B2B to R3**
R5(config-interface)#**ip add 10.1.5.1 255.255.255.252**
R5(config-interface)#**ip ospf authentication-key G00Dluck**
R5(config-interface)#**ip ospf authentication**
R5(config-interface)#**no sh**
R5(config-interface)#**exit**
R5(config)#**int loopback 5**
R5(config-interface)#**description loopback**
R5(config-interface)#**ip add 10.1.5.4 255.255.255.255**
R5(config-interface)#**no sh**
R5(config-interface)#**exit**
R5(config)#**router ospf 1**
R5(config-router)#**network 10.1.5.0 255.255.255.252 area 0**
R5(config-router)#**network 10.1.5.4 255.255.255.255 area 0**
R5(config-router)#**router-id 5.5.5.5**
R5(config-router)#**end**
R5#**wr**
R5#
```

### Testing and Verification

Once all configurations are completed, the following tests should be conducted to ensure full connectivity between all routers and loopback interfaces:

- **Ping Test**: Test connectivity to and from loopback interfaces.
    
- **Traceroute**: Ensure proper routing paths between routers.
    
- **Routing Table Check**: Verify that the routing table includes routes learned from both OSPF and eBGP.

```plaintext
ping [LOOPBACK_IP]
ping [LOOPBACK_IP] source [LOOPBACK_IP]
traceroute [DESTINATION_IP]
show ip route ospf
show ip route bgp
```

## Conclusion

This documentation provides the necessary configurations to implement a network with full intercommunication between routers and loopback interfaces using OSPF and eBGP. The setup also ensures redundancy using local preference and proper route redistribution between the two protocols. After applying the configurations, the network should be fully operational, and the routers will be able to communicate with each other and their respective loopback interfaces.
## Credits
- [eBGP Configuration Guide](https://howtorouteswitch.com/bgp-configuration/#bgp-neighbor-description)
- [BGP AS Number Guide](https://networklessons.com/bgp/bgp-private-and-public-as-range)
- [BGP BFD](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_bgp/configuration/xe-16/irg-xe-16-book/bgp-support-for-bfd.html)
- [BGP Redundancy](https://netseccloud.com/how-to-configure-bgp-local-preference-a-step-by-step-guide)
- [OSPF Configuration Guide](https://study-ccna.com/ospf-configuration/)
- [OSPF Loopback Interface](https://study-ccna.com/loopback-interface-loopback-address/)
- [OSPF Authentication](https://study-ccnp.com/ospf-authentication/)
