
To start off we are told HQ and branch should have 1 core router each, each core router connecting to two L3 switches, which in turn connects to Several Access Layer switches. The core routers should also be connected to 2 ISPs. Theres a server site too, which will be connecting to the HQ core router directly. I amde a smple network using draw.io to understand what our final topology might look like. 

## Diagram from Draw.io

<img src="https://i.imgur.com/QWCF2Wk.png" height="85%" width="85%" alt="Disk Sanitization Steps"/>

- Before doing any sort of configurations we will first make the whole topology in cisco packet tracer.

- The core routers are to be connected with serial dce connection. Why? Core routers use serial DCE connections for clocking control and synchronization over point-to-point WAN links.
- But when you click on the router, you wont see any serial itnerfaces, as shown below.

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

## HQ Network

| Department | Network Address  | Subnet Mask         | Host Address Range                | Broadcast Address |
|------------|------------------|---------------------|-----------------------------------|-------------------|
| MLOC       | 192.168.100.0     | 255.255.255.192/26  | 192.168.100.1 to 192.168.100.62   | 192.168.100.63    |
| MER        | 192.168.100.64    | 255.255.255.192/26  | 192.168.100.65 to 192.168.100.126 | 192.168.100.127   |
| MRM        | 192.168.100.128   | 255.255.255.192/26  | 192.168.100.129 to 192.168.100.190| 192.168.100.191   |
| IT         | 192.168.100.192   | 255.255.255.192/26  | 192.168.100.193 to 192.168.100.254| 192.168.100.255   |
| CS         | 192.168.101.0     | 255.255.255.192/26  | 192.168.101.1 to 192.168.101.62   | 192.168.101.63    |
| Guest      | 192.168.101.64    | 255.255.255.192/26  | 192.168.101.65 to 192.168.101.126 | 192.168.101.127   |

## Branch Network
| Department | Network Address  | Subnet Mask         | Host Address Range                | Broadcast Address |
|------------|------------------|---------------------|-----------------------------------|-------------------|
| Finance    | 192.168.101.128   | 255.255.255.252/27  | 192.168.101.129 to 192.168.101.158| 192.168.101.159   |
| Marketing  | 192.168.101.160   | 255.255.255.252/27  | 192.168.101.161 to 192.168.101.190| 192.168.101.191   |
| HR         | 192.168.101.192   | 255.255.255.252/27  | 192.168.101.193 to 192.168.101.222| 192.168.101.223   |
| Hospital   | 192.168.101.224   | 255.255.255.252/27  | 192.168.101.225 to 192.168.101.254| 192.168.101.255   |
| Surgery    | 192.168.102.0     | 255.255.255.252/27  | 192.168.102.1 to 192.168.102.30   | 192.168.102.31    |
| Guest      | 192.168.102.32    | 255.255.255.252/27  | 192.168.102.33 to 192.168.102.62  | 192.168.102.63    |

## SErver Side Site
| Department | Network Address  | Subnet Mask         | Host Address Range                | Broadcast Address |
|------------|------------------|---------------------|-----------------------------------|-------------------|
| SSS        | 192.168.102.64    | 255.255.255.240/28  | 192.168.102.65 to 192.168.102.78  | 192.168.102.79    |


## Between Routers and L3 switches and ISPs

<img src="https://i.imgur.com/LZalLYf.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- If we want to make a port on a layer 3 routable we need to use the command no switchport on that interface. This way we can assign IP Address to that interface. The interface is marked below.

<img src="https://i.imgur.com/yXtcjr2.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

Once we use no switchport we can simply assign an IP Address using the command shown below.

<img src="https://i.imgur.com/X5GXmx2.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

We do the same above steps on all Layer 3 switches.

### Assigning ip addresses on Routers
- We can use the CLI to assign ip addresses like we did in previous projects, but this time we will use the gui. click on the router, go to config, and choose the interface. 

<img src="https://i.imgur.com/5tftIWb.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- We have to note that the first usable address is given to L3 interface. Now we assign accordingly.

<img src="https://i.imgur.com/BVlRjHZ.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

Use the above steps on other routers and their interfaces.

- We can use the command "do show ip interfaces brief" from global config mode, to see if we assigned the ip Addresses correctly.

# -----

# Step 5

## OSPF on the routers and L3 switches
- OSPF is enabled on devices that route packet. That means L3 Switches and Routers.
- How to know which netowrk will be included for OSPF? Check which network are the switches and routers directly connected to. As shown below the L3 switch is connected to 6 Vlan departments and 1 core router.

<img src="https://i.imgur.com/l4yp4Dh.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

### Note - We need to issue the command **ip routing** on L3 switches.
- According to point 16 in question "Configure default static routing to enable routers and multilayer switches to forward any traffic that does not match routing table entries. Use next-hop IP addresses.". We need to give a next hop address as we. Since Layer 3 switch is connected to the network 192.168.102.80/30, the next hop adress we will use is 192.168.102.82.
- Command  - **ip route 0.0.0.0 0.0.0.0 192.168.102.82**

