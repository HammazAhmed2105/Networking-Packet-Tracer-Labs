

```markdown
# Vic Modern Hotel Network Project

As part of your end-of-year networking project, you are required to design and implement the Vic Modern Hotel Network. The hotel has three floors:

- **1st Floor:**
  - Reception (VLAN 80, 192.168.8.0/24)
  - Store (VLAN 70, 192.168.7.0/24)
  - Logistics (VLAN 60, 192.168.6.0/24)
  
- **2nd Floor:**
  - Finance (VLAN 50, 192.168.5.0/24)
  - HR (VLAN 40, 192.168.4.0/24)
  - Sales (VLAN 30, 192.168.3.0/24)
  
- **3rd Floor:**
  - Admin (VLAN 20, 192.168.2.0/24)
  - IT (VLAN 10, 192.168.1.0/24)

## Network Requirements

1. Three routers (placed in the IT department server room) connecting each floor.
2. Routers connected using serial DCE cables.
3. Network between routers: 
   - `10.10.10.0/30`
   - `10.10.10.4/30`
   - `10.10.10.8/30`
4. Each floor has one switch.
5. Wi-Fi networks on each floor connected to laptops and phones.
6. Each department has a printer.
7. Each department is in a different VLAN.
8. Use OSPF as the routing protocol to advertise routes.
9. All devices obtain IP addresses dynamically via DHCP configured on respective routers.
10. All devices communicate with each other.
11. SSH configured on all routers for remote login.
12. Port security on IT department switch (only Test-PC allowed on port fa0/1 using sticky method; violation mode: shutdown).

## Steps to Configure the Network

### 1. Setup Routers and Serial Interfaces
- Place three routers in the IT department server room.
- Enable serial interfaces by adding "HWIC-2T" modules.
- Connect routers using serial DCE cables.
- Set clock rate on serial DCE interfaces and bring up the interfaces using `no shutdown`.

### 2. Connect Switches to Routers
- Each floor has one switch connected to the respective router.
- Place the required number of end devices (PCs, printers, Wi-Fi access points).

### 3. Configure VLANs and Inter-VLAN Routing
- Create VLANs for each department.
- Configure access ports for VLANs and trunk ports between routers and switches.
- Setup inter-VLAN routing using sub-interfaces on the routers.
  
  Example for VLAN 80:
  ```shell
  interface g0/0.80
  encapsulation dot1q 80
  ip address 192.168.8.1 255.255.255.0
  ```
- Configure DHCP for each VLAN on the routers.

### 4. Configure OSPF
- Enable OSPF on all routers to allow communication between different VLANs.
  
  Example for R1:
  ```shell
  router ospf 2
  network 10.10.10.0 0.0.0.3 area 0
  network 192.168.8.0 0.0.0.255 area 0
  ```
  
### 5. Configure SSH on Routers
- Configure SSH on all routers for secure remote access.

  Example commands:
  ```shell
  hostname R1
  ip domain-name vicmodernhotel.local
  username admin password admin123
  crypto key generate rsa
  line vty 0 15
  login local
  transport input ssh
  ```

### 6. Test Network Connectivity
- Verify inter-VLAN communication with ping tests.
- Ensure DHCP is working and devices are getting the correct IP addresses.

### 7. Final Configuration and Documentation
- Save all configurations on routers and switches.
- Document the final network setup.
