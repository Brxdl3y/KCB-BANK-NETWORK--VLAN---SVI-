KCB Bank HQ Network Lab 

Elevator pitch: A bank‑grade campus + edge design that balances security, scalability, and operational pragmatism. Built on multilayer switching (SVIs + routed uplink), OSPF with MD5 authentication, NAT at the WAN edge, and layered L2 security (Port‑Security, DHCP Snooping, DAI).

1) Scenario & Objectives

Organization: KCB Bank — HQ site

Goal: Provide segmented connectivity for departments (Customer Service, Finance & Accounting, HR, IT/Network, ATM zone) with secure internet egress via an ISP. Demonstrate enterprise‑grade design patterns you’d expect in a financial environment.

What you’ll see in this lab:

Inter‑VLAN routing on a Layer‑3 core switch (Catalyst 3560)

Routed point‑to‑point to an edge router for WAN/ISP

OSPFv2 peering (MD5 authentication) between core and edge

Static + default routing logic (fail‑safe simplicity at the edge)

NAT overload at the edge for private subnets

VLAN hardening: access/trunk best practices, native VLAN quarantine, DTP disabled

L2 security: Port‑Security, BPDU‑Guard, DHCP Snooping, Dynamic ARP Inspection, IP Source Guard

L3 security: granular inter‑VLAN ACLs (principle of least privilege)

Network services: DHCP (per‑VLAN scopes), NTP, logging, SNMPv3, SSH‑only management

Optional add‑ons for polish: NetFlow (NBAR‑lite), QoS marking, banners, and structured comments

Why this matters : It demonstrates comfort with both design trade‑offs and hands‑on CLI across switching, routing, and security — the trio most common in campus/branch roles.


Topology (at a glance)

Devices:

HQ‑CORE: Catalyst 3560 (Layer‑3)

HQ‑ACCESS: Catalyst 2960 (Layer‑2)

EDGE‑RTR: Cisco ISR (WAN/ISP)

ISP Cloud (simulated)

Linking: Access ports to PCs, trunks between switches, routed port from CORE → EDGE.


3) IP Plan & VLANs

VLAN	Name	Subnet	SVI (CORE)

10	CUSTOMER_SERVICE	20.0.17.0/24	20.0.17.2

40	FINANCE_ACCOUNTING	10.5.10.0/24	10.5.10.1

30	HR	130.9.1.0/24	130.9.1.1

20	ATM_ZONE	11.0.1.0/24	11.0.1.3

50	IT_NETWORK	124.9.1.0/24	124.9.1.12
P2P	Purpose	Subnet	CORE IP	EDGE IP
PTp	CORE ↔ EDGE	37.20.10.0/30	37.20.10.2	37.20.10.1
WAN	EDGE ↔ ISP	192.168.5.32/30	192.168.5.34	192.168.5.33

Routing model:

OSPF area 0 between CORE and EDGE (MD5 auth)

CORE advertises VLAN SVIs to EDGE; EDGE has default route to ISP and injects a default into OSPF

NAT overload at EDGE out the ISP interface


4) Security & Best‑Practice Highlights

Switching hygiene:

All access ports: switchport mode access, DTP disabled, BPDU‑Guard ON

Trunks: Allowed VLAN list restricted; native VLAN 99 (unused / blackhole)

Storm‑control on access ports (broadcast/multicast/unicast)

L2 protections: DHCP Snooping + DAI + IP Source Guard on user ports

L3 controls: Inter‑VLAN ACLs to segment Finance/HR/ATM as a bank would require

Edge hardening: SSHv2 only, strong crypto, no password reuse, logging, NTP

Observability: SNMPv3 (authPriv), local syslog buffer


What I’d do in production (next steps)

Redundant core (HSRP/VRRP + dual uplinks), link aggregation (LACP), and rapid‑PVST/MST tune

Centralized syslog/NetFlow collector, TACACS+ for AAA

IPS/IDS at the edge, next‑gen firewall for app‑aware policies

Site‑to‑site VPN for DR/branch interconnects


Author - Bradleyy Giovanni  |  Network Engineer  |  Network Administartor  |  Adoured by ISP Companies & Start-up Enterprise Infrastructure 

EMAIL - giovanniibradley@gmail.com

 [![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/bradley-giovanniii293) 
