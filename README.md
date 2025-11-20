# UFW Firewall Filter Demonstration (Kali Linux)

**Project Description:**  
This exercise demonstrates the core principle of a host-based firewall: filtering traffic based on port configuration. We use the **Uncomplicated Firewall (UFW)** on Kali Linux to simulate a network attack and defense. The key takeaway is understanding the **"Silent Drop"** action.

---

**Setup and Initialization:**  
We use a three-terminal setup for this test:  
- **Terminal 1:** UFW Controller (Managing Rules)  
- **Terminal 2:** Listener (The simulated server on port 23)  
- **Terminal 3:** Client (The tester attempting connection)  

**Core Configuration (Terminal 1):**  
These commands establish a secure, restrictive baseline for the host:  
`sudo apt update && sudo apt install ufw nmap -y`  
`sudo ufw default deny incoming`  
`sudo ufw default allow outgoing`  
`sudo ufw allow 22/tcp`  
`sudo ufw enable`  

**Initial Firewall State Verification:**  
Confirm the firewall is active and only allows SSH:  
`sudo ufw status numbered`  

---

**Core Exercise: Blocking the Port**  
We use `ncat` to demonstrate that **port 23 (Telnet) connectivity** can be blocked via the firewall.  

**Test 1: Baseline (Unblocked Success):**  
- **Terminal 2 (Listener):** Start the server on port 23: `ncat -l -p 23`  
- **Terminal 3 (Client):** Attempt connection: `ncat 127.0.0.1 23`  

*Result:* Connection succeeds immediately. You can type text between the two terminals. ✅  

**Enforce the Block Rule:**  
- **Terminal 1 (UFW Controller):** `sudo ufw deny 23/tcp`  

**Test 2: Blocked State Confirmation (Silent Drop):**  
- **Terminal 2 (Listener):** Restart listener: `ncat -l -p 23`  
- **Terminal 3 (Client):** Attempt connection: `ncat 127.0.0.1 23`  

*Result:* The command hangs silently. The firewall is actively blocking the port. ✅  

---

**Conclusion and Restoration:**  

**Understanding the "Silent Drop":**  
The blank, hanging client screen in Test 2 proves the UFW filter in action. The **`deny`** action tells the Linux kernel to **DROP** incoming connection packets. DROP means no response is sent back to the client, not even "connection refused". The client eventually times out, confirming the block.  

**Cleanup and State Restoration:**  
- Get Rule Number (Terminal 1): `sudo ufw status numbered`  
- Delete the Rule (Terminal 1): `sudo ufw delete [NUMBER]` *(replace `[NUMBER]` with the DENY 23/tcp rule number)*  
- Final Test (Terminal 3): `ncat 127.0.0.1 23`  

*Result:* Connection succeeds immediately, proving the original state has been restored. ✅  

---

**Notes:**  
- To disable UFW completely (testing only): `sudo ufw disable`  
- Always test firewall rules in a **safe environment** to avoid disrupting critical services.  

*Paste images of firewall status and connection results in the appropriate places to document outputs visually.*



