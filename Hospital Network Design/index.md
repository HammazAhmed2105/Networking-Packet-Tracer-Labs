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

# -----

## Configuration Steps
1. Basic settings on all devices including setting up ssh on routers and L3 switches
2. Vlan Assignment plus all access and trunk ports on L2 and L3 switches.
3. Switchport security to Server side site department
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
# -----

# Step 1
# Basic settings on all devices and setting up ssh on routers and L3 switches
- For this step we will be configuring hostnames, console password, enable password, banner messages, and disable IP domain lookup, on Layer 2 and Lyaer 3 Switches.

<img src="https://i.imgur.com/4g527Au.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>
- Explanation for each command

1. Switch#conf t 
    **Enters global configuration mode.**

2. Switch(config)#hostname MLOC-Switch 
   **Sets the hostname of the switch to "MLOC-Switch."**

3. MLOC-Switch(config)#enable password hammaz 
   **Sets the privileged EXEC mode password to "hammaz."**

4. MLOC-Switch(config)#no ip domain lookup 
   **Disables DNS lookup when a command is mistyped.**

5. MLOC-Switch(config)#banner motd #No illegal access# 
    **Sets a Message of the Day (MOTD) banner to display "No illegal access" on login.**

6. MLOC-Switch(config)#line console 0 
    **Enters configuration for the console line (physical access port).**

7. MLOC-Switch(config-line)#password hammaz 
   **Sets the console line password to "hammaz."**

8. MLOC-Switch(config-line)#login 
    **Enables login using the set password for the console.**

9. MLOC-Switch(config-line)#exit 
    **Exits console line configuration mode.**

10. MLOC-Switch(config)#do wr 
    **Saves the running configuration to the startup configuration.**

- The configuration is same on all the other switches, except the host name part. So i wrote the commands on Notepad, changed it for each switch and just copy pasted to save time.
  
<img src="https://i.imgur.com/WxB8Hbx.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- Upon checking I realized I forgot to use the command "service password encryption", which encrypts all password. So also include that command at the end.
  
### Configuring the above basic settings on L3 switches and SSH as well.
- For basic settings the commands are same as above, but we also include ssh config commands.
<img src="https://i.imgur.com/lbHkWEA.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>
  
**Below are the commands and basic info on them on how to configure ssh.**

1. **HQ-L3-Switch(config)#ip domain name cisco.net**
    Sets the domain name of the switch to "cisco.net."

2. **HQ-L3-Switch(config)#username hammaz password hammaz**
    Creates a local username "hammaz" with the password "hammaz."

3. **HQ-L3-Switch(config)#crypto key generate rsa**
    Generates RSA keys for SSH encryption.

4. **HQ-L3-Switch(config)#line vty 0 15**
    Configures the virtual terminal (vty) lines 0 through 15 for remote access.

5. **HQ-L3-Switch(config-line)#login local**
    Requires local username and password for vty login.

6. **HQ-L3-Switch(config-line)#transport input ssh**
    Restricts remote access to SSH only (disabling Telnet).

### For the Core routers, the configuration is exactly the same as L3 switches, except the host name should be changed accordingly.

# -----

# Step 2
# Vlan Assignment plus all access and trunk ports on L2 and L3 switches

### Note - Any link between Layer 3 switch and end devices will be access ports, while any link between Layer 2 switch and Layer 3 switch will be trunk ports. 
- From the below image, the interfaces inside the purple circle are trunk, while the others are access ports.

<img src="https://i.imgur.com/ng7okWp.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

### Commands used for trunk and access port configuration

<img src="https://i.imgur.com/rsJ01H1.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

1. Specifies the interface range (FastEthernet 0/3 to 0/5)
MLOC-Switch(config)#**int range f0/3-5**

2. Configuring the interfaces to operate in access mode
MLOC-Switch(config-if-range)#**switchport mode access**

3. Configuring switchport to access a VLAN
MLOC-Switch(config-if-range)#**switchport access vlan 10**

4. Creating VLAN 10
MLOC-Switch(config-if-range)#**vlan 10**

4. Naming VLAN 10 as "MLOC-Dept"
MLOC-Switch(config-vlan)#**name MLOC-Dept**

5. Saving the configuration to the device's memory
MLOC-Switch(config-vlan)#**do wr**
6. Making the ports trunk
MLOC-Switch(config-if-range)#**switchport mode trunk**

The above commands are bery similar for all other Layer 2 switches. We need to tweak the vlan number, name, and interface range a bit.

## Vlan Configurations on Layer 3 Switches
1. Create the appropirate number of vlans on the L3 switch. For instance the HQ switch will have vlan 10-60 while the branch will have the rest.
2. Layer 3 interfaces facing towards L2 switches will be configured as ports.
The interfaces marked below will be trunk.

<img src="https://i.imgur.com/5aKPb9L.png" height="35%" width="35%" alt="Disk Sanitization Steps"/>

<img src="https://i.imgur.com/l3W2hzI.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

# -----

# Step 3
## Switchport security to Server side site department
- For this step we need to configure the switcport security settings on the switch thats directly connected to the server side.

<img src="https://i.imgur.com/rVSjtkl.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- Instruction - Configure port security for server site department switch to allow only one device to connect to a switch port, use sticky method to obtain mac address and violation mode shutdown.

<img src="https://i.imgur.com/llnbacg.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

# -----

# Step 4

## Subnetting and IP Addressing

- Instruction: Base network  - 192.168.100.0, carry out subnetting to allocate correct number of IP addresses to each department.
- For HQ side we are using /26 prefix, while for Branch we are using /27.
- Network between router and L3 switches will use /30 notation.

| Department | Network Address  | Subnet Mask         | Host Address Range                | Broadcast Address |
|------------|------------------|---------------------|-----------------------------------|-------------------|
| MLOC       | 192.168.100.0     | 255.255.255.192/26  | 192.168.100.1 to 192.168.100.62   | 192.168.100.63    |
| MER        | 192.168.100.64    | 255.255.255.192/26  | 192.168.100.65 to 192.168.100.126 | 192.168.100.127   |
| MRM        | 192.168.100.128   | 255.255.255.192/26  | 192.168.100.129 to 192.168.100.190| 192.168.100.191   |
| IT         | 192.168.100.192   | 255.255.255.192/26  | 192.168.100.193 to 192.168.100.254| 192.168.100.255   |
| CS         | 192.168.101.0     | 255.255.255.192/26  | 192.168.101.1 to 192.168.101.62   | 192.168.101.63    |
| Guest      | 192.168.101.64    | 255.255.255.192/26  | 192.168.101.65 to 192.168.101.126 | 192.168.101.127   |
























