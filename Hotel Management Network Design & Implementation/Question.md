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
