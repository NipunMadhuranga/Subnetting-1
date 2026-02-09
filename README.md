Project Overview:Subnetting and Static Routing Implementation

This lab demonstrates the efficient use of the 192.168.1.0/24 address space through Variable Length Subnet Masking (VLSM). The project focuses on interconnecting two distinct LAN environments via a WAN backbone using static routing.

1. Address Management and Subnetting
The network is divided into four functional subnets to maximize address efficiency:

LAN Subnets: Large blocks (using /26 masks) provide ample addresses for end-user devices and servers.

WAN Subnets: Serial links between routers utilize /30 masks, which provide exactly two usable IP addresses per link, preventing address wastage.

DHCP Services: A server is utilized to automatically assign IP addresses to PCs within the LANs, ensuring streamlined management.

Management: VLAN 1 interfaces are configured with IP addresses in both LANs for management purposes.

2. Routing Logic (Static Configuration)
To allow communication between the disparate subnets, Static IP Routing was configured on all three routers (R1, IntRouter, and R2).

R1 Configuration: Includes static routes for the 192.168.1.128/30 WAN link and the 192.168.1.192/26 remote LAN, pointing to the next-hop address 192.168.1.66.

IntRouter Configuration: Acts as a central hub, containing routes for the 192.168.1.0/26 network (via 192.168.1.65) and the 192.168.1.192/26 network (via 192.168.1.129).

R2 Configuration: Directs traffic destined for the 192.168.1.0/26 and 192.168.1.64/30 networks through the gateway at 192.168.1.130.

3.Connectivity & Routing Logic

The success of this lab relies on the synchronization between the physical interface states, the static routing table entries, and the end-to-end ICMP verification.

1. Routing Table Analysis (sh ip route)
By using the show ip route command, we verify that each router has a complete "map" of the 192.168.1.0/24 network.

Directly Connected (C): Routers automatically identify subnets physically attached to their FastEthernet or Serial interfaces, such as the /30 WAN links.

Static Routes (S): Since the routers are not using a dynamic protocol like OSPF, manual routes were injected. For example, R1 is told to reach the remote 192.168.1.192/26 subnet by sending traffic to the "Next Hop" IP 192.168.1.66.

L-Routes (Local): The routing table shows specific /32 host routes for the router’s own interface IPs, ensuring the router knows traffic for itself should not be forwarded elsewhere.

2. Connectivity Verification (ping)
The ping command was used to test the Data Plane, ensuring packets can actually travel the path defined in the routing table.

Successful ICMP Replies: The test from PC2 to the remote host 192.168.1.193 resulted in a 0% loss, confirming that every router along the path (R1 → IntRouter → R2) has a valid return route.

Latency Observations: Pings to the local gateway (192.168.1.3) show <1ms latency, while pings across the WAN serial links to remote subnets show slightly higher average times (e.g., 8ms), reflecting the processing time across multiple router hops.

TTL (Time to Live) Insights: The TTL=125 value in remote pings indicates the packet passed through several routers (hops) before reaching its destination, as each router decrements the TTL value by 1.

Project Outcomes
Efficiency: Minimal IP wastage by matching subnet masks to host requirements.

Reachability: Full communication across all four subnets.



Automation: Successful integration of DHCP for end-device configuration.
