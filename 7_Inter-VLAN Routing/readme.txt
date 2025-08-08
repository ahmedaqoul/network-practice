---

## 🧪 Lab 7 – Inter-VLAN Routing with Trunk Links

### 🖥️ Topology

* **2 Switches**
* **4 PCs**
* **1 Router (R1)**

---

### ✅ Objective

1. Test initial connectivity between PCs.
2. Assign VLANs to PCs.
3. Create a trunk link between switches.
4. Configure inter-VLAN routing using subinterfaces on R1.
5. Verify communication between PCs in different VLANs.

---

### 🧩 Step-by-Step Instructions

#### 1️⃣ Initial Ping Test

* Ping between the PCs before configuration.
* Only PCs on the same switch and same default VLAN may succeed in pinging each other.

---

#### 2️⃣ Assign VLANs to PCs

**🔧 Switch 1 (SW1):**

```bash
Switch> enable
Switch# configure terminal
Switch(config)# interface f0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 13

Switch(config)# interface f0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 24
```

**🔧 Switch 2 (SW2):**

```bash
Switch> enable
Switch# configure terminal
Switch(config)# interface f0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 13

Switch(config)# interface f0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 24
```

---

#### 3️⃣ Create a Trunk Link Between Switches

Before trunking, pinging between PCs on different switches will fail.

**On SW1:**

```bash
Switch(config)# interface g0/2
Switch(config-if)# switchport mode trunk
```

**On SW2:**

```bash
Switch(config)# interface g0/1
Switch(config-if)# switchport mode trunk
```

Now, **ping between PC1 and PC3** should succeed.
**Ping between PC1 and PC4** will still fail because routing is not configured yet.

---

#### 4️⃣ Configure Inter-VLAN Routing on R1

Enable the router interface:

```bash
Router> enable
Router# configure terminal
Router(config)# interface g0/0
Router(config-if)# no shutdown
```

Create subinterfaces:

```bash
Router(config)# interface g0/0.13
Router(config-subif)# encapsulation dot1Q 13
Router(config-subif)# ip address 10.0.0.1 255.255.255.128

Router(config)# interface g0/0.24
Router(config-subif)# encapsulation dot1Q 24
Router(config-subif)# ip address 10.0.0.129 255.255.255.128
```

Now configure **SW1’s interface connected to the router as a trunk**:

```bash
Switch(config)# interface g0/1
Switch(config-if)# switchport mode trunk
```

---

#### 5️⃣ Final Ping Test

After completing the above steps:

* ✅ **PC1 ↔ PC3** (VLAN 13): should succeed
* ✅ **PC2 ↔ PC4** (VLAN 24): should succeed
* ✅ Inter-VLAN communication through the router is now active

---

### 📂 Files Included

* `lab7.pkt` – Packet Tracer file
* `README.md` – This documentation

this lab from [Jeremy's IT Lab]
 
---