<img src="https://i.imgur.com/NQCbxUN.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- We can confirm that the gateway of last resort is set by using the command **do sh ip route**

<img src="https://i.imgur.com/Qt3D7xW.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- The configurtion for HQ Layer 3 other switch is the same as above, except the default route will be 192.168.102.86.

- Below are the configurations for Branch Layer 3 Switch.

<img src="https://i.imgur.com/0niYsay.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- The configurtion for Branch Layer 3 other switch is the same as above, except the default route will be 192.168.102.98

### Configuring OSPF on the Routers

- Similar to above we configure it similarly bu noting down which ever network that router is connected to.
- For isntance the below router is connected to 192.168.102.80/30, 195.136.17.0/30, 195.136.17.4/30, and 192.168.102.88/30, 192.168.102.64/28, 192.168.102.84/30, 192.168.102.80/30

<img src="https://i.imgur.com/LmFebwj.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- One thing to note about core routers is, they have redundancy with ISPs. So if one goes down it will use the other as its default route. So there will be 2 Default routes.

<img src="https://i.imgur.com/B012OQ6.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- Below are the configurations for Branch Router. Very similar.

<img src="https://i.imgur.com/s8FyyLW.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- Configuration of OSPF on the ISP rotuers are the same, except the network will change accordingly.

# -----

# 6. Static IP addresses to server room

<img src="https://i.imgur.com/9Epjgvr.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- As shown above, we need to assign static IP Address with the network, 192.168.102.64/28.
- The default route is 192.168.102.65, and dns server 192.168.102.68

<img src="https://i.imgur.com/SerHNs7.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

Accoridngly for Email, DNS, and HTTP server the ip address will be 192.168.102.67, 192.168.102.68, 192.168.102.69
# -----

# 7. DHCP server device configurations

- The DHCP server mentioned and showed in Step 6, will be reposible for allocating IP Addresses to all other departments. We will create pools for that. Theres 12 departments therefore there will be 12 pools created.
- First we click on the DHCP server, click on services tab on top, and choose DHCP from the left column. 

<img src="https://i.imgur.com/rTS0NZQ.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- The default gateway will be first usable IP Address for that department and network. So for the MOC department the network is 192.168.100.0, therefore the default gateway will be first usable ip address, that is 192.168.100.1/
- The DNS server is 192.168.102.68, as we assigned in step 6. This will be same for all departments.
- You can choose the starting IP address of hosts as you like, as long as they are in the designated network, and dont over lap.
-  See below for reference

<img src="https://i.imgur.com/fXvgra7.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- Below are shown, all the pools that I created.

<img src="https://i.imgur.com/qqepcYt.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

# -----

# 8. Inter vlan routing on L3 switches and ip dhcp helper address

### Configurations for Layer 3 Switches
- For each Vlan, for example Vlan 10, we assign the default address of the network, and then we use the command ip helper address and use the dhcp address.

<img src="https://i.imgur.com/v8uSTyk.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

1. **int vlan 10** - Using this command we enter the vlan 10 interface
2. **ip address 192.168.100.1 255.255.255.192**  - We assign the defauly address of the network to this interface.
3. **no ip helper-address 192.168.102.65** - I used the command with no to undo the command. I entered 192.168.102.65, which isn't the DHCP server address. it should be 192.168.102.66, which i corrected in the next step.
- The configuration is similar for other vlans, except the vlan number and default address changes accordingly.

- In order to save time, I first wrote all the commands on notepad and copy pasted them. Since the configuration for HQ Layer switch will be same for both.

<img src="https://i.imgur.com/SwJ39F4.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/f3CnvUE.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- We do the same exact commands on HQs other router/
- Below are the commands for Branch Layer 3 switch.

<img src="https://i.imgur.com/4MWIEJu.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

### Inter-Vlan config on router connected to server side site.
- First we do the configuration on the inteface thats shown below.

<img src="https://i.imgur.com/vBKjzZW.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- The interface is g0/2, and we need to remove the ip address from that interface, and then create sub interface, and assign the removed IP address to it, as shown below.

<img src="https://i.imgur.com/hlavQ2d.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

### Important note 
- I tried going to different desktops, click on them, and turning on DHCP, but I faced issues, the DHCP wasnt able to assign IP Addresses. After spending some time I realized, when we made pools on the DHCP device, I forgot to turn them on. So make sure you click on. See below screenshot for reference.
  
<img src="https://i.imgur.com/friuLnx.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- The guest departments are still not working. We will figure that out later on.

# -----

# 9. Wireless network config

1. First we need to make some configurations on the access point, followed by the smartphone and laptop.

### Configurations on the Access Point

<img src="https://i.imgur.com/KZI8Iwa.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- Click on the access point, followed by config, then port 1. Enter the below details.

<img src="https://i.imgur.com/s4PNKjE.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- We put in the SSID, which basically means the name of the router. Followed by WPA2 authentication, which is the most secure available on thie access point. Adn then we choose a passphrase, of our choice.

### Configurations on the Laptop

