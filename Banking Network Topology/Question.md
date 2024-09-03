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

## Basic Configurations to be Done on Devices

- **Hostnames**
- **Line console and VTY passwords**
- **Banner messages**
- **Disable domain IP lookup**

## VLAN Configuration for Departments

- Create VLANs for each department
- Ensure each VLAN is associated with a different subnetwork

## Planning of IP Addresses

- **Base Address:** 192.168.10.0
- Perform subnetting based on the number of hosts in each department
- Identify the following for each subnet:
  - Subnet mask
  - Usable IP Address range
  - Broadcast address

## End Devices

- Configure all end devices in the network with appropriate IP addresses based on the calculations above

## Configure Port-Security

- Use the `sticky` command to obtain MAC addresses
- Set violation mode to `shutdown`

## Test Communications

- Ensure communication is working correctly among all end devices



