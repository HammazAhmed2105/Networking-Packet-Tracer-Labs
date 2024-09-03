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


<img src="https://i.imgur.com/3ZvF3rI.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
