# Small-Office-Network-Source-Command

The following commands use to configure all the cisco devices for the [Small Office Network Deployment](https://github.com/AllanMontalvo/Small-Office-Network) project is listed below.

<h1>Cisco Command Configuration</h1>


**Part 1: Initial Setup**

- Configure the device's name on each device.
  <br/> **hostname (name)**

- Enable secret for R1, ASW-A1, ASW-A2, ASW-A3, ASW-B1, ASW-B2, and ASW-B3. 
 <br/> **enable secret jeremysitlab**

- Enable secret with type 9 hashing for CSW1, CSW2, DSW-A1, DSW-A2, DSW-B1, and DSW-B2 switch.
 <br/> **enable algorithm scrypt secret jeremysitlab**

- Create a local user account and secret password for R1, ASW-A1, ASW-A2, ASW-A3, ASW-B1, ASW-B2, and ASW-B3.
 <br/> **username cisco secret ccna**

- Create a local user account and secret password with type 9 hashing for CSW1, CSW2, DSW-A1, DSW-A2, DSW-B1, and DSW-B2.
 <br/> **username cisco algorithm scrypt secret ccna**

- Configure the console line to require login with a local account. Set a 30-min inactivity timeout and enable synchronous
    logging.
<br/> **line console 0
<br/> login local
<br/> exec-timeout 30
<br/> logging synchronous**

- Verify configuration
<br/> **show run**

**Part 2: Vlans, Layer-2 EtherChannel**

- Configure interface G1/0/4 and G1/0/5 as a Layer-2 EtherChannel between DSW-A1 and DSW-A2 using PAnP. 
<br/> **inter range g1/0/4-5
<br/> channel-group 1 mode desirable**
- Verify Etherchannel connection.
<br/> **show etherchannel summary**


- Configure interface G1/0/4 and G1/0/5 as a Layer-2 EtherChannel between DSW-B1 and DSW-B2 using LACP.
<br/> **inter range g1/0/4-5
<br/> channel-group 1 mode active**
- Verify Etherchannel connection.
<br/> **show etherchannel summary**


- Configure Port-Channel 1 as a trunk link with the allow VLANs, change native VLAN, and disable DTP on DSW-A1 and DSW-A2. 
<br/> **inter port-channel 1
<br/> switch mode trunk
<br/> switch trunk native vlan 1000
<br/> switch trunk allow vlan 10,20,40,99
<br/> switch nonegotiate**
- Verify configuration of Port- Channel 1.
<br/> **show run**

- Single port configuration connects to access switches as trunk links  with the allow VLANS, change native VLAN, and
    disable DTP on DSW-A1 and DSW-A2.
<br/> **inter range g1/0/1-3
<br/> switch mode trunk
<br/> switch trunk native vlan 1000
<br/> switch trunk allow vlan 10,20,40,99
<br/> switch nonegotiate**
- Verify configuration of interfaces.
<br/> **show inter (interface port) switchport**

- Configure Port-Channel 1 as a trunk link with the allow VLANs, change native VLAN, and disable DTP on DSW-B1 and DSW-B2.
<br/> **inter port-channel 1
<br/> switch mode trunk
<br/> switch trunk native vlan 1000
<br/> switch trunk allow vlan 10,20,30,99
<br/> switch nonegotiate** 
- Verify configuration of interfaces.
<br/> **show run**


- Single port configuration connects to access switches as trunk links  with the allow VLANS, change native VLAN, and
    disable DTP on DSW-B1 and DSW-B2.
<br/> **inter range g1/0/1-3
<br/> switch mode trunk
<br/> switch trunk native vlan 1000
<br/> switch trunk allow vlan 10,20,30,99
<br/> switch nonegotiate**
- Verify configuration of interfaces.
<br/> **show inter (interface port) switchport**


- Configure one Distribution switch as VTPv2 server and use domain name JeremysITLab for each office.
<br/> **vtp version 2
<br/> vtp mode server
<br/> vtp domain JeremysITLab**
- Configure Access switches as vtp client for each office
<br/> **vtp mode client**
- Verify status of vtp.
<br/> **show vtp status**


- Create VLANs of the following below on DSW-A1.
<br/> **vlan 10
<br/> name PCs
<br/> vlan 20
<br/> name Phones
<br/> vlan 40
<br/> name Wi-Fi
<br/> vlan 99
<br/> name Management**
- Verify creation of vlan.
<br/> **show vlan**

- Create VLANs of the following below on DSW-B1.
<br/> **vlan 10
<br/> name PCs
<br/> vlan 20
<br/> name Phones
<br/> vlan 30
<br/> name Servers
<br/> vlan 99
<br/> name Management**
- Verify creation of vlan.
<br/> **show vlan**


- Since LWAP will not use FlexConnect configure for ASW-A1 and ASW-B1
<br/> **inter f0/1
<br/> switch mode access
<br/> switch access vlan 99
<br/> switch nonegotiate**
- Verify configuration of interface f0/1
<br/> **show inter f0/1 switchport**

 
- Configuration for PCs and phone connections on ASW-A2, ASW-A3, and ASW-B2.
<br/> **inter f0/1
<br/> switch mode access
<br/> switch access vlan 10
<br/> switch voice vlan 20
<br/> switch nonegotiate**
- Verify configuration of interface.
<br/> **show inter f0/1 switchport**

- Configuration for server connection on ASW-B3.
<br/> **inter f0/1
<br/> switch mode access
<br/> switch access vlan 30
<br/> switch nonegotiate**
- Verify configuration of interface.
<br/> **show inter f0/1 switchport**

- Configuration for ASW-A1 for WLC1.
<br/> **inter f0/2
<br/> switch mode trunk
<br/> switch trunk allow vlan 40,99
<br/> switch trunk native vlan 99
<br/> switch nonegotiate**
- Verify configuration of interface.
<br/> **show inter f0/2 switchport**


- Use to find what ports are not in use and verify shutdown.
<br/> **show ip inter brief**
-  Disable interface ports on ASW-A1.
<br/> **inter range f0/3-24
<br/> shutdown**
- Disable interface ports on ASW-A2, ASW-A3, ASW-B1, ASW-B2, and ASW-B3.
<br/> **inter range f0/2-24
<br/> shutdown**

- Disable interface ports on DSW-A1, DSW-A2, DSW-B1, and DSW-B2.
<br/> **inter range g1/0/6-24
<br/> shutdown
<br/> inter range g1/1/3-4
<br/> shutdown**


**Part 3 IP Address, Layer-3 EtherChannel**

- Configure DHCP client address on R1 connected to the internet.
<br/> **inter g0/0/0
<br/> ip address dhcp
<br/> no shutdown
<br/> inter g0/1/0
<br/> ip address dhcp
<br/> no shutdown**

- Assign ip address on R1 with the following connected to Core switches.
<br/> **inter g0/0
<br/> ip address 10.0.0.33 255.255.255.252
<br/> no shutdown
<br/> inter g0/1
<br/> ip address 10.0.0.37 255.255.255.252
<br/> no shutdown**

- Assign loopback0 address on R1 with the following.
<br/> **inter loopback0
<br/> ip address 10.0.0.76 255.255.255.255
<br/> no shutdown**
 
- Enabled routing on CSW1, CSW2, DSW-A1, DSW-A2, DSW-B1, and DSW-B2.
<br/> **ip routing**

- Configure a Layer-3 EtherChannel on CSW1 as a PAnP
<br/> **inter range 1/0/2-3
<br/> channel-group 1 mode desirable
<br/> exit
<br/> inter port-channel 1
<br/> no switch
<br/> ip address 10.0.0.41 255.255.255.252
<br/> exit**
- Configure a Layer-3 EtherChannel on CSW2 as a PAnP
<br/> **inter range 1/0/2-3
<br/> channel-group 1 mode desirable
<br/> exit
<br/> inter port-channel 1
<br/> no switch
<br/> ip address 10.0.0.42 255.255.255.252
<br/> exit**


- Assign IP address to interface and shutdown unused interface on CSW1
<br/> **inter g1/0/1
<br/> no switch
<br/> ip address 10.0.0.34 255.255.255.252
<br/> no shutdown
<br/> inter g1/1/1
<br/> no switch
<br/> ip address 10.0.0.45 255.255.255.252
<br/> no shutdown
<br/> inter g1/1/2
<br/> no switch
<br/> ip address 10.0.0.49 255.255.255.252
<br/> no shutdown
<br/> inter g1/1/3
<br/> no switch
<br/> ip address 10.0.0.53 255.255.255.252
<br/> no shutdown
<br/> inter g1/1/4
<br/> no switch
<br/> ip address 10.0.0.57 255.255.255.252
<br/> no shutdown
<br/> inter loopback0
<br/> ip address 10.0.0.77 255.255.255.255
<br/> no shutdown
<br/> exit
<br/> inter range g1/0/4-24
<br/> shutdown**


- Assign IP address to interface and shutdown unused interface on CSW1
<br/> **inter g1/0/1
<br/> no switch
<br/> ip address 10.0.0.38 255.255.255.252
<br/> no shutdown
<br/> inter g1/1/1
<br/> no switch
<br/> ip address 10.0.0.61 255.255.255.252
<br/> no shutdown
<br/> inter g1/1/2
<br/> no switch
<br/> ip address 10.0.0.65 255.255.255.252
<br/> no shutdown
<br/> inter g1/1/3
<br/> no switch
<br/> ip address 10.0.0.69 255.255.255.252
<br/> no shutdown
<br/> inter g1/1/4
<br/> no switch
<br/> ip address 10.0.0.73 255.255.255.252
<br/> no shutdown
<br/> inter loopback0
<br/> ip address 10.0.0.78 255.255.255.255
<br/> no shutdown
<br/> exit
<br/> inter range g1/0/4-24
<br/> shutdown**


- Assign IP address to interface on DSW-A1
<br/> **inter g1/1/1
<br/> no switch
<br/> ip address 10.0.0.46 255.255.255.252
<br/> no shutdown
<br/> inter g1/1/2
<br/> no switch
<br/> ip address 10.0.0.62 255.255.255.252
<br/> no shutdown
<br/> inter loopback0
<br/> ip address 10.0.0.79 255.255.255.255**


- Assign IP address to interface on DSW-A2
<br/> **inter g1/1/1
<br/> no switch
<br/> ip address 10.0.0.50 255.255.255.252
<br/> no shutdown
<br/> inter g1/1/2
<br/> no switch
<br/> ip address 10.0.0.66 255.255.255.252
<br/> no shutdown
<br/> inter loopback0
<br/> ip address 10.0.0.80 255.255.255.255**


- Assign IP address to interface on DSW-B1
<br/> **inter g1/1/1
<br/> no switch
<br/> ip address 10.0.0.54 255.255.255.252
<br/> no shutdown
<br/> inter g1/1/2
<br/> no switch
<br/> ip address 10.0.0.70 255.255.255.252
<br/> no shutdown
<br/> inter loopback0
<br/> ip address 10.0.0.81 255.255.255.255**


- Assign IP address to interface on DSW-B2
<br/> **inter g1/1/1
<br/> no switch
<br/> ip address 10.0.0.58 255.255.255.252
<br/> no shutdown
<br/> inter g1/1/2
<br/> no switch
<br/> ip address 10.0.0.74 255.255.255.252
<br/> no shutdown
<br/> inter loopback0
<br/> ip address 10.0.0.82 255.255.255.255**


- Opening Server 1 and set the following ip address
<br/> **Default gateway: 10.5.0.1
<br/> IPv4 address: 10.5.0.4
<br/> Subnet mask: 255.255.255.0**


- Configuration of VLAN 99 on each Access switches
  - ASW-A1
<br/> **ip default-gateway 10.0.0.1
<br/> inter vlan 99
<br/> ip address 10.0.0.4 255.255.255.240
<br/> no shutdown**
  - ASW-A2
<br/> **ip default-gateway 10.0.0.1
<br/> inter vlan 99
<br/> ip address 10.0.0.5 255.255.255.240
<br/> no shutdown**
  - ASW-A3
<br/> **ip default-gateway 10.0.0.1
<br/> inter vlan 99
<br/> ip address 10.0.0.6 255.255.255.240
<br/> no shutdown**
  - ASW-B1
<br/> **ip default-gateway 10.0.0.17
<br/> inter vlan 99
<br/> ip address 10.0.0.20 255.255.255.240
<br/> no shutdown**
  - ASW-B2
<br/> **ip default-gateway 10.0.0.17
<br/> inter vlan 99
<br/> ip address 10.0.0.21 255.255.255.240
<br/> no shutdown**
  - ASW-B3
<br/> **ip default-gateway 10.0.0.17
<br/> inter vlan 99
<br/> ip address 10.0.0.22 255.255.255.240
<br/> no shutdown**

- Verify IP Address configuration for all router and switches.
<br/> **show ip inter brief**

**Part 4 HSRP**

- Configure HSRPv2 for VLAN 99 on DSW-A1.
<br/> **int vlan 99
<br/> ip address 10.0.0.2 255.255.255.240
<br/> standby ver 2
<br/> standby 1 ip 10.0.0.1
<br/> standby 1 priority 105
<br/> standby 1 preempt**
-  configuration of HSRPv2 for VLAN 99 on DSW-A2
<br/> **inter vlan 99
<br/> ip address 10.0.0.3 255.255.255.240
<br/> standby ver 2
<br/> standby 1 ip 10.0.0.1**

- Verify HSRP.
<br/> **show standby**


- Configure HSRPv2 for VLAN 10 on DSW-A1.
<br/> **inter vlan 10
<br/> ip address 10.1.0.2 255.255.255.0
<br/> standby ver 2
<br/> standby 2 ip 10.1.0.1
<br/> standby 2 priority 105
<br/> standby 2 preempt**
- Configure HSRPv2 for VLAN 10 on DSW-A2
<br/> **inter vlan 10
<br/> ip address 10.1.0.3 255.255.255.0
<br/> standby ver 2
<br/> standby 2 ip 10.1.0.1**

- Verify HSRP 
<br/> **show standby**


- Configure HSRPv2 for VLAN 20 on DSW-A2.
<br/> **inter vlan 20
<br/> ip address 10.2.0.3 255.255.255.0
<br/> standby ver 2
<br/> standby 3 ip 10.2.0.1
<br/> standby 3 priority 105
<br/> standby 3 preempt**
- Configure HSRPv2 for VLAN 20 on DSW-A1.
<br/> **inter vlan 20
<br/> ip address 10.2.0.2 255.255.255.0
<br/> standby ver 2
<br/> standby 3 ip 10.2.0.1**

- Verify HSRP 
<br/> **show standby**


- Configure HSRPv2 for VLAN 40 on DSW-A2.
<br/> **inter vlan 40
<br/> ip address 10.6.0.3 255.255.255.0
<br/> standby ver 2
<br/> standby 4 ip 10.6.0.1
<br/> standby 4 priority 105
<br/> standby 4 preempt**
- Configure HSRPv2 for VLAN 40 on DSW-A1.
<br/> **inter vlan 40
<br/> ip address 10.6.0.2 255.255.255.0
<br/> standby ver 2
<br/> standby 4 ip 10.6.0.1**

- Verify HSRP 
<br/> **show standby**


- Configure HSRPv2 for VLAN 99 on DSW-B1.
<br/> **inter vlan 99
<br/> ip address 10.0.0.18 255.255.255.240
<br/> standby ver 2
<br/> standby 1 ip 10.0.0.17
<br/> standby 1 priority 105
<br/> standby 1 preempt**
- Configure HSRPv2 for VLAN 99 on DSW-B2.
<br/> **inter vlan 99
<br/> ip address 10.0.0.19 255.255.255.240
<br/> standby ver 2
<br/> standby 1 ip 10.0.0.17**

- Verify HSRP 
<br/> **show standby**


- Configure HSRPv2 for VLAN 10 on DSW-B1.
<br/> **inter vlan 10
<br/> ip address 10.3.0.2 255.255.255.0
<br/> standby ver 2
<br/> standby 2 ip 10.3.0.1
<br/> standby 2 priority 105
<br/> standby 2 preempt**
- Configure HSRPv2 for VLAN 10 on DSW-B2.
<br/> **inter vlan 10
<br/> ip address 10.3.0.3 255.255.255.0
<br/> standby ver 2
<br/> standby 2 ip 10.3.0.1**

- Verify HSRP 
<br/> **show standby**



- Configure HSRPv2 for VLAN 20 on DSW-B2.
<br/> **inter vlan 20
<br/> ip address 10.4.0.3 255.255.255.0
<br/> standby ver 2
<br/> standby 3 ip 10.4.0.1
<br/> standby 3 priority 105
<br/> standby 3 preempt**
- Configure HSRPv2 for VLAN 20 on DSW-B1
<br/> **inter vlan 20
<br/> ip address 10.4.0.2 255.255.255.0
<br/> standby ver 2
<br/> standby 3 ip 10.4.0.1**

- Verify HSRP 
<br/> **show standby**


- Configure HSRPv2 for VLAN 30 on DSW-B2
<br/> **inter vlan 30
<br/> ip address 10.5.0.3 255.255.255.0
<br/> standby ver 2
<br/> standby 4 ip 10.5.0.1
<br/> standby 4 priority 105
<br/> standby 4 preempt**
- Configure HSRPv2 for VLAN 30 on DSW-B1
<br/> **inter vlan 30
<br/> ip address 10.5.0.2 255.255.255.0
<br/> standby ver 2
<br/> standby 4 ip 10.5.0.1**

- Verify HSRP 
<br/> **show standby**


**Part 5 Rapid Spanning Tree Protocol**

<br/> Configuration of Rapid PVST+ on all access and distribution switches.

- Enabled Rapid PVST+ for all access and distribution swtiches.
<br/> **spanning-tree mode rapid-pvst**
- Configure priority of Rapid PVST+ for each VLAN on DSW-A1. 
<br/> **spanning-tree vlan 10,99 priority 0
<br/> spanning-tree vlan 20,40 priority 4096**
- Configure priority of Rapid PVST+ for each VLAN on DSW-A2. 
<br/> **spanning-tree vlan 20,40 priority 0
<br/> spanning-tree vlan 10,99 priority 4096**

- Configure priority of Rapid PVST+ for each VLAN on DSW-B1. 
<br/> **spanning-tree vlan 10,99 priority 0
<br/> spanning-tree vlan 20,30 priority 4096**
- Configure priority of Rapid PVST+ for each VLAN on DSW-B2. 
<br/> **spanning-tree vlan 20,30 priority 0
<br/> spanning-tree vlan 10,99 priority 4096**

- Verify spanning-tree configuration of vlan.
<br/>  **show spanning-tree vlan (number)**


<br/> Enabled PortFast and BPDUGuard on necessary interfaces
- Configuration for accss port interface on all Access switches.
<br/> **inter f0/1
<br/> spanning-tree portfast
<br/> spanning-tree bpduguard enable**

- configuration for trunk port interface on ASW-A1
<br/> **inter f0/2
<br/> spanning-tree portfast trunk
<br/> spanning-tree bpduguard enable**


**Part 6 Static and Dynamic Routing**

<br/>  Configuration of OSPF on R1, CSW1, CSW2, DSW-A1, DSW-A2, DSW-B1, and DSW-B2

- Configuration for R1
<br/> **router ospf 1
<br/> router-id 10.0.0.76
<br/> passive-interface loopback0
<br/> inter loopback0
<br/> ip ospf 1 area 0
<br/> inter range g0/0-1
<br/> ip ospf 1 area 0
<br/> ip ospf network point-to-point**

- Verify ospf neighbors connections 
<br/> **show ip ospf neighbor**

- Configuration for CSW1
<br/> **router ospf 1
<br/> router-id 10.0.0.77
<br/> passive-interface loopback0
<br/> network 10.0.0.41 0.0.0.0 area 0
<br/> network 10.0.0.34 0.0.0.0 area 0
<br/> network 10.0.0.45 0.0.0.0 area 0
<br/> network 10.0.0.49 0.0.0.0 area 0
<br/> network 10.0.0.53 0.0.0.0 area 0
<br/> network 10.0.0.57 0.0.0.0 area 0
<br/> network 10.0.0.77 0.0.0.0 area 0
<br/> exit
<br/> inter range g1/0/1,g1/1/1-4
<br/> ip ospf network point-to-point
<br/> exit**

- Verify ospf neighbors connections.
<br/> **show ip ospf neighbor**


- Configuration for CSW2
<br/> **router ospf 1
<br/> router-id 10.0.0.78
<br/> passive-interface loopback0
<br/> network 10.0.0.42 0.0.0.0 area 0
<br/> network 10.0.0.38 0.0.0.0 area 0
<br/> network 10.0.0.61 0.0.0.0 area 0
<br/> network 10.0.0.65 0.0.0.0 area 0
<br/> network 10.0.0.69 0.0.0.0 area 0
<br/> network 10.0.0.73 0.0.0.0 area 0
<br/> network 10.0.0.78 0.0.0.0 area 0
<br/> exit
<br/> inter range g1/0/1,g1/1/1-4
<br/> ip ospf network point-to-point
<br/> exit**

- Verify ospf neighbors connections 
<br/> **show ip ospf neighbor**

- Configuration for DSW-A1
<br/> **router ospf 1
<br/> router-id 10.0.0.79
<br/> passive-interface loopback0
<br/> passive-interface vlan 10
<br/> passive-interface vlan 20
<br/> passive-interface vlan 40
<br/> network 10.0.0.46 0.0.0.0 area 0
<br/> network 10.0.0.62 0.0.0.0 area 0
<br/> network 10.0.0.79 0.0.0.0 area 0
<br/> network 10.1.0.2 0.0.0.0 area 0
<br/> network 10.2.0.2 0.0.0.0 area 0
<br/> network 10.0.0.2 0.0.0.0 area 0
<br/> network 10.6.0.2 0.0.0.0 area 0
<br/> exit
<br/> inter range g1/1/1-2
<br/> ip ospf network point-to-point
<br/> exit**

- Verify ospf neighbors connections 
<br/> **show ip ospf neighbor**

- Configuration for DSW-A2
<br/> **router ospf 1
<br/> router-id 10.0.0.80
<br/> passive-interface loopback0
<br/> passive-interface vlan 10
<br/> passive-interface vlan 20
<br/> passive-interface vlan 40
<br/> network 10.0.0.50 0.0.0.0 area 0
<br/> network 10.0.0.66 0.0.0.0 area 0
<br/> network 10.0.0.80 0.0.0.0 area 0
<br/> network 10.1.0.3 0.0.0.0 area 0
<br/> network 10.2.0.3 0.0.0.0 area 0
<br/> network 10.0.0.3 0.0.0.0 area 0
<br/> network 10.6.0.3 0.0.0.0 area 0
<br/> exit
<br/> inter range g1/1/1-2
<br/> ip ospf network point-to-point
<br/> exit**

- Verify ospf neighbors connections .
<br/> **show ip ospf neighbor**

- Configuration for DSW-B1.
<br/> **router ospf 1
<br/> router-id 10.0.0.81
<br/> passive-interface loopback0
<br/> passive-interface vlan 10
<br/> passive-interface vlan 20
<br/> passive-interface vlan 30
<br/> network 10.0.0.54 0.0.0.0 area 0
<br/> network 10.0.0.70 0.0.0.0 area 0
<br/> network 10.0.0.81 0.0.0.0 area 0
<br/> network 10.3.0.2 0.0.0.0 area 0
<br/> network 10.4.0.2 0.0.0.0 area 0
<br/> network 10.5.0.2 0.0.0.0 area 0
<br/> network 10.0.0.18 0.0.0.0 area 0
<br/> exit
<br/> inter range g1/1/1-2
<br/> ip ospf network point-to-point
<br/> exit**

- Verify ospf neighbors connections. 
<br/> **show ip ospf neighbor**

- Configuration for DSW-B2.
<br/> **router ospf 1
<br/> router-id 10.0.0.82
<br/> passive-interface loopback0
<br/> passive-interface vlan 10
<br/> passive-interface vlan 20
<br/> passive-interface vlan 30
<br/> network 10.0.0.58 0.0.0.0 area 0
<br/> network 10.0.0.74 0.0.0.0 area 0
<br/> network 10.0.0.82 0.0.0.0 area 0
<br/> network 10.3.0.3 0.0.0.0 area 0
<br/> network 10.4.0.3 0.0.0.0 area 0
<br/> network 10.5.0.3 0.0.0.0 area 0
<br/> network 10.0.0.19 0.0.0.0 area 0
<br/> exit
<br/> inter range g1/1/1-2
<br/> ip ospf network point-to-point
<br/> exit**

- Verify ospf neighbors connections. 
<br/> **show ip ospf neighbor**


- Configure a static route on R1 to the internet.
<br/> **ip route 0.0.0.0 0.0.0.0 203.0.113.1**
- Add AD higher than the other static route.
<br/> **ip route 0.0.0.0 0.0.0.0 203.0.113.5 2**

- Verify connection to internet and their ip address for next hop 
<br/> **show inter g0/0/0 or g0/1/0**

- Advertise R1's default route as an OSPF ASBR to other routers in the OSPF Domain.
<br/> **router ospf 1
<br/> default-information originate**


**Part 7 DHCP**

<br/> Configure DHCP pools on R1 to make it serve as the DHCP server for hosts in Office A and B
- Configuration of R1 to exclude the first 10 useable ip addresses for each pool.
<br/> **ip dhcp excluded-address 10.0.0.1 10.0.0.10
<br/> ip dhcp excluded-address 10.0.0.17 10.0.0.26
<br/> ip dhcp excluded-address 10.1.0.1 10.1.0.10
<br/> ip dhcp excluded-address 10.2.0.1 10.2.0.10
<br/> ip dhcp excluded-address 10.3.0.1 10.3.0.10
<br/> ip dhcp excluded-address 10.4.0.1 10.4.0.10
<br/> ip dhcp excluded-address 10.6.0.1 10.6.0.10**

- Configure DHCP pool for VLAN 99 Management in Office A on R1.
<br/> **ip dhcp pool A-Mgmt
<br/>  network 10.0.0.0 255.255.255.240
<br/> default-router 10.0.0.1
<br/> domain-name jeremysitlab.com 
<br/> dns-server 10.5.0.4
<br/> option 43 ip 10.0.0.7**

- Verify creation of dhcp pool
<br/> **show ip dhcp pool**

- Configure DHCP pool for VLAN 10 PCs in Office A on R1.
<br/> **ip dhcp pool A-PC
<br/> network 10.1.0.0 255.255.255.0
<br/> default-router 10.1.0.1
<br/> domain-name jeremysitlab.com
<br/> dns-server 10.5.0.4**

- verify creation of dhcp pool
<br/> **show ip dhcp pool**

- Configure DHCP pool for VLAN 20 Phone in Office A on R1.
<br/> **ip dhcp pool A-Phone
<br/> network 10.2.0.0 255.255.255.0
<br/> default-router 10.2.0.1
<br/> domain-name jeremysitlab.com
<br/> dns-server 10.5.0.4**

- Verify creation of dhcp pool.
<br/> **show ip dhcp pool**


- Configure DHCP pool for VLAN 99 Management in Office B on R1.
<br/> **ip dhcp pool B-Mgmt
<br/> network 10.0.0.16 255.255.255.240
<br/> default-router 10.0.0.17
<br/> domain-name jeremysitlab.com
<br/> dns-server 10.5.0.4
<br/> option 43 ip 10.0.0.7**

- verify creation of dhcp pool
<br/> **show ip dhcp pool**


- Configure DHCP pool for VLAN 10 PC in Office B on R1.
<br/> **ip dhcp pool B-PC 
<br/> network 10.3.0.0 255.255.255.0
<br/> default-router 10.3.0.1
<br/> domain-name jeremysitlab.com
<br/> dns-server 10.5.0.4**

- verify creation of dhcp pool
<br/> **show ip dhcp pool**


- Configure DHCP pool for VLAN 20 Phone in Office B on R1.
<br/> **ip dhcp pool B-Phone
<br/> network 10.4.0.0 255.255.255.0
<br/> default-router 10.4.0.1
<br/> domain-name jeremysitlab.com
<br/> dns-server 10.5.0.4**

- verify creation of dhcp pool
<br/> **show ip dhcp pool**


- Configure DHCP pool for VLAN 40 Wi-Fi in Office A on R1.
<br/> **ip dhcp pool Wi-Fi
<br/> network 10.6.0.0 255.255.255.0
<br/> default-router 10.6.0.1
<br/> domain-name jeremysitlab.com
<br/> dns-server 10.5.0.4**

- Verify creation of dhcp pool.
<br/> **show ip dhcp pool**



- Configure DSW-A1 and DSW-A2 to relay wired DHCP clients' broadcast message to R1's Loopback0 IP address.
<br/> **inter vlan 10
<br/> ip helper-address 10.0.0.76
<br/> inter vlan 20
<br/> ip helper-address 10.0.0.76
<br/> inter vlan 40
<br/> ip helper-address 10.0.0.76
<br/> inter vlan 99
<br/> ip helper-address 10.0.0.76**

- Configure DSW-B1 and DSW-B2 to relay wired DHCP clients' broadcast message to R1's Loopback0 IP address.
<br/> **inter vlan 10
<br/> ip helper-address 10.0.0.76
<br/> inter vlan 20
<br/> ip helper-address 10.0.0.76
<br/> inter vlan 30
<br/> ip helper-address 10.0.0.76
<br/> inter vlan 99
<br/> ip helper-address 10.0.0.76**

- Verify if PC1 can reach SVR1 
<br/> **cmd prompt: ping 10.5.0.4**

**Part 8 DNS**

- Configure all routers and switches to use domain name jeremysitlab.com and use Server 1 as thier DNS server.
<br/> **ip domain name jeremysitlab.com
<br/> ip name-server 10.5.0.4**

**Part 9 NTP**

- Configure R1 as a Stratum 5 NTP server and learn time from NTP server 216.239.35.0.
<br/> **ntp master 5
<br/> ntp server 216.239.35.0**

- Configuration to generate authentication key and password for ntp on R1.
<br/> **ntp authentication-key 1 md5 ccna
<br/> ntp trusted-key 1**
- Configure all Core, Distribution, and Access switches sync with R1 as their NTP server.
<br/> **ntp authentication-key 1 md5 ccna
<br/> ntp trusted-key 1
<br/> ntp server 10.0.0.76 key 1**

**Part 10 SNMP**

- Configuration for R1 of SNMP community string.
<br/> **snmp-server community SNMPSTRING ro** 

**Part 11 Syslog**

- Configure Syslog on all routers and switches by sending all Syslog message to
	to Server 1(10.5.0.4) and enable logging to the buffer with reserve of 8192 bytes
	of memory.
<br/> **logging 10.5.0.4
<br/> logging trap debugging
<br/> logging buffer 8192**

- Verify syslog configuration.
<br/> **show logging**

**Part 12 FTP**

Configuration on R1 to use FTP to download new IOS version from Server 1.

- Configure R1's default FTP credentials: username cisco, password cisco.
<br/> **ip ftp username cisco
<br/> ip ftp password cisco**

- Copy file c2900-universalk9-mz.SPA.155-3.M4a.bin from Server 1 (10.5.0.4) to 
	flash and have R1 boot to this IOS version and save configuration.
<br/> **copy ftp flash
<br/> boot system flash c2900-universalk9-mz.SPA.155-3.M4a.bin
<br/> wr** 
	
- Verify new IOS is download to flash.
<br/> **show flash**

- Reload R1 with new IOS version and delete old IOS version, c2900-universalk9-mz.SPA.151-4.M4.bin, from flash.
<br/> **reload
<br/> delete flash c2900-universalk9-mz.SPA.151-4.M4.bin**
	
- Verify IOS on router.
<br/> **show version**

**Part 13 SSH**

Configure SSH for secure remote access on all routers and switches.

- Generate a RSA key with the largest modulus size (in this case 4096).
<br/> **crypto key generate rsa**
	
- Update SSH to version 2 on all routers and switches.
<br/> **ip ssh ver 2**
- Verify ssh version.
<br/> **show ip ssh**

- Create a standard ACL 1 to allow packets sourced from Office A's PCs subnet
<br/> **access-list 1 permit 10.1.0.0 0.0.0.255**

- Allow only SSH connections to VTY lines, require local login to user account on device, and configure synchronous
    logging on VTY lines.
<br/> **line vty 0 15
<br/> access-class 1 in
<br/> transport input ssh
<br/> login local
<br/> logging synchronous**

- Verify connection by login in to router through a PC.
<br/> **cmd prompt: ssh -l cisco 10.0.0.76**

- Verify vty line.
<br/> **show run | section line vty**

**Part 14 NAT**

- Configure a static NAT on R1 to enable hosts on the internet to access Server 1.
<br/> **ip nat inside source static 10.5.0.4 203.0.113.113
<br/> inter range g0/0/0, g0/1/0
<br/> ip nat outside
<br/> inter range g0/0-1
<br/> ip nat inside**


Configure a pool-base Dynamic PAT on R1

- Defining appropriate inside local address ranges for Dynamic PAT
<br/> **access-list 2 permit 10.1.0.0 0.0.0.255
<br/> access-list 2 permit 10.2.0.0 0.0.0.255
<br/> access-list 2 permit 10.3.0.0 0.0.0.255
<br/> access-list 2 permit 10.4.0.0 0.0.0.255
<br/> access-list 2 permit 10.6.0.0 0.0.0.255**

- Defining a range of inside global addresses called POOL1 with range 203.0.113.200
	to 203.0.113.207 with /29 subnet mask.
<br/> **ip nat pool POOL1 203.0.113.200 203.0.113.207 netmask 255.255.255.248**

- Map ACL 2 to POOL1 and enable PAT on R1
<br/> **ip nat inside source list 2 pool POOL1 overload**
- verify connection by ping jeremysitlab.com from pcs
<br/> **cmd prompt: ping jeremysitlab.com**


**Part 15 LLDP**

- Disable CDP and enable LLDP on all devices. 
<br/> **no cdp run
<br/> lldp run**
- Disable LLDP Tx on each access port connected to a host.
<br/> **inter f0/1
<br/> no lldp transmit**

- Verify change to LLDP
<br/> **show lldp neighbor**


**Part 16 ACLs**

- Configure an extended ACL on DSW-A1 and DSW-A2 to allow IMCP messages but deny any
	other traffic from PCs in Office A to Office B and allow other traffic.
<br/> **ip access-list extended OfficeA_to_OfficeB
<br/> permit icmp 10.1.0.0 0.0.0.255 10.3.0.0 0.0.0.255 
<br/> deny ip 10.1.0.0 0.0.0.255 10.3.0.0 0.0.0.255
<br/> permit ip any any
<br/> inter vlan 10
<br/> ip access-group OfficeA_to_OfficeB in**

- Verify conncetion by pinging a pc in Office B 
<br/> **cmd prompt: ping (ip address PC B)**

**Part 17 Port Security**

Configure Port Security on each Access switches' f0/1 port

- Configuration for ASW-A1, ASW-B1, ASW-B3
<br/> **inter f0/1
<br/> switch port-security
<br/> switch port-security violation restrict
<br/> switch port-security mac-address sticky**

- configuration for ASW-A2, ASW-A3, ASW-B2
<br/> **inter f0/1
<br/> switch port-security
<br/> switch port-security max 2
<br/> switch port-security violation restrict
<br/> switch port-security mac-address sticky**

**Part 18 DHCP Snooping**

Configure DHCP Snooping on all Access switches
- Configuration for Office A Access switches on all active VLANs.
<br/> **ip dhcp snooping
<br/> ip dhcp snooping vlan 10,20,40,99**
- Configuration for Office B Access switches on all active VLANs.
<br/> **ip dhcp snooping
<br/> ip dhcp snooping vlan 10,20,30,99**

- Configure interfaces connected to Distribution switches as trust.
<br/> **inter range g0/1-2
<br/> ip dhcp snooping trust**

- Disable insertion of DHCP Option 82.
<br/> **no ip dhcp snooping information option**


- Set DHCP rate limit to 15 on all active untrusted ports on all Access switches.
<br/> **inter f0/1
<br/> ip dhcp snooping limit rate 15**

- Set DHCP rate limit on interface f0/2 to 100 ASW-A1.
<br/> **inter f0/2
<br/> ip dhcp snooping limit rate 100**


**Part 19 DAI**

Configure DAI on all Access switches

- Enable DAI in Office A Access switches
<br/> **ip arp inspection vlan 10,20,40,99**
- Enable DAI in Office B Access switches
<br/> **ip arp inspection vlan 10,20,30,99**

- Configure Access switches' interfaces connected to Distribution switch as trust
<br/> **inter range g0/1-2
<br/> ip arp inspection trust**

- Enable validation checks on all Access switches
<br/> **ip arp inspection validate dst-mac src-mac ip**


**Part 20 IPv6**
Configure IPv6 Address on R1, CSW1, and CSW2.

- Configure IPv6 Address on R1 on interface g0/0/0 and g0/1/0.
<br/> **ipv6 unicast-routing
<br/> inter g0/0/0
<br/> ipv6 address 2001:db8:a::2/64
<br/> inter g0/1/0 
<br/> ipv6 address 2001:db8:b::2/64**

- Configure IPv6 Address with eui-64 on R1. 
<br/> **inter g0/0
<br/> ipv6 address 2001:db8:a1::/64 eui-64**

- Configure IPv6 Address with eui-64 on CSW1.
<br/> **ipv6 unicast-routing
<br/> inter g1/0/1
<br/> ipv6 address 2001:db8:a1::/64 eui-64**

- Configure IPv6 Address with eui-64 on R1.
<br/> **inter g0/1 
<br/> ipv6 address 2001:db8:a2::/64 eui-64**
- Configure IPv6 Address with eui-64 on CSW2.
<br/> **ipv6 unicast-routing
<br/> inter g1/0/1
<br/> ipv6 address 2001:db8:a2::/64 eui-64**

- Enable IPv6 for CSW1 and CSW2 w/o using ipv6 address.
<br/> **inter port-channel 1
<br/> ipv6 enable**

- Verify ipv6 configuration
<br/> **show ipv6 inter brief**


- Create a recursive route on R1.
<br/> **ipv6 route ::/0 2001:db8:a::1**
- Creating a floating recursive route with 1 AD higher than default on R1.
<br/> **ipv6 route ::/0 g0/1/0 2001:db8:b::1 2**


**Part 21 Wireless**

- Login to WLC1 through any pcs via 10.0.0.7 using the following information below.
<br/> **a. Username: admin
<br/> b. Password: adminPW12**


- Configure a dynamic interface for Wi-Fi WLAN with the following information below.
	<br/> **a. Name: Wi-Fi
	<br/> b. VLAN: 40
	<br/> c. Port number: 1
	<br/> d. IP address: 10.6.0.4
	<br/> e. Subnet: 255.255.255.0
	<br/> e. Gateway: 10.6.0.1
	<br/> f. DHCP server: 10.0.0.76**
	

- Configure and enable the WLAN with the following information below.
<br/> 	**a. Profile name: Wi-Fi
<br/> 	b. SSID: Wi-Fi
<br/> 	c. ID: 1
<br/> 	d. Status: Enabled
<br/> 	f. select Wi-Fi
<br/> 	e. Security: WPA2 Policy with AES encryption, PSK of cisco123**
	

- check if both LWAPs have associated with WLC1
	<br/> **Open Laptop 1 and under config select Wireless0.
	<br/> Select WPA2-PSK and enter psk pass phase.
	<br/> Wait a few seconds and the laptop should be assign an IP address.
	<br/> ** Note that it may note be in the expected IP address because of issues from 
	Packet Tracer.**



