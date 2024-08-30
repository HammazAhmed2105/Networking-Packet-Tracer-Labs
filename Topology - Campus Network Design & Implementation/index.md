
# Network Topology Explanation

## 1. Serial DCE Connection Setup
**Objective**: Establish a connection between the cloud router and the main router using a serial DCE.

**Steps**:
1. Physically add a HWIC-2T module to the router.
2. Turn off the router, insert the module, and turn it back on.
3. The serial connection should now appear as expected.

## 2. Network Configuration
**Objective**: Assign each department its own network and configure the routers accordingly.

**VLAN Assignment**:
- **Admin**: VLAN 10, Network: `192.168.1.0/24`
- **HR**: VLAN 20, Network: `192.168.2.0/24`
- Continue similarly for other departments.

**Router Configuration**:
- Use `/30` subnet masks for router-to-router connections.
- Refer to the network diagram for exact network assignments.

## 3. Router Interface Activation
**Objective**: Enable the router interfaces as they are administratively down by default.

**Steps**:
1. Check which interfaces are down using the command: `do show ip interface brief`.
2. Enable the interfaces on the main campus router (`int g0/0` and serial interfaces) and other routers.

## 4. Clock Rate Configuration on Serial DCE Cables
**Objective**: Set the correct clock rate for serial DCE connections.

**Steps**:
1. Identify the correct interface by hovering over the red line connecting the routers (look for the clock symbol).
2. Configure the clock rate using the commands:
    ```bash
    int serial <interface number>
    clock rate 64000
    ```

## 5. VLAN Configuration
**Objective**: Configure VLANs across the network.

**Steps**:
1. Example configuration for VLAN 10:
    ```bash
    interface range f0/1-24
    switchport mode access
    switchport access vlan 10
    ```
2. Rename VLANs to their respective departments:
    ```bash
    vlan <VLAN number>
    name <department name>
    ```
3. Assign switch interfaces to their respective VLANs:
    ```bash
    int f0/2
    switchport mode access
    switchport access vlan 10
    ```
4. On the Layer 3 switch, configure VLANs as needed:
    ```bash
    int g1/0/2
    switchport mode access
    switchport access vlan 90
    ```

## 6. Trunk Configuration
**Objective**: Configure trunk ports for VLANs.

**Steps**:
1. Set up trunking on relevant interfaces.
2. Repeat the configuration on other routers.

## 7. IP Address Configuration on Serial Interfaces
**Objective**: Assign IP addresses to the serial interfaces of routers.

**Steps**:
1. Configure IP addresses on the main campus router’s serial interfaces (`se0/0/1` and `se0/0/0`).
2. The remaining interfaces will be used for inter-VLAN routing.

## 8. Server Configuration
**Objective**: Assign IP addresses to the server via a static method.

## 9. Inter-VLAN Routing Configuration
**Objective**: Enable inter-VLAN routing on branch campus routers.

**Steps**:
1. Configure VLANs 90 and 100 on the router’s `gig 0/0` interface.
2. Assign the first IP address of the subnet as the default gateway for the VLAN.
3. Set up a DHCP server for automatic IP assignment to end devices.
4. Repeat the configuration for other branch routers.

## 10. RIP Configuration
**Objective**: Enable communication between devices connected to different routers.

**Steps**:
1. On the branch campus router, configure RIP for the 3 connected networks:
    ```bash
    router rip
    version 2
    network <network>
    ```
2. For the main campus router, configure RIP for the connected networks, following the same process.
