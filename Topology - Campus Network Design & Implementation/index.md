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
