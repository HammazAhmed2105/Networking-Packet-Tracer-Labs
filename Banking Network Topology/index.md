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

