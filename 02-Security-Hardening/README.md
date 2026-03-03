# 🔐 Project: Operation Secure Shell & Edge Defense
**Status:** Hardened Baseline Established (Integrated in Master Build)

### 🛠️ Management Plane Hardening
To prevent credential sniffing and unauthorized remote access:
* **Protocol:** Transitioned from Telnet to **SSH v2**.
* **Encryption:** Generated **2048-bit RSA keys** for secure handshakes.
* **Access Control:** Restricted VTY lines using `transport input ssh`.

### 🛡️ Layer 2 Security: Port Security
Implemented hardware-level lockdown on all access ports:
* **Mechanism:** **Sticky MAC** learning enabled.
* **Violation Policy:** **Shutdown** (Configured to disable the port immediately upon unauthorized device detection).

### 🔍 Incident Report: #003 (The Syntax Ghost)
* **Symptom:** SSH connection refused via Command Prompt.
* **Discovery:** Identified a character confusion between the number `1` and lowercase `l` in the login string.
* **Resolution:** Corrected command to `ssh -l architect [IP]`.


---
## 🛡️ Phase 4.3: Enterprise Perimeter Refactor (Named ACLs)
**Status:** Production-Ready | **Standard:** Least Privilege Access

In this phase, the **Citadel** security posture was audited and refactored from a "Student-Level" standard to an **Enterprise Production** standard. 

### 📋 Architectural Improvements
* **Self-Documenting Logic:** Transitioned to **Named Extended ACLs** (`ACL-GUEST-INBOUND`). This allows for easier auditing and modification by multiple engineers.
* **Granular Protocol Control:** Configured specific filtering to block **TCP/80 (HTTP)** and **TCP/443 (HTTPS)** while maintaining **ICMP (Ping)** for network diagnostics.
* **Edge Placement Strategy:** ACL is applied **Inbound** on the Guest Gateway sub-interface. This adheres to the best practice of dropping unauthorized traffic at the source, preserving Router CPU cycles.

### 📜 The "Golden Config" (HQ-CORE-RTR-01)
```bash
! Define the Professional Security Policy
ip access-list extended ACL-GUEST-INBOUND
 remark -- [SEC-01] Permit ICMP for Internal Diagnostics --
 permit icmp 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
 remark -- [SEC-02] Deny Web Access (HTTP/HTTPS) to Architect VLAN --
 deny tcp 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255 eq 80
 deny tcp 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255 eq 443
 remark -- [SEC-03] Permit Outbound Internet Traffic --
 permit ip 192.168.20.0 0.0.0.255 any
!
! Interface Application (Edge Placement)
interface GigabitEthernet0/0/0.20
 description *** GUEST_VLAN_GATEWAY ***
 ip access-group ACL-GUEST-INBOUND in
