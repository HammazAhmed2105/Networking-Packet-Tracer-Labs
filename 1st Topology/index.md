As part of your end year networking project, you are required to design and implement Vic Modern Hotel Network. The hotel has three floors: 
- On the first floor, there are three departments: Reception, Store, and Logistics.
- On the 2nd floor, there are three departments: Finance, HR, and Sales.
- On the 3rd floor, there are IT and Admin.

1. There should be three routers connecting each floor (all placed in the server room in the IT department).
2. All routers should be connected using serial DCE cables.
3. The network between the routers should be 10.10.10.0/30, 10.10.10.4/30, and 10.10.10.8/30.
4. Each floor is expected to have one switch (placed on the respective floor).
5. Each floor is expected to have WIFI networks connected to laptops and phones.
6. Each department is expected to have a printer.
7. Each department is expected to be in a different VLAN:

1st Floor:
- Reception – Vlan 80, network of 192.168.8.0/24
- Store – Vlan 70, network of 192.168.7.0/24
- Logistics – Vlan 60, network of 192.168.6.0/24

2nd Floor:
- Finance – Vlan 50, network of 192.168.5.0/24
- HR – Vlan 40, network of 192.168.4.0/24
- Sales – Vlan 30, network of 192.168.3.0/24

3rd Floor:
- Admin – Vlan 20, network of 192.168.2.0/24
- IT – Vlan 10, network of 192.168.1.0/24

8. Use OSPF as the routing protocol to advertise routes.
9. All devices are expected to obtain IP addresses dynamically, with their respective routers configured as the DHCP server.
10. All devices are expected to communicate with each other.
11. Configure SSH in all the routers for remote login.
12. In the IT department, add a PC called Test-PC to port fa0/1 and use it to test remote login.
13. Configure port security on the IT-dept switch to allow only Test-PC to access port fa0/1 (use sticky method to obtain MAC address with violation mode of shutdown).

### Step 2 - After putting 3 routers, we need to connect them using serial DCE cables.

- We cannot use the cable directly; we must first enable serial interfaces on the routers.
- First, turn off the router (click on the router, and click the circle above the green line), and then drag and drop “HWIC-2T” from the left column and place it, then turn on the router again. Do this for all routers.
- Now, when we click on the router, we can see the serial interfaces. Connect them.

Side note – Serial interfaces provide a physical layer connection that can be used to connect routers across long distances.

### Connect the switches to the routers:
- Floor 1 has 3 departments. Each department must have 1 printer.
- Place the appropriate number of end devices on each floor. Also, place Wi-Fi on each floor and connect it to the switch.
- We are now done labeling everything, and our diagram looks like this.

One thing to note is, we must enable clock rate on Serial DCE interfaces. If we hover over the red lines, we see the interface name. If it has a clock beside it, then we enable it on that interface only.

Before enabling the clock rate, we must also use `no shutdown` since router interfaces are shut down by default.

### For R3:
### For R1:
- No config needed except turning the interfaces on.

### For R2:
- The config is almost the same.

Now we set up VLANs. Since there are different VLANs, we will configure them using access port, but the link between the router and switch will be a trunk.

For respective interfaces, we use `switchport mode access`, followed by `sw access vlan “number”`. For switch one, f0/1 is connected to the router. Hence, it’s the trunk link. Do the same for all other switches.

### Configuring IP addresses in the router:
- Now we have /30 notations on the routers, so that means there will be 2 IP addresses.

For example:
- `se0/2/0` gets `10.10.10.5`.
- `se0/2/1` gets `10.10.10.9` since it has the network `10.10.10.8/30`.

We go on the router and use the command `ip address 10.10.10.9` followed by the subnet mask, which is `255.255.255.252`.

Do this for all 3 routers. Once done setting the IP addresses, we can check which interface has which IP address using `do sh ip int br`.

