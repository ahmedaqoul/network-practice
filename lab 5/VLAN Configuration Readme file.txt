# 🔧 VLAN Configuration Lab - Packet Tracer

## 🧠 Objective
Configure VLANs between two switches and test connectivity between 4 PCs connected to those switches.

---

## 🪜 Student Tasks

```bash
# Step 1: Test connectivity from each PC (done via PC command prompt)
# Example:
ping PC2_IP
ping PC3_IP
ping PC4_IP

# Step 2: Create VLANs and assign ports (on SW1)
enable
configure terminal
vlan 1
name VLAN1
exit
vlan 2
name VLAN2
exit
interface fa0/1
switchport mode access
switchport access vlan 1
exit
interface fa0/2
switchport mode access
switchport access vlan 2
exit

# Step 2 continued: VLANs on SW2
enable
configure terminal
vlan 1
name VLAN1
exit
vlan 2
name VLAN2
exit
interface fa0/1
switchport mode access
switchport access vlan 1
exit
interface fa0/2
switchport mode access
switchport access vlan 2
exit

# Step 3: Ping between same VLAN PCs
# PC1 → PC3 (should succeed)
# PC2 → PC4 (should fail for now)
ping PC3_IP
ping PC4_IP

# Step 4: Configure trunk between SW1 and SW2 (assume Fa0/24 used)
interface fa0/24
switchport mode trunk
exit

# Repeat trunk config on both switches!

# Step 5: Test connectivity again
# PC1 → PC3 ✅
# PC2 → PC4 ✅
# PC1 → PC2 ❌ (different VLANs)
ping PC3_IP
ping PC4_IP
ping PC2_IP
