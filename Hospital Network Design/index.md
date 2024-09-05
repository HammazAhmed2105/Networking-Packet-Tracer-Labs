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
12. PAT + ACL
13. Testing config.

## Basic settings on all devices and setting up ssh on routers and L3 switches
- For this step we will be configuring hostnames, console password, enable password, banner messages, and disable IP domain lookup, on Layer 2 and Lyaer 3 Switches.

<img src="https://i.imgur.com/4g527Au.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>
- Explanation for each command is shown below.
- Switch#conf t 
# Enters global configuration mode.

Switch(config)#hos 
# Incomplete command, likely a shorthand for entering the hostname.

Switch(config)#hostname MLOC-Switch 
# Sets the hostname of the switch to "MLOC-Switch."

MLOC-Switch(config)#enable password hammaz 
# Sets the privileged EXEC mode password to "hammaz."

MLOC-Switch(config)#no ip dom 
# Incomplete command, should be "no ip domain lookup."

MLOC-Switch(config)#no ip domain lookup 
# Disables DNS lookup when a command is mistyped.

MLOC-Switch(config)#banner motd #No illegal access# 
# Sets a Message of the Day (MOTD) banner to display "No illegal access" on login.

MLOC-Switch(config)#lin 
# Incomplete command, likely referring to "line console."

MLOC-Switch(config)#line con 
# Incomplete command, likely referring to "line console 0."

MLOC-Switch(config)#line console 0 
# Enters configuration for the console line (physical access port).

MLOC-Switch(config-line)#pass 
# Incomplete command, likely referring to "password."

MLOC-Switch(config-line)#password hammaz 
# Sets the console line password to "hammaz."

MLOC-Switch(config-line)#login 
# Enables login using the set password for the console.

MLOC-Switch(config-line)#ex 
# Ambiguous command; doesn't specify what to exit or execute.

MLOC-Switch(config-line)#exi 
# Incomplete command, likely referring to "exit."

MLOC-Switch(config-line)#exit 
# Exits console line configuration mode.

MLOC-Switch(config)#do wr 
# Saves the running configuration to the startup configuration.
















