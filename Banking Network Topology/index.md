# Configurations

1. **Router Setup**
    - Connect routers with DCE cables.
    - Turn on the serial interfaces and set the clock rate accordingly.
    - Note: When connecting using serial DCE cables, the serial interfaces may not be visible initially. 
      - **Steps:**
        1. Turn off the router.
        2. Insert the HWIC (High-Speed WAN Interface Card) module.
        3. Turn the router back on.
<img src="https://i.imgur.com/R9WlCbT.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/HKg6ssq.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
# Note

We will also connect **Router 1** to **Router 4** and **Router 2** to **Router 3** using a standard crossover cable. This setup ensures network redundancy, meaning if one router fails, there is a secondary path available for network traffic.

Our final router connection will look like this:

- **Router 1** <-> **Router 4** (via crossover cable)
- **Router 2** <-> **Router 3** (via crossover cable)
<img src="https://i.imgur.com/ObCDZ81.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
2. Now, connect these routers to the **Layer 3 switch**. Ensure redundancy by connecting them to another router as shown below.
  <img src="https://i.imgur.com/MdIEFDl.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

3. Our final topology, before configuring anything, looks like this:
 <img src="https://i.imgur.com/qU3iKMj.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
 

4. Going back to point 1, we need to turn on the interfaces and enable the clock rate on the correct ones. 

   - For the interfaces that have a clock symbol, the clock rate needs to be enabled.
      <img src="https://i.imgur.com/jKSTPNa.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
   - Alternatively, you can click on the router, navigate to the `Config` tab, and turn on the interfaces as shown below. However, using CLI commands is recommended.
      <img src="https://i.imgur.com/2itX7I9.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

5. Clock rate configuration:

   - The clock rate can be easily configured on interfaces connected via DCE cables.
   - Interfaces with a clock symbol indicate the need to enable the clock rate.

## Layer 3 Switch Configurations

1. **Add Power Supply to Each Layer 3 Switch**
   - Drag and drop the power supply into any empty slot of the Layer 3 switch.
   - Ensure that the power supply is securely attached before proceeding with any further configurations.
  <img src="https://i.imgur.com/kYcADgg.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

 

```markdown
## Configuring Hostnames, Line Console, VTY Passwords, and Disabling Domain IP Lookup

All commands will be executed on the switch.

```plaintext
# Enter privileged exec mode
en
conf t

# Configure hostname and banner
hostname Switch
banner motd "This is Layer 2 Switch"

# Access console line config mode and set a password
line console 0
password hammaz
login
exit

# Configure VTY lines for remote access (Telnet/SSH) and set a password
line vty 0 15
password hammaz
login
exit

# Disable domain IP lookup and encrypt passwords
no ip domain-lookup
enable password hammaz
service password-encryption
```

Note: I will be copy-pasting these commands on the switch, hence I'll use the same hostname on all.


<img src="https://i.imgur.com/adyrzZo.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>


```markdown
## L3 Switch Configuration

The steps are similar to the previous configurations, but we also add SSH configuration for L3 switches and routers.

```plaintext
# Enter privileged exec mode
en
conf t

# Configure hostname and banner
hostname L3Switch
banner motd "This is Core Router"

# Access console line config mode and set a password
line console 0
password hammaz
login
exit

# Disable domain IP lookup and encrypt passwords
no ip domain-lookup
enable password hammaz
service password-encryption

# Configure SSH settings
ip domain-name cisco.net
username hammaz password hammaz1
crypto key generate rsa
1024

# Configure VTY lines for SSH access
line vty 0 15
login local
transport input ssh

# Exit configuration mode and save the configuration
exit
exit
wr
```

## Assigning VLANs and switch port security to access layer switches.

 <img src="https://i.imgur.com/klEl6Tt.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

For all access layer switches the port inside purple circle will be access port, and the other two will be trunk ports.
Commands are simple as shown below.

 <img src="https://i.imgur.com/6PycTGc.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

 ## Port Security Configuration

For all access layer switches, configure port security with a maximum of 2 MAC addresses, using sticky MAC address learning, and set the violation mode to shutdown.

### Configuration Commands

```plaintext
# Enter privileged exec mode
en
conf t

# Select the interface to configure port security (e.g., FastEthernet0/1)
interface FastEthernet0/1
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation shutdown
exit

# Repeat the above configuration for additional interfaces as needed
# Example for FastEthernet0/2
interface FastEthernet0/2
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation shutdown
exit

# Exit configuration mode and save the configuration
exit
wr
 <img src="https://i.imgur.com/riJgecK.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
The above commands are done on all layer 2 switches.

The below interfaces on the L3 switch will be trunk ports too.

 <img src="https://i.imgur.com/i05gEVF.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
### Configuration Commands

```plaintext
# Enter privileged exec mode
en
conf t

# Configure the interface as a trunk port
interface GigabitEthernet1/0/2
switchport mode trunk
exit

