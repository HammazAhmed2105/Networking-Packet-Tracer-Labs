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

<img src="https://i.imgur.com/qU3iKMj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
