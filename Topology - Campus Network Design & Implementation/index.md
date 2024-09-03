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


