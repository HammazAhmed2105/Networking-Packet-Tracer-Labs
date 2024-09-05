To start off we are told HQ and branch should have 1 core router each, each core router connecting to two L3 switches, which in turn connects to Several Access Layer switches. The core routers should also be connected to 2 ISPs. Theres a server site too, which will be connecting to the HQ core router directly. I amde a smple network using draw.io to understand what our final topology might look like. 

## Diagram from Draw.io

<img src="https://i.imgur.com/QWCF2Wk.png" height="85%" width="85%" alt="Disk Sanitization Steps"/>

- Before doing any sort of configurations we will first make the whole topology in cisco packet tracer.

- The core routers are to be connected with serial dce connection. But when you click on the router, you wont see any serial itnerfaces, as shown below.

<img src="https://i.imgur.com/XFgF3H7.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- In order to turn on the serial interfaces, we must click on the router, turn it off, drag and drop the HWIC module to add it to the router, and turn the router back on. Use this method on all the routers.

<img src="https://i.imgur.com/A0tuUuH.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- As shown below, we can see the serial interfaces now.
<img src="https://i.imgur.com/rMFpC5f.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>

- Our router connections will look like below.

<img src="https://i.imgur.com/TPa41hK.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>
