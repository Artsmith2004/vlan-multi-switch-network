# VLAN-Based Multi-Switch Network Configuration

Designed and configured a multi-switch network in **Cisco Packet Tracer** using VLANs, access ports, trunk links, IP addressing and connectivity testing between network devices.

## Tools & Concepts

| Item | Details |
|---|---|
| **Simulator** | Cisco Packet Tracer 9.0.0 |
| **Switches** | 3 × Cisco Catalyst 2960-24TT |
| **End devices** | 14 × PC-PT (PC0 – PC13) |
| **Concepts** | VLANs, access ports, 802.1Q trunk links, IP addressing, ping testing |

## Network Topology

Three switches are connected in a chain through trunk links (dashed lines). Each switch has end devices connected to its access ports.

![Network topology](images/01-network-topology.png)

| Switch | Connected PCs | Access ports | Trunk port(s) |
|---|---|---|---|
| Switch 1 | PC0 – PC5 | Fa0/1 – Fa0/6 | Fa0/7 → Switch 2 |
| Switch 2 | PC6 – PC9 | Fa0/1 – Fa0/4 | Fa0/5 → Switch 1, Fa0/6 → Switch 3 |
| Switch 3 | PC10 – PC13 | Fa0/1 – Fa0/4 | Fa0/5 → Switch 2 |

## VLAN & IP Plan

> Update this table with the VLAN IDs, names and IP ranges you used.

| VLAN ID | Name | Subnet | PCs |
|---|---|---|---|
| 10 | VLAN10 | 192.168.10.0/24 | _e.g. PC0, PC1, PC6, PC10_ |
| 20 | VLAN20 | 192.168.20.0/24 | _e.g. PC2, PC3, PC7, PC11_ |
| 30 | VLAN30 | 192.168.30.0/24 | _e.g. PC4, PC5, PC8, PC9_ |

## Configuration

> Replace the VLAN IDs and port numbers with your own values.

### 1. Create VLANs (on every switch)

```
Switch> enable
Switch# configure terminal
Switch(config)# vlan 10
Switch(config-vlan)# name VLAN10
Switch(config-vlan)# exit
Switch(config)# vlan 20
Switch(config-vlan)# name VLAN20
Switch(config-vlan)# exit
```

### 2. Configure access ports

```
Switch(config)# interface fastEthernet 0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# exit
```

Repeat for each PC-facing port with its VLAN.

### 3. Configure trunk links (switch-to-switch ports)

```
Switch(config)# interface fastEthernet 0/7
Switch(config-if)# switchport mode trunk
Switch(config-if)# exit
```

### 4. Assign IP addresses to the PCs

For each PC: **Desktop → IP Configuration**, then set the IP address and subnet mask from the VLAN plan above.

## Verification

Useful commands to verify the setup:

```
show vlan brief
show interfaces trunk
show running-config
```

Connectivity tests:

- Ping between PCs in the **same VLAN**, even when they are on different switches. This should succeed through the trunk links.
- Ping between PCs in **different VLANs**. This should fail without a Layer 3 device, which confirms VLAN isolation.

## Key Learnings

- Creating and assigning VLANs on multiple switches
- Difference between access ports and trunk ports
- How trunk links carry multiple VLANs between switches
- Network segmentation and traffic isolation using VLANs
- Testing connectivity with ping

## Future Improvement

- Add a router (router-on-a-stick) or a Layer 3 switch to enable **Inter-VLAN Routing**

## Files

- `ccna.pkt`: the Packet Tracer project file (add it to the repo if you want others to open the topology)

## Author

**Your Name**
