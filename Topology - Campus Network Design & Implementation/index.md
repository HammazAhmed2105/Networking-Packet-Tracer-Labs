## Configurations

To make the connection between cloud router and main router as serial dce, we need to physically add a HWIC-2T module. Turn off router, add the module and turn it on. The serial connection should appear as below.

<img src="https://i.imgur.com/RJD4L9x.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

## Network Configurations

1. **Department Network Assignments:**
   - Each department will have its own network. The VLAN and IP address assignments are as follows:
     - **Admin Department:** VLAN 10, IP range 192.168.1.0/24
     - **HR Department:** VLAN 20, IP range 192.168.2.0/24
     - Continue assigning VLANs and IP ranges for other departments as required.

2. **Router Interfaces:**
   - The routers will use /30 networks. Refer to the network diagram for the exact network configuration.

3. **Step 1 – Activating Router Interfaces:**
   - By default, router interfaces are administratively down. To view which interfaces are down, access the router's CLI and enter the following command:
     ```
     do show ip interfaces brief
     ```
   - This command will display the status of all interfaces on the router.

<img src="https://i.imgur.com/TOchgIa.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

This router is our main campus router, int g0/0 and the other two serial interfaces are In use so we will only turn them on.

<img src="https://i.imgur.com/SLF7H34.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

Same config is to be done for all other routers.
## Network Configurations

### Step 2 – Enable Clock Rate

1. **Identify Interfaces:**
   - To determine which interfaces need the clock rate enabled, check the diagram where the two routers are connected with a red line.
   - Look for the interface that has a clock sign on it, indicating it is the DCE (Data Communications Equipment) side of the connection.

2. **Configure Clock Rate:**
   - On the router connected to the DCE end, enable the clock rate on the correct serial interface.

   Example command:
<img src="https://i.imgur.com/IsqgqqU.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

## Network Configurations

### Step 3 – Configuring VLANs

1. **VLAN Configuration:**
   - VLANs have been configured on all switches. Below are the commands used for configuring VLAN 10 as an example.

2. **Commands for VLAN 10:**

   ```bash
   Switch(config)# vlan 10
   Switch(config-vlan)# name Admin_VLAN

   Switch(config)# interface range f0/1-24
   Switch(config-if-range)# switchport mode access
   Switch(config-if-range)# switchport access vlan 10

<img src="https://i.imgur.com/LqYXKG2.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

<img src="https://i.imgur.com/zk3usIR.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

Additionally, I also renamed the vlans to their respective department. Simply type vlan followed by vlan number, and in the next command use name “name”.
We must also add each interface of the switch in main campus to the departments respective vlan.
Switch(config)# vlan 10
Switch(config-vlan)# name Admin_VLAN

Switch(config)# vlan 20
Switch(config-vlan)# name HR_VLAN

# Continue for other VLANs...


<img src="https://i.imgur.com/0UQravU.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
## Network Configurations

### Step 3 – Configuring VLANs

1. **VLAN Configuration:**
   - VLANs have been configured on all switches. Below are the commands used for configuring VLAN 10 as an example, including VLAN naming.

2. **Commands for VLAN 10:**

   ```bash
   Switch(config)# vlan 10
   Switch(config-vlan)# name Admin_VLAN

   Switch(config)# interface range f0/1-24
   Switch(config-if-range)# switchport mode access
   Switch(config-if-range)# switchport access vlan 10

<img src="https://i.imgur.com/56iuA3Y.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
Int g1/0/2 should be connected with VLAN 90, and the other int with VLAN 100.

## Trunk config
<img src="https://i.imgur.com/AZJmmOv.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

See the below image to understand which interface will be assigned ip addresses, which will have trunk config, and which will be used for inter-vlan routing.

<img src="https://i.imgur.com/K9eG2cC.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
## Network Configurations

### Configuring IP Addresses on Serial Interfaces

1. **Main Campus Routers:**
   - Configure IP addresses on the serial interfaces of the main campus routers.

2. **Serial Interface Configuration:**
   - For the serial interfaces `se0/0/1` and `se0/0/0`, use the following commands:

   ```bash
   Router(config)# interface Serial0/0/0
   Router(config-if)# ip address 10.0.0.1 255.255.255.252
   Router(config-if)# no shutdown

   Router(config)# interface Serial0/0/1
   Router(config-if)# ip address 10.0.0.5 255.255.255.252
   Router(config-if)# no shutdown

<img src="https://i.imgur.com/bXy4tR7.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

The cloud router is connected to a server on the other end so we will configure ip address for both.

## Server Config

We need to assign the ip address to server via static method.

<img src="https://i.imgur.com/64B2RUm.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

Now we configure inter-vlan routing
Branch campus router.
Commands for vlan 90 and 100. On router gig 0/0 interface

<img src="https://i.imgur.com/mWdjsKT.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

I took the first ip address of the subnet. This will be used as the default fateway on the rouer for that vlan.
We will also configure DHCP server to let the end devices choose their ip addresses.


<img src="https://i.imgur.com/tUH4c3F.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

## Configuring RIP

## Network Configurations

### Configuring RIP on Branch Campus Router

To enable communication between devices connected to the main branch router and devices in the branch network, configure RIP (Routing Information Protocol) on the branch campus router. RIP will advertise the networks connected to the branch campus router.

1. **Enable RIP Routing:**
   - Enter RIP configuration mode and specify the networks to be advertised.

2. **Commands:**

   ```bash
   Router(config)# router rip
   Router(config-router)# version 2
   Router(config-router)# network <network1>
   Router(config-router)# network <network2>
   Router(config-router)# network <network3>


<img src="https://i.imgur.com/7ZIEip5.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

Final Topology

<img src="https://i.imgur.com/4kCP71q.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
