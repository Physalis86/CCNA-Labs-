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


## 🛡️ Phase 4: Perimeter Security (The Bouncer)
**Objective:** Enforce departmental isolation by preventing the Guest Network (VLAN 20) from accessing the Architect Network (VLAN 10).

### 📋 The Technical Blueprint
* **Technology:** Standard IPv4 ACLs (Numbered).
* **Logic:** Applied an "Implicit Deny" mitigation strategy with a `permit any` catch-all.
* **Placement:** Applied **Outbound** on the Architect Sub-interface (Gi0/0.10) to ensure traffic is only filtered at the final destination.

### 🔍 Incident Report: #004 (The Local Interface Loophole)
* **Symptom:** Guest PC was still able to ping the Architect Gateway (192.168.10.1) despite the ACL being active.
* **Discovery:** Standard ACLs applied outbound filter traffic *leaving* the interface toward the network, but do not stop traffic destined for the router's own virtual IP (the sub-interface).
* **Resolution:** Verified the ACL by pinging an actual end-host (192.168.10.2). The router successfully dropped the packet and returned "Destination host unreachable," confirming the perimeter was secure.

### 📜 Golden Config: Standard ACL
```bash
! Define the Bouncer: Block Guest Subnet, Allow Everything Else
access-list 1 deny 192.168.20.0 0.0.0.255
access-list 1 permit any

! Apply to the Architect Gateway
interface GigabitEthernet0/0.10
 ip access-group 1 out
---
## 🛡️ Phase 4.2: Extended Perimeter Control (Granular Filtering)
**Objective:** Transition from "All-or-Nothing" filtering to Protocol-Specific isolation. 

### 📋 The Technical Evolution
The "Citadel" security policy was upgraded from a Standard ACL to an **Extended ACL (101)**. This allows the network to distinguish between different types of traffic (ICMP vs. TCP), ensuring that Guests can test connectivity without accessing sensitive internal web services.

### 🔍 Incident Report: #005 (The 'WWW' Alias)
* **Symptom:** After configuring the ACL for port 80, the running configuration displayed `permit tcp any any eq www`.
* **Discovery:** Cisco IOS uses "Port Aliases." It automatically translates common port numbers (80, 443, 22) into human-readable text (www, https, ssh). 
* **Resolution:** Verified that the underlying logic remains the same; the router is simply providing a display alias for the engineer.

### 📜 Golden Config: Extended ACL
```bash
! Allow ICMP (Ping) for connectivity testing
access-list 101 permit icmp 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255

! Block HTTP (Web) traffic from Guest to Architect
access-list 101 deny tcp 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255 eq 80

! Allow all other traffic (Internet Access)
access-list 101 permit ip any any

! Apply Inbound on the Guest Interface (Efficiency: Drop bad traffic early)
interface GigabitEthernet0/0.20
 ip access-group 101 in

### 📁 Lab Files
* [Download Master Project File](../Citadel_Full_Build.pkt)
