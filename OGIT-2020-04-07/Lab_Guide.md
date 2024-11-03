
### Step 1: Basic Switch Configuration

The following commands are applied to configure the basic settings on each switch (SW1, SW2, SW3).

```plaintext
conf t
no ip domain-lookup
line con 0
logging synchronous
no exec-timeout
privilege level 15
exit
spanning-tree mode rapid
spanning-tree portfast default
hostname SW1/SW2/SW3
end
```

#### Explanation:

- `no ip domain-lookup`: Disables domain lookup to prevent delays if a command is mistyped.
- `line con 0`: Access console line configuration.
- `logging synchronous`: Ensures console messages are displayed in sync with command-line input, reducing interruptions.
- `no exec-timeout`: Prevents the console session from timing out automatically.
- `privilege level 15`: Sets the highest privilege level for the console line, giving full administrative access.
- `spanning-tree mode rapid`: Configures Rapid Spanning Tree Protocol (RSTP) for faster convergence.
- `spanning-tree portfast default`: Enables PortFast on all access ports, allowing end devices to connect without delay.
- `hostname SW1/SW2/SW3`: Assigns a unique hostname to each switch for easier identification.

---

### Step 2: VLAN Trunking Protocol (VTP) Domain Configuration

This configuration is applied to ensure all switches are in the same VTP domain, enabling VLAN information sharing.

```plaintext
conf t
vtp domain OGIT
int range fa0/11, fa0/22
switchport mode trunk
end
```

#### Explanation:

- `vtp domain OGIT`: Sets the VTP domain name to "OGIT" to allow VLAN configuration updates across all switches within this domain.
- `int range fa0/11, fa0/22`: Selects specific interfaces to configure as trunk links.
- `switchport mode trunk`: Sets the selected interfaces to trunk mode, allowing them to carry traffic for all VLANs.

---

### Step 3: VLAN Configuration on SW1 and SW2

On each access switch (SW1 and SW2), VLANs 10 and 20 are created and assigned to specific ports.

```plaintext
int fa0/1
switchport mode access
switchport access vlan 10
int fa0/2
switchport mode access
switchport access vlan 20
end
```

#### Explanation:

- `switchport mode access`: Sets the port as an access port for end devices.
- `switchport access vlan 10`: Assigns VLAN 10 to the port for devices in VLAN 10 (e.g., PC1 and PC3).
- `switchport access vlan 20`: Assigns VLAN 20 to the port for devices in VLAN 20 (e.g., PC2 and PC4).

---

### Step 4: VLAN Interface Configuration on SW3 (Layer 3 Switch)

SW3 is a Layer 3 switch configured for inter-VLAN routing. Each VLAN interface (SVI) is assigned an IP address to act as the default gateway for devices within each VLAN.

```plaintext
interface vlan 10
ip address 10.16.0.1 255.255.255.0
interface vlan 20
ip address 172.16.0.1 255.255.255.0
interface vlan 777
ip address 192.168.1.1 255.255.255.0
end
```

#### Explanation:

- `interface vlan X`: Creates a VLAN interface (SVI) for VLANs 10, 20, and 777.
- `ip address X.X.X.X 255.255.255.0`: Assigns an IP address to each VLAN interface, which will act as the default gateway for devices in that VLAN.

---

### Step 5: DHCP Relay (IP Helper Address)

SW3 is configured with an IP helper address to forward DHCP requests from clients in VLANs 10 and 20 to a DHCP server in VLAN 777.

```plaintext
interface vlan 10
ip helper-address 192.168.1.100
interface vlan 20
ip helper-address 192.168.1.100
end
```

#### Explanation:

- `ip helper-address 192.168.1.100`: Configures a DHCP relay, forwarding DHCP requests from clients in VLAN 10 and VLAN 20 to the DHCP server at `192.168.1.100` (located in VLAN 777).

---

### Step 6: Enable IP Routing on SW3

Enabling IP routing on SW3 allows inter-VLAN communication by routing traffic between different VLANs.

```plaintext
ip routing
```

#### Explanation:

- `ip routing`: Activates routing on the Layer 3 switch, enabling it to route packets between VLANs.

---

### Step 7: Verifying IP Routing

To verify the routing configuration, the command below shows the routing table with directly connected networks.

```plaintext
show ip route
```

#### Explanation:

- `show ip route`: Displays the routing table, allowing verification that VLANs 10, 20, and 777 are correctly set up as directly connected routes. This ensures that inter-VLAN routing is operational.

---

### Summary

This configuration achieves the following objectives:

1. **VLAN Segmentation**: VLANs 10, 20, and 777 are created to segregate network traffic.
2. **Inter-VLAN Routing**: Configured on SW3 to allow communication between VLANs.
3. **DHCP Relay**: Set up on SW3 to forward DHCP requests to a DHCP server in VLAN 777.
4. **Trunk Links**: Configured between switches to carry VLAN traffic across the network.
5. **Network Verification**: IP routing and connectivity are confirmed using the routing table.

By following these steps, a functional network setup with VLANs and inter-VLAN routing is achieved in Cisco Packet Tracer.

---
