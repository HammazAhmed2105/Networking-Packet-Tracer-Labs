# Bank Network Design for Radeon Company Ltf.

**Objective:** Design and implement a network for Radeon Company Ltf.'s new branch located in Nairobi, Kenya.

## Floor Layout and Department Details

### 1st Floor
| No | Department      | No. of PCs | No. of Printers |
|----|-----------------|------------|-----------------|
| 1  | Management      | 20         | 4               |
| 2  | Research        | 20         | 4               |
| 3  | Human Resource  | 20         | 4               |

### 2nd Floor
| No | Department      | No. of PCs | No. of Printers |
|----|-----------------|------------|-----------------|
| 1  | Marketing       | 20         | 4               |
| 2  | Accounting      | 20         | 4               |
| 3  | Finance         | 20         | 4               |

### 3rd Floor
| No | Department      | No. of PCs | No. of Printers |
|----|-----------------|------------|-----------------|
| 1  | Logistics       | 20         | 4               |
| 2  | Customer Care   | 20         | 4               |
| 3  | Guest           | 40         | 2               |

### 4th Floor
| No | Department     | No. of PCs | No. of Servers  |
|----|----------------|------------|-----------------|
| 1  | Administration | 2          | DHCP            |
| 2  | ICT            | 2          | HTTP            |
| 3  | Server Room    | 2 admin PCs| Email           |

## Network Requirements
1. **Network Visualization:** Use a software modeling tool (e.g., [draw.io](https://app.diagrams.net/)) to visualize the network topology.
2. **Router Configuration:** Deploy one router on each floor.
3. **Routing Protocol:** Implement OSPF for route advertisement across all routers.
4. **Wireless Network:** Each department must have a wireless network.
5. **User Devices:** Each department, excluding the server, should support 60 users (both wired and wireless).
6. **Device Communication:** Ensure that devices across all departments can communicate with each other.
7. **IP Addressing:** Assign IP addresses dynamically using DHCP.
8. **Server Configuration:** Set up HTTP and Email servers.
9. **Remote Management:** Configure SSH on all routers for secure remote login.

## Steps to Implement the Network

```bash
# Step 1: Configure OSPF on each router
router ospf 1
 network 192.168.0.0 0.0.255.255 area 0

# Step 2: Set up DHCP server for IP address assignment
ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1

# Step 3: Configure SSH on each router for remote management
hostname <router_name>
ip domain-name radeoncompany.com
crypto key generate rsa
ip ssh version 2
line vty 0 4
 transport input ssh
 login local
