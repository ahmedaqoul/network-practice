# 🖧 Packet Tracer Lab – Basic Serial Connection Between Routers

## 📚 Description

This lab demonstrates a basic serial connection between two routers using WIC-2T modules and a DCE serial cable.  
Each router is also connected to a switch to simulate local area networks (LANs) on both sides.

The goal is to configure serial interfaces, enable communication between LANs, and verify network connectivity.

## 🛠️ Tools Used

- Cisco Packet Tracer
- Routers: 2 × 2811 (each with WIC-2T module)
- Switches: 2 × 2960-24TT


- Serial interface IPs:
  - R1 Serial0/0/0: 192.168.0.1/24 (DCE side)
  - R2 Serial0/0/0: 192.168.0.2/24 (DTE side)

## ⚙️ Configuration Steps

1. Used **CDP** commands to discover interfaces connecting routers and switches. use this command (show cdp neighbors) 
2. Identified which side of the serial cable is **DCE** and which is **DTE**. (show cdp neighbors)
3. (configure terminal)               # Enter global configuration mode
   (interface serial 0/0/0)           # Enter serial interface configuration mode
4. Set clock rate on the **DCE** side to 64000 bps:  (clock rate 64000)   


5. Configured IP addresses on serial interfaces:
Router1(config-if)# ip address 192.168.0.1 255.255.255.0
Router2(config-if)# ip address 192.168.0.2 255.255.255.0

6. Tested connectivity with:
Router1# ping 192.168.0.2

## ✅ Results

- CDP successfully discovered connected interfaces.
- DCE and DTE sides correctly identified.
- Serial link configured with correct clock rate.
- Ping tests between routers succeeded, confirming connectivity.

## 👨‍💻 Author

- Name: Ahmad Akul  
- Date: 04/08/2025  
- Course: CCNA Lab

## Credits

This lab is based on a tutorial by [Jeremy's IT Lab][https://www.youtube.com/watch?v=NkJEKGIzN7I&list=PLxbwE86jKRgMQ4HTuaJ7yQgA2BoNwY9ct&index=5]

