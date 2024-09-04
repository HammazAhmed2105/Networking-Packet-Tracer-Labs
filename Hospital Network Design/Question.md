# Case Scenario

## Melbourne Health Services

Melbourne Health Services is a well-established healthcare provider in Australia, offering a range of health solutions and services to its clients. The institution operates in two locations within the same city, with the hospital headquarters 20km away from the branch hospital. 

### Headquarters Departments
- Medical Lead Operations & Consultancy Services
- Medical Emergency Reporting
- Medical Records Management
- IT
- Customer Service

### Branch Hospital Departments
- Nurse and Surgery Operations
- Hospital Labs
- HR
- Marketing
- Finance

### Guest Areas
Each location includes a Guest Area for patients and visitors.

## Current Network Setup
The network currently relies on third-party services for IT maintenance. Senior management has decided to implement their own network infrastructure, utilizing LAN, WAN, and server-side components located separately at the headquarters. The server side will host:
- DHCP Server
- DNS Server
- Web Server
- Email Server

## Network Design Requirements
- **Cost-Effective:** The network should be designed with cost-efficiency in mind.
- **Information Security:** The network must adhere to the CIA (Confidentiality, Integrity, and Availability) principles.
- **Hierarchical Model:** The network will use a hierarchical model with two core routers (one at HQ and one at the Branch), each connecting to two subscribed ISPs.
- **Separate Network Segments:** All departments will be on separate network segments within the same LAN.

## Your Role
As a Network Security Engineer, you are tasked with:
- Designing the network according to the senior management’s requirements.
- Consulting an appropriate robust network design model to meet these requirements.
- Implementing access control lists (ACLs) and Virtual Private Network (VPN) solutions to ensure secure communication.
- Considering security and network performance factors to safeguard the CIA of data and communication.

# Network Design and Implementation Requirements

1. Use Cisco Packet Tracer to design and implement.
2. Use a hierarchical model to provide redundancy in the network.
3. Both HQ and branch routers are expected to be connected using a serial connection.
4. For network cost-effectiveness, each site is expected to have one core router, two multilayer switches, and several access switches connecting each department.
5. Each department is expected to have a wireless network for users.
6. Each department in HQ is expected to have around 60 users, while the branch has 30 users.
7. Each department should be in a different VLAN and a different subnet.
8. Base network: `192.168.100.0/24`, carry out subnetting to allocate the correct number of IP addresses to each department.
9. The company network is connected to the static public IP address (Internet protocol): `195.136.17.0/30`, `195.136.17.4/30`, `195.136.17.8/30`, `195.136.17.12/30`, connected to the two Internet providers.
10. Configure basic device settings such as hostnames, console password, enable password, banner messages, and disable IP domain lookup.
11. Devices in all departments are required to communicate with each other with the respective multilayer switch configured for Inter-VLAN routing.
12. The multilayer switches are expected to carry out both routing and switching functionalities and thus will be assigned IP addresses.
13. All devices in the network are expected to obtain IP addresses dynamically from the dedicated DHCP server located in the server room.
14. Devices in the server room are to be allocated IP addresses statically.
15. Use OSPF as the routing protocol to advertise the routes on the routers and multilayer switches.
16. Configure default static routing to enable routers and multilayer switches to forward any traffic that does not match routing table entries. Use next-hop IP addresses.
17. Configure SSH in all routers and layer 3 switches for remote login.
18. Configure port security for the server site department switch to allow only one device to connect to a switch port, use sticky method to obtain MAC address, and set violation mode to shutdown.
19. Configure the extended ACL rule together with site-to-site VPN (IP Sec VPN) to create a tunnel and encrypt communication between HQ and Branch network.
20. Configure PAT to use the respective outbound router interface IPv4 address, and implement the necessary ACL rule.
21. Test communication, ensure everything configured is working as expected.

