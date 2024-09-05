To start off we are told HQ and branch should have 1 core router each, each core router connecting to two L3 switches, which in turn connects to Several Access Layer switches. The core routers should also be connected to 2 ISPs. Theres a server site too, which will be connecting to the HQ core router directly. I amde a smple network using draw.io to understand what our final topology might look like. 

## Diagram from Draw.io

<img src="https://i.imgur.com/QWCF2Wk.png" height="85%" width="85%" alt="Disk Sanitization Steps"/>

- Before doing any sort of configurations we will first make the whole topology in cisco packet tracer.

- The core routers are to be connected with serial dce connection. But when you click on the router, you wont see any serial itnerfaces, as shown below.

<img src="https://i.imgur.com/XFgF3H7.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- In order to turn on the serial interfaces, we must click on the router, turn it off, drag and drop the HWIC module to add it to the router, and turn the router back on. Use this method on all the routers.

<img src="https://i.imgur.com/A0tuUuH.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- As shown below, we can see the serial interfaces now.
<img src="https://i.imgur.com/rMFpC5f.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- Our router connections will look like below.

<img src="https://i.imgur.com/TPa41hK.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- Now the routers on the left will be connected to 2 Layer 3 switches, which in turn will be connected to Layer 2 switches, and finally L2 switches will connect to 1PC, 1 Smartphone, 1 Printer, 1 Access Point, and 1 laptop. Once we are done connecting them with proper cables, we get the below diagram. Same connections will be done for 2 core routers on the right.

<img src="https://i.imgur.com/gLxQaDI.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

## Turning on the devices
- First we will turn on the Serial Interfaces in the 2 ISP routers, and tunr on all interfaces on the Core Routers. Click on the router, go to config, and turn on the previosu said interfaces.

<img src="https://i.imgur.com/o510fXs.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

## Turning on all the L3 Switches
- For L3 switches, click on it, drag and drop the AC power supply. Thats all.

<img src="https://i.imgur.com/kPKCP0B.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>


- Final topology after labelling everything looks like below. With this we now move to configuration steps.

<img src="https://i.imgur.com/69cNL1W.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

## Configuration Steps
1. Basic settings on all devices and setting up ssh on routers and L3 switches
2. Vlan Assignment plus all access and trunk ports on L2 and L3 switches.
3. Switchport security to Finance department
4. Subnetting and IP Addressing
5. OSPF on the routers and L3 switches
6. Static IP addresses to server room
7. DHCP server device configurations
8. Inter vlan routing on L3 switches and ip dhcp helper address
9. Wireless networm config
10. Site to site IPSec VPN.
11. Default static route
12. APT + ACL
13. Testing config.





















