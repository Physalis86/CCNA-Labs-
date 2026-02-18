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

### 🤖 Phase 2: Automated Services (DHCP)
**Objective:** Transition from static IP assignments to a dynamic, scalable addressing model.

#### **The Implementation**
I configured local DHCP pools on the Citadel Router to serve both VLAN segments:
* **Architect Pool:** 192.168.10.0/24
* **Guest Pool:** 192.168.20.0/24

#### **🔍 Incident Report: #002 (The Gateway Conflict)**
* **Symptom:** Intermittent connectivity; duplicate IP warnings on end-user devices.
* **Discovery:** The DHCP engine attempted to lease `.1` (the router's own interface address) to a PC.
* **Resolution:** Applied `ip dhcp excluded-address` for both gateway IPs.
* **Lesson:** Always reserve static infrastructure before enabling dynamic pools.
* 
### 📁 Lab Files* [Download .pkt File](../Citadel_Full_Build.pkt) 
