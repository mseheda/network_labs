
# Tracing and Shutting Down Malicious MAC Addresses

### Malicious MAC Addresses to Trace:
- `0006.2a55.34de`
- `0000.0c07.be8e`
- `0001.9666.3d1b`

### Restrictions:
- **Do not access PCs (PC1 through PC10) directly.**
- **Do not log into switches randomly**; trace the path systematically.
- **Use the `show mac address-table` command** to identify the next switch in the path.
- **Shut down ports** on switches when a neighbor with the malicious MAC address is no longer found.

---

## Step-by-Step Guide

### Step 1: Start by Viewing the MAC Address Table on Switch0

1. **On `Switch0`**, enter the following command to display the MAC address table:
   ```plaintext
   show mac address-table
   ```

   This will display a list of MAC addresses associated with each port on `Switch0`.

2. **Identify Ports with Malicious MAC Addresses**:
   - Look for each malicious MAC address (listed above) in the output.
   - Note down the ports where each MAC address is found.

---

### Step 2: Trace the Path to the Next Switch

1. Use the `show cdp neighbors` command on `Switch0` to identify neighboring switches and the interfaces that connect to them:
   ```plaintext
   show cdp neighbors
   ```

2. Based on the MAC address table and CDP output, determine which neighboring switch to access next. Each MAC address will be associated with a specific port that leads to another switch.

---

### Step 3: Access the Next Switch and Repeat

1. Log into the neighboring switch (e.g., `Switch1`, `Switch2`, or `Switch3` based on your CDP output).

2. Run `show mac address-table` on each switch to locate the malicious MAC addresses.

3. Continue tracing each MAC address through the network by identifying the next switch in the path.

---

### Step 4: Shut Down Ports with Malicious MAC Addresses

1. If you encounter a switch where a port with the malicious MAC address does not have a neighboring switch connected (no CDP neighbor on that port), you should shut down the port.

2. To shut down the port:
   ```plaintext
   configure terminal
   interface Fa0/X  # Replace X with the port number
   shutdown
   end
   ```

3. Repeat this process until you have located and shut down all ports with malicious MAC addresses.

---

### Verification

1. After shutting down the ports, verify the MAC address table again to ensure the malicious addresses are no longer accessible on the network.
   ```plaintext
   show mac address-table
   ```

2. Confirm that all required ports have been shut down by using:
   ```plaintext
   show interfaces status
   ```

---

## Summary

By following this method, you can systematically trace and disable the ports associated with malicious MAC addresses on your network, ensuring that these addresses no longer have access to your network.

This guide allows you to secure the network without directly accessing the PCs or randomly logging into switches, adhering to the lab’s restrictions.

---
