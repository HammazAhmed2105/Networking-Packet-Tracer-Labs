```markdown
# Configurations

## Router Configurations

1. **Routers Connection:**
   - Routers should be connected with DCE cables.
   - The serial interfaces should be turned on, and the clock rate set accordingly.
   - When connecting using serial DCE cables, if you don't see any serial interfaces, turn off the router, insert the HWIC module, and turn it back on.

   **Note:** We will also connect Router 1 and 4, and Router 2 and 3 together with a normal crossover cable. This will help in network redundancy, meaning if a router fails, there's a secondary path. The final connection for the router will look as shown below.

2. **Router to L3 Switch Connection:**
   - Now connect these routers to the L3 switch.
   - Provide redundancy by connecting them to another router as shown below.

3. **Final Topology:**
   - Our final topology before configuring anything looks like this.

4. **Interface Configurations:**
   - Going back to point 1, we need to turn on the interfaces and enable clock rate on the correct ones.
   - The interfaces that have a clock sign need to have the clock rate enabled.

   Alternatively, you can click on the router, go to `Config`, and turn on the interfaces as shown below. However, I prefer using the commands.

5. **Clock Rate Configuration:**
   - Clock rate can be easily configured on the interfaces that have a clock sign (DCE cables).

## Layer 3 Switch Configurations

1. **Power Supply:**
   - Add power supply to each Layer 3 switch.
   - Drag and drop the power supply into any empty slot.

2. **Basic Configurations (Hostnames, Passwords, Domain Lookup):**
   - Enter privileged exec mode:
     ```
     en
     conf t
     ```
   - Configure hostname and banner:
     ```
     hostname Switch
     banner motd "This is Layer 2 Switch"
     ```
   - Configure line console password:
     ```
     line console 0
     password hammaz
     login 
     exit
     ```
   - Configure VTY line password:
     ```
     line VTY 0 15 
     password hammaz	
     login 
     exit
     ```
   - Disable domain IP lookup and encrypt passwords:
     ```
     no ip domain-lookup
     enable password hammaz
     service password-encryption
     ```
   - These commands are applied on all switches with the same hostname.

3. **L3 Switch Configurations:**
   - Steps are similar to above, with added SSH configuration:
     ```
     hostname L3Switch
     banner motd "This is Core Router"
     line console 0
     password hammaz
     login 
     exit

     no ip domain-lookup
     enable password hammaz
     service password-encryption

     ip domain-name cisco.net
     username hammaz password hammaz1
     crypto key generate rsa
     1024

     line vty 0 15
     login local
     transport input ssh 

     exit
     exit

     wr
     ```

## Assigning VLANs and Switch Port Security to Access Layer Switches

- For all access layer switches, the port inside the purple circle will be the access port, and the other two will be trunk ports.
- Configure port security with a maximum of 2 MAC addresses, learned via sticky MAC addresses, and set the mode to violation shutdown:
  ```
  switchport port-security
  switchport port-security maximum 2
  switchport port-security mac-address sticky
  switchport port-security violation shutdown
  ```
- Apply the following commands to the trunk ports on the L3 switch:
  ```
  interface g1/0/2
  switchport mode trunk
  do write
  ```

## IP Addressing

**Base Network:** 192.168.10.0

### 1st Floor
| Department | Network Address | Subnet Mask          | Host Address Range      | Broadcast Address |
|------------|-----------------|----------------------|-------------------------|-------------------|
| Marketing  | 192.168.10.0     | 255.255.255.192/26   | 192.168.10.1 to 192.168.10.62  | 192.168.10.63     |
| Research   | 192.168.10.64    | 255.255.255.192/26   | 192.168.10.65 to 192.168.10.126| 192.168.10.127    |
| HR         | 192.168.10.128   | 255.255.255.192/26   | 192.168.10.129 to 192.168.10.190| 192.168.10.191    |

### 2nd Floor
| Department | Network Address | Subnet Mask          | Host Address Range      | Broadcast Address |
|------------|-----------------|----------------------|-------------------------|-------------------|
| Marketing  | 192.168.10.192  | 255.255.255.192/26   | 192.168.10.193 to 192.168.10.254| 192.168.10.255    |
| Accounts   | 192.168.11.0     | 255.255.255.192/26   | 192.168.11.1 to 192.168.11.62  | 192.168.11.63     |
| Finance    | 192.168.11.64    | 255.255.255.192/26   | 192.168.11.65 to 192.168.11.126| 192.168.11.127    |

### 3rd Floor
| Department | Network Address | Subnet Mask          | Host Address Range      | Broadcast Address |
|------------|-----------------|----------------------|-------------------------|-------------------|
| Logistics  | 192.168.11.128   | 255.255.255.192/26   | 192.168.11.127 to 192.168.11.190| 192.168.11.191    |
| Customer   | 192.168.11.192   | 255.255.255.192/26   | 192.168.11.192 to 192.168.11.254| 192.168.11.255    |
| Guest      | 192.168.12.0     | 255.255.255.192/26   | 192.168.12.1 to 192.168.12.62  | 192.168.12.63     |

### 4th Floor
| Department | Network Address | Subnet Mask          | Host Address Range      | Broadcast Address |
|------------|-----------------|----------------------|-------------------------|-------------------|
| Admin      | 192.168.12.64    | 255.255.255.192/26   | 192.168.12.65 to 192.168.12.126| 192.168.12.127    |
| ICT        | 192.168.12.128   | 255.255.255.192/26   | 192.168.12.129 to 192.168.12.190| 192.168.12.191    |
| Server-Room| 192.168.12.192   | 255.255.255.192/26   | 192.168.12.193 to 192.168.12.254| 192.168.12.255    |

### Router to L3 Switch IP Addressing
- We also need IP addresses between the routers and Layer 3 switches. We will start with 10.10.10.0/30 and move up.
  
| No. | Network Address | Subnet Mask         | Host Address Range | Broadcast Address |
|-----|-----------------|---------------------|--------------------|-------------------|
| 1   | 10.10.10.0      | 255.255.255.252     |                    |                   |
| 2   | 10.10.10.4      | 255.255.255.252     |                    |                   |
| 3   | 10.10.10.8      | 255.255.255.252     |                    |                   |
| 4   | 10.10.10.12     | 255.255.255.252     |                    |                   |
| 5   | 10.10.10.16     | 255.255.255.252     |                    |                   |
| 6   | 10.10.10.20     | 255.255.255.252     |                    |                   |
| 7   | 10.10.10.24     | 255.255.255.252     |                    |                   |
| 8   | 10.10.10.28     | 255.255.255.252     |                    |                   |
| 9   | 10.10.10.32     | 255.255.255.252     |                    |                   |
| 10  | 10.10.10.36     | 255.255.255.252     |                    |                   |
| 11  | 10.10.10.40     | 255.255.255.252     |                    |                   |
| 12  | 10.10.10.44     | 255.255.255.252     |
