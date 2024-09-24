# -Design-and-Implementation-of-a-Multi-Area-OSPF-Network-for-Optimized-Inter-Departmental-Routing-

Project: Multi-Area OSPF Network Design and Implementation

In this project, I designed and implemented a multi-area OSPF network to interconnect departments (ECE, IT, MECH, and CIVIL) in an enterprise environment. The network was divided into three OSPF areas with Area 0 serving as the backbone. I configured routers in each department and utilized Area Border Routers (ABRs) to summarize routes and efficiently manage inter-area communication.

Network Configuration:
I configured four routers, each representing a department, and assigned them to different OSPF areas. For instance:

ECE Router (R1) in Area 1:
router ospf 10
network 1.1.1.1 0.0.0.0 area 1
network 192.168.12.0 0.0.0.255 area 1

IT Router (R2) acting as an ABR between Area 1 and Area 0:
router ospf 10
network 2.2.2.2 0.0.0.0 area 1
network 192.168.12.0 0.0.0.255 area 1
network 192.168.23.0 0.0.0.255 area 0

MECH Router (R3) connecting Area 0 and Area 2:
router ospf 10
network 3.3.3.3 0.0.0.0 area 0
network 192.168.23.0 0.0.0.255 area 0
network 192.168.34.0 0.0.0.255 area 2


CIVIL Router (R4) in Area 2:
router ospf 10
network 4.4.4.4 0.0.0.0 area 2
network 192.168.34.0 0.0.0.255 area 2

Key Steps and Commands:
I configured OSPF areas to efficiently segment the network and reduce the size of the routing table, ensuring each router only processed routes relevant to its own area.
To verify the OSPF configuration, I used:
ping to test connectivity between routers (e.g., from ECE to CIVIL):
ping 4.4.4.4
traceroute to check the path taken by packets:
traceroute 4.4.4.4


show ip route to display the routing table and ensure OSPF routes were being advertised properly:
show ip route

show ip ospf neighbor to verify OSPF adjacencies:
show ip ospf neighbor


Troubleshooting:
During the project, I encountered and resolved several OSPF-related issues, including:

Authentication Mismatch: When configuring authentication on one router but not the other, the OSPF adjacency was stuck in the down state. To resolve this, I implemented the following commands on both routers:
router ospf 10
area 1 authentication message-digest
interface FastEthernet0/0
ip ospf message-digest-key 1 md5 KEY
MTU Mismatch: The MTU size mismatch between routers caused them to be stuck in the exstart state. To fix this, I ensured consistent MTU sizes across interfaces:
interface FastEthernet0/0
mtu 1500


Router ID Conflict: If two routers had the same router ID, OSPF adjacency would not form. I resolved this by manually setting unique router IDs:
router ospf 10
router-id 2.2.2.2
Area Mismatch: Incorrect area configurations (like assigning interfaces to the wrong area) prevented adjacencies from forming. I corrected this by ensuring proper area assignments using the network command under the OSPF process.

Unstable Adjacencies due to MTU Size: Sometimes, even though authentication was correct, the routers would get stuck in the loading state due to mismatched MTU sizes. I fixed this by ensuring both sides of the connection had the same MTU size:
interface GigabitEthernet0/0
mtu 1500


Results:
By properly configuring the multi-area OSPF network, I achieved:

Scalability: Efficient division into areas allowed the network to scale easily, reducing the routing overhead on each router.
Improved Routing Performance: ABRs summarized routes and minimized the size of the routing tables, ensuring faster convergence and optimized inter-area routing.
Network Stability: After resolving issues like authentication mismatches and MTU conflicts, I ensured stable OSPF adjacencies and reliable network communication across all departments.
