# Vic Modern Hotel Network Setup

## Step-by-Step Configuration

### 1. Enable Serial Interfaces on Routers

To connect the routers using serial DCE cables, follow these steps:

1. **Turn Off the Router:**
   - Click on the router.
   - Click the circle above the green line to turn off the router.

2. **Add HWIC-2T Module:**
   - Drag and drop the “HWIC-2T” module from the left column onto the router.

3. **Turn On the Router:**
   - Click the circle above the green line to turn the router back on.

4. **Repeat for All Routers:**
   - Perform the above steps for each router.

### 2. Connect Routers Using Serial DCE Cables

1. **View Serial Interfaces:**
   - Click on the router to see the serial interfaces.

2. **Connect Routers:**
   - Use serial DCE cables to connect the routers:
     - **Router 1 to Router 2:** Use the `10.10.10.0/30` network.
     - **Router 2 to Router 3:** Use the `10.10.10.4/30` network.
     - **Router 1 to Router 3:** Use the `10.10.10.8/30` network.

**Note:** Serial interfaces provide a physical layer connection that can be used to connect routers across long distances.

### 3. Connect Switches to Routers

1. **Connect Each Floor’s Switch to Routers:**
   - **1st Floor Switch** to Router 1.
   - **2nd Floor Switch** to Router 2.
   - **3rd Floor Switch** to Router 3.

### 4. Configure Printers and End Devices

#### 1st Floor Configuration

1. **Add Printers:**
   - Place one printer for each department (Reception, Store, Logistics).

2. **Add End Devices:**
   - Add the appropriate number of end devices (PCs, laptops) for each department.

3. **Wi-Fi Configuration:**
   - Place a Wi-Fi access point on the 1st floor.
   - Connect the Wi-Fi access point to the floor’s switch.

#### 2nd Floor Configuration

1. **Add Printers:**
   - Place one printer for each department (Finance, HR, Sales).

2. **Add End Devices:**
   - Add the appropriate number of end devices (PCs, laptops) for each department.

3. **Wi-Fi Configuration:**
   - Place a Wi-Fi access point on the 2nd floor.
   - Connect the Wi-Fi access point to the floor’s switch.

#### 3rd Floor Configuration

1. **Add Printers:**
   - Place one printer for each department (Admin, IT).

2. **Add End Devices:**
   - Add the appropriate number of end devices (PCs, laptops) for each department.

3. **Wi-Fi Configuration:**
   - Place a Wi-Fi access point on the 3rd floor.
   - Connect the Wi-Fi access point to the floor’s switch.

### 5. Label Everything

- **Ensure All Devices Are Labeled Correctly:**
  - Routers, switches, end devices, printers, and Wi-Fi access points.

### Diagram

Your network diagram should now look similar to the following:

- **Router Connections:**
  - Routers are connected with serial DCE cables.
- **Floor Layouts:**
  - Switches connected to routers.
  - Printers and end devices configured in each department.
  - Wi-Fi access points connected to each floor’s switch.

<img src="https://i.imgur.com/9WMvy19.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

One thing to note is, we must enable clock rate on Serial Dce interfaces. If we hover over the red lines we see the interface name. if it has a clock beside it then we enable it on that interface only.

<img src="https://i.imgur.com/nKpDqOw.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

Before enabling the clock rate, we must also use no shutdown since routers interfaces are shutdown by default.
For R3

<img src="https://i.imgur.com/Ved0tsg.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

# Router and Switch Configuration

## R1
No config needed except turning the interfaces on.

## R2
The config is almost same.

## VLAN Setup
Now we setup VLANs. Since there are different VLANs, we will configure them using access port, but the link between router and switch will be trunk.

For respective interfaces we use `switchport mode access`, followed by `switchport access vlan “number”`.

For switch one, `f0/1` is connected to router. Hence it’s the trunk link.

Do the same for all other switches.

## Configuring IP Addresses on Routers
Now we have `/30` notations on the routers, so that means there will be 2 IP addresses.

### Example Configuration

For Router 1:
```plaintext
interface Serial0/0
ip address 10.10.10.1 255.255.255.252
no shutdown
```

<img src="https://i.imgur.com/3ZvF3rI.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>

So se0/2/0 gets 10.10.10.5.
And se0/2/1 gets 10.10.10.9 since it has the network 10.10.10.8/30

<img src="https://i.imgur.com/ni3szVG.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>

We go on router and use the command ip address 10.10.10.9 followed by subnet mask which is 255.255.255.252
Do this for all 3 routers
Once done setting the ip addresses, we can check which interface has which ip add using do s hip int br


# Setting Up Inter-VLAN Routing

To set this up, we will need to create sub-interfaces on the router and assign an IP address (default gateway) according to the given range.

## First Floor Configuration

### For VLAN 80
<img src="https://i.imgur.com/maibNyi.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>

# Setting Up Inter-VLAN Routing

The setup is exactly similar for all other routers and interfaces. I have a habit of setting up the default gateway as the first usable IP address.

## Configuration Steps

The setup for each VLAN on the routers is similar. For each VLAN, use the following commands to configure the sub-interfaces:

```plaintext
interface GigabitEthernet0/x.VLAN
encapsulation dot1Q VLAN_ID
ip address IP_ADDRESS SUBNET_MASK
no shutdown
```
<img src="https://i.imgur.com/GQ46bVO.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>

I did some pings and everything works.
<img src="https://i.imgur.com/KvsSMWP.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>

One thing to notice is we still cannot communicate with devices in other vlans. To solve this we need to enable ospf on the routers.

## Configuing OSPF
For R1 we see its connected to 5 networks.

