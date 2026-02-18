# 🏰 Project: The Citadel Foundation
**Focus:** Inter-VLAN Routing (ROAS) & Initial Segmentation

### 🛠️ Lab Architecture
* **VLAN 10:** Architect (192.168.10.0/24)
* **VLAN 20:** Guest (192.168.20.0/24)
* **Device:** Cisco ISR Router + Catalyst Layer 2 Switch

### 🔍 Technical Incident Report: #001 (The "196" Glitch)
**Issue:** 100% packet loss between networks. 
**Discovery:** Used `show ip route`. Found the sub-interface was incorrectly addressed as `196.168.10.1`.
**Resolution:** Re-addressed to `192.168.10.1`. Connectivity restored.

### 📁 Lab Files
* [Download .pkt File](../Citadel_Full_Build.pkt) 
