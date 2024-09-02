
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