# Repeat the above configuration for additional trunk interfaces as needed
# Example for GigabitEthernet1/0/3
interface GigabitEthernet1/0/3
switchport mode trunk
exit

# Exit configuration mode and save the configuration
exit
wr
```

## IP Addressing Scheme

### Base Network: 192.168.10.0

#### 1st Floor
| Department | Network Address | Subnet Mask          | Host Address Range            | Broadcast Address |
|------------|------------------|----------------------|-------------------------------|--------------------|
| Marketing  | 192.168.10.0     | 255.255.255.192/26   | 192.168.10.1 to 192.168.10.62 | 192.168.10.63     |
| Research   | 192.168.10.64    | 255.255.255.192/26   | 192.168.10.65 to 192.168.10.126| 192.168.10.127    |
| HR         | 192.168.10.128   | 255.255.255.192/26   | 192.168.10.129 to 192.168.10.190| 192.168.10.191    |

#### 2nd Floor
| Department | Network Address | Subnet Mask          | Host Address Range            | Broadcast Address |
|------------|------------------|----------------------|-------------------------------|--------------------|
| Marketing  | 192.168.10.192   | 255.255.255.192/26   | 192.168.10.193 to 192.168.10.254| 192.168.10.255    |
| Accounts   | 192.168.11.0     | 255.255.255.192/26   | 192.168.11.1 to 192.168.11.62 | 192.168.11.63     |
| Finance    | 192.168.11.64    | 255.255.255.192/26   | 192.168.11.65 to 192.168.11.126| 192.168.11.127    |

#### 3rd Floor
| Department | Network Address | Subnet Mask          | Host Address Range            | Broadcast Address |
|------------|------------------|----------------------|-------------------------------|--------------------|
| Logistics  | 192.168.11.128   | 255.255.255.192/26   | 192.168.11.129 to 192.168.11.190| 192.168.11.191    |
| Customer   | 192.168.11.192   | 255.255.255.192/26   | 192.168.11.193 to 192.168.11.254| 192.168.11.255    |
| Guest      | 192.168.12.0     | 255.255.255.192/26   | 192.168.12.1 to 192.168.12.62 | 192.168.12.63     |

#### 4th Floor
| Department   | Network Address | Subnet Mask          | Host Address Range            | Broadcast Address |
|--------------|------------------|----------------------|-------------------------------|--------------------|
| Admin        | 192.168.12.64    | 255.255.255.192/26   | 192.168.12.65 to 192.168.12.126| 192.168.12.127    |
| ICT          | 192.168.12.128   | 255.255.255.192/26   | 192.168.12.129 to 192.168.12.190| 192.168.12.191    |
| Server-Room  | 192.168.12.192   | 255.255.255.192/26   | 192.168.12.193 to 192.168.12.254| 192.168.12.255    |

## IP Addressing for Routers and Layer 3 Switches

### Base Network: 10.10.10.0/30

| No. | Network Address | Subnet Mask        |
|-----|------------------|--------------------|
| 1   | 10.10.10.0       | 255.255.255.252    |
| 2   | 10.10.10.4       | 255.255.255.252    |
| 3   | 10.10.10.8       | 255.255.255.252    |
| 4   | 10.10.10.12      | 255.255.255.252    |
| 5   | 10.10.10.16      | 255.255.255.252    |
| 6   | 10.10.10.20      | 255.255.255.252    |
| 7   | 10.10.10.24      | 255.255.255.252    |
| 8   | 10.10.10.28      | 255.255.255.252    |
| 9   | 10.10.10.32      | 255.255.255.252    |
| 10  | 10.10.10.36      | 255.255.255.252    |
| 11  | 10.10.10.40      | 255.255.255.252    |
| 12  | 10.10.10.44      | 255.255.255.252    |
| 13  | 10.10.10.48      | 255.255.255.252    |
| 14  | 10.10.10.52      | 255.255.255.252    |


<img src="https://i.imgur.com/dzo1o77.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

Before configuring IP addresses on L3 interfaces we need to use the command “no switchport” on the router-facing interfaces, to make that interface a “Layer 3 Interface”.

<img src="https://i.imgur.com/KJQGiTk.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/T4VbjEM.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

## Configuring OSPF on All Routers and Layer 3 Switches
How to know which networks will be advertised by the routers and switches? A simple trick you can use is see how many networks they are connecting to.

<img src="https://i.imgur.com/PK7LwIC.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
### OSPF Configuration for R1

Configure OSPF on R1 with the following commands:

```plaintext
Router ospf 10
network 10.10.10.0 0.0.0.3 area 0
network 10.10.10.4 0.0.0.3 area 0
network 10.10.10.16 0.0.0.3 area 0
network 10.10.10.28 0.0.0.3 area 0
network 10.10.10.32 0.0.0.3 area 0
```
<img src="https://i.imgur.com/AxCH4v6.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