- Click on the laptop, and as shown in below image, first turn it off, and drag the module thats already there, and remove it. Drag and put the module named WPC300N. This will let te laptop connect to the access point. Dont forget to turn on the laptop again.

<img src="https://i.imgur.com/yGOpARx.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- Now click on config, followed by wireless0 from the left pane. Do the below marked configuration. 

<img src="https://i.imgur.com/YxtlLdo.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

### Configurations on the Smart Phone

- The configurations of smart phone is jsut putting in the SSId, and password.

<img src="https://i.imgur.com/MHGteBI.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

The above steps apply to all the access points, laptops, and smartphone in all other department. Ofcourse, the SSID,password changes accordingly.

# -----

## Note - According to our topology, we need to configure site to site VPN, default static route, and then PAT + ACL. For reasons I'll configure PAT and ACL first hence we will do step 12 first.

### Step 12 -PAT + ACL
- Port Address Translation (PAT) is used to allow multiple devices on a local network to share a single public IP address by mapping unique port numbers for each session, enabling efficient use of limited public IP addresses.
- We will be enabling PAT on the 2 core routers.
- Also, clock rate should be set on appropriate interfaces, on the core routers for efficient use of PAT. Why?
- Setting the clock rate is required on the DCE side of a serial connection to establish proper timing for data transmission. However, clock rate configuration and Port Address Translation (PAT) are not directly related. The clock rate is necessary for the serial link to function, while PAT operates on IP addressing and port mapping. Both need to be configured properly for successful data flow, but the clock rate specifically ensures the serial link's communication timing, which is essential before enabling any routing or NAT/PAT functionality.
- In order to set clock rate, hover over the routers serial cables, if theres a clock sign beside the interface name we need to set a clock rate.

<img src="https://i.imgur.com/t8dp4q3.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- The interface on the left has a clock sign therefore we assign a clock rate of 64000 on it.

<img src="https://i.imgur.com/7KS5JdD.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- Do this for all required interfaces.

### Configuring PAT

- The PAT will be configured on HQ and Branch core routers. Either outside or inside.
- The interfaces marked in green will be ip nat inside, while the black ones will be outside.

<img src="https://i.imgur.com/8idlh7a.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- Commands on HQ core router is shwon below.
<img src="https://i.imgur.com/EPphc2m.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- We do the same commands on Branch router but the interfaces change accoridngly.
  
<img src="https://i.imgur.com/QGXIxCr.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- Before configuring Access Lists, we need to also use the below commands on routers and add the serial interfaces to them as shwon below. On both HQ and Branch router.

<img src="https://i.imgur.com/pZMYe5B.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

```bash
Branch-Router(config)#ip nat inside source list 1 interface ser0/3/1 overload
Branch-Router(config)#ip nat inside source list 1 interface ser0/2/0 overload
```

### Allowing correct networks for ACL

- Now we add the networks to the Acces list as shown below.

<img src="https://i.imgur.com/yl9tyex.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>
### Note that we also need to permit the correct networks, but we will do that after configuring IP Sec tunnel.

# -----

# Step 10

##  Site to site IPSec VPN
- In order to create a site to site vpn, we will create a tunnel between Branch and HQ router, so all the traffic flowing between them is encrypted.
- First step is to enable security technology packet on the routers
- Command - **license boot module c2900 technology-package securityk9**

<img src="https://i.imgur.com/ix5MuPC.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- After that type **do reload** to restart the router.
- Do these steps on both Hq and Branch router.

##
-Since we want all traffic from HQ to branch network and vice versa to be encrypted, we need to cofngiure Access control lists.
- We will configure lots of rules, since for instance one network from theHQ side should connect to all networks on the Branch network.
- To reduce workload we will do route summarization, Route summarization is a method where we create one summary route that represent multiple networks/subnets. It's also called route aggregation or supernetting.

# HQ-Route Summarization

192.168.100.0/26  
192.168.100.64/26  
192.168.100.128/26  
192.168.100.192/26  

**Summarized as:** 192.168.100.0/24

192.168.101.0/26  
192.168.101.64  

**Summarized as:** 192.168.101.0/25

# Branch Router Summarization

192.168.101.128/27  
192.168.101.160/27  
192.168.101.192/27  
192.168.101.224/27  
192.168.102.0/27  
192.168.102.32/27  

**Summarized as:** 192.168.101.128/24

## HQ-Router ACL config

-  1st rule on HQ router - **access-list 110 permit ip 192.168.100.0 0.0.0.255 192.168.101.128 0.0.0.255**
- 2nd rule on HQ router - **access-list 100 permit ip 192.168.101.0 0.0.0.127 192.168.101.128 0.0.0.255**

## Branch-Router ACL config  

- 1st rule on Branch router - **access-list 110 permit ip 192.168.101.128 0.0.0.255 192.168.100.0 0.0.0.255**
- 2nnd rule on Branch router - **access-list 110 permit ip 192.168.101.128 0.0.0.255 192.168.101.0 0.0.0.127**

## Creating Internet Key Exchange
















