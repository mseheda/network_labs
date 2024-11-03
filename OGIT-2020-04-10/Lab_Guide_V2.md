## Step 1: Basic Switch Configuration

```plaintext
conf t
spann mode rapid
spann portfast default
no ip domain-lookup
line con 0
logging synchronous
no exec-timeout
privilege level 15
exit
hostname {sw1/sw2}
end
```

#### Explanation:

- **Spanning Tree Configuration**: `spanning-tree mode rapid` and `spanning-tree portfast default` set up Rapid Spanning Tree Protocol and enable PortFast on all access ports.
- **General Switch Configuration**:
  - `no ip domain-lookup`: Prevents delays caused by DNS lookups on mistyped commands.
  - `logging synchronous`: Synchronizes logging messages with command-line input.
  - `no exec-timeout`: Disables automatic session timeout.
  - `privilege level 15`: Grants full administrative access.

---

## Step 2: Router Basic Configuration

```plaintext
conf t
no ip domain-lookup
line con 0
logging synchronous
no exec-timeout
privilege level 15
exit
hostname {R1}
end
```

#### Explanation:

- The router (R1) is configured similarly to the switches to ensure stable management sessions and administrative access.

---

## Step 3: VLAN and Trunk Configuration on Switches

```plaintext
conf t
int range fa 0/11, fa 0/22
switchport mode trunk
end

conf t
vtp domain OGIT
vlan 50
vlan 60
vlan 70
end
wr
```

#### Explanation:

- **Trunk Ports**: Sets the specified interfaces to trunk mode, allowing VLAN traffic to pass between switches.
- **VTP and VLAN Setup**:
  - `vtp domain OGIT`: Sets the VTP domain to OGIT for VLAN consistency across switches.
  - `vlan 50`, `vlan 60`, `vlan 70`: Creates VLANs 50, 60, and 70 as per lab requirements.
  - `wr`: Saves the configuration.

---

## Step 4: Router Interface and Subinterface Configuration

```plaintext
R1#conf t
int gig 0/0
no shutdown

int gig 0/0.50
encapsulation dot1Q 50
ip address 10.67.83.1 255.255.255.240

int gig 0/0.60
encapsulation dot1Q 60
ip address 10.67.83.17 255.255.255.240

int gig 0/0.70
encapsulation dot1Q 70
ip address 10.67.83.33 255.255.255.240
end
```

#### Explanation:

- **Subinterfaces for VLANs**:
  - `int gig 0/0.50`: Creates a subinterface for VLAN 50, assigns an IP (`10.67.83.1`), and tags the traffic with VLAN 50.
  - `int gig 0/0.60`: Configures the subinterface for VLAN 60 with IP `10.67.83.17`.
  - `int gig 0/0.70`: Configures the subinterface for VLAN 70 with IP `10.67.83.33`.
- Each subinterface acts as the default gateway for its respective VLAN.

---

## Step 5: Configuring DHCP Relay on Router

```plaintext
int range gig0/0.50, gig0/0.60
ip helper-address 10.67.83.46
end
```

#### Explanation:

- **DHCP Relay**: `ip helper-address 10.67.83.46` forwards DHCP requests from VLANs 50 and 60 to the DHCP server in VLAN 70.

---

## Step 6: Switch Access and Trunk Port Configuration

```plaintext
SW1#conf t
int fa 0/1
switchport mode access
switchport access vlan 50
int fa 0/2
switchport mode access
switchport access vlan 60
int gig 0/1
switchport mode trunk
end
```

#### Explanation:

- **Access Ports**: Assigns VLANs to specific ports for connecting end devices.
  - `switchport access vlan 50`: Assigns VLAN 50 to `fa0/1`.
  - `switchport access vlan 60`: Assigns VLAN 60 to `fa0/2`.
- **Trunk Port**: `switchport mode trunk` on `gig0/1` enables it to carry traffic for multiple VLANs.

---

## Step 7: Port Channel and EtherChannel Configuration on SW2

```plaintext
int range fa 0/11, fa 0/22
shutdown
channel-group 1 mode active
no shutdown
end
```

#### Explanation:

- **EtherChannel (Port-Channel)**: Groups ports (`fa0/11` and `fa0/22`) into a single logical interface, enhancing bandwidth and redundancy.
- **Channel-Group Mode Active**: Enables LACP for dynamic link aggregation.

---

## Step 8: Verification Commands

1. **Show EtherChannel Summary**:
   ```plaintext
   show etherchannel summary
   ```
   - Verifies the status and member ports of the EtherChannel.

2. **Show Interface Trunk**:
   ```plaintext
   show interface trunk
   ```
   - Checks which VLANs are allowed and active on trunk links.

3. **Show Spanning-Tree VLAN**:
   ```plaintext
   show spanning-tree vlan 50
   show spanning-tree vlan 60
   ```
   - Verifies Spanning Tree Protocol (STP) status and active ports for each VLAN.

---

### Summary

This configuration achieves the following:

- **VLAN Segmentation**: VLANs 50, 60, and 70 are created for different subnet groups.
- **Inter-VLAN Routing**: Router subinterfaces are set up for communication between VLANs.
- **DHCP Relay**: DHCP requests are forwarded to the DHCP server in VLAN 70.
- **Port Channel**: Configures EtherChannel on SW2 for link aggregation.
- **Verification**: Ensures that VLANs are functioning correctly without STP blocking.

This configuration sets up a robust network with VLANs, DHCP, and routing in Cisco Packet Tracer.

---
