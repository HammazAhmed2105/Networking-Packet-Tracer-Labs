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
