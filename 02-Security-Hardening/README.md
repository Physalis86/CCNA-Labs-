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

### 📁 Lab Files
* [Download Master Project File](../Citadel_Full_Build.pkt)
