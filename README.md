

```markdown
# Computer Networks Practical Documentation

**Subject:** Computer Networks  
**Task:** Practical Test Submission (Practical No. 15 & Practical No. 16)  

---

# Practical 15: Configuring RIPv2 (Classless) Dynamic Routing

## 1. Objective
To configure and verify Routing Information Protocol version 2 (RIPv2) on a multi-router network topology utilizing Variable Length Subnet Masking (VLSM) subnets (`/26` and `/30`) in Cisco Packet Tracer.

---

## 2. Network Topology & Addressing Plan

| Device | Interface | IP Address | Subnet Mask | Default Gateway | Connection Type |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **R0** | Fa2/0 | 192.168.20.1 | 255.255.255.192 (`/26`) | N/A | FastEthernet |
| | Se0/0 | 192.168.20.193 | 255.255.255.252 (`/30`) | N/A | Serial DCE (Clock: 64000) |
| **R1** | Fa2/0 | 192.168.20.65 | 255.255.255.192 (`/26`) | N/A | FastEthernet |
| | Se1/0 | 192.168.20.194 | 255.255.255.252 (`/30`) | N/A | Serial DTE |
| | Se0/0 | 192.168.20.197 | 255.255.255.252 (`/30`) | N/A | Serial DCE (Clock: 64000) |
| **R2** | Fa2/0 | 192.168.20.129 | 255.255.255.192 (`/26`) | N/A | FastEthernet |
| | Se1/0 | 192.168.20.198 | 255.255.255.252 (`/30`) | N/A | Serial DTE |
| **PC0** | Fa0 | 192.168.20.2 | 255.255.255.192 (`/26`) | 192.168.20.1 | FastEthernet |
| **PC2** | Fa0 | 192.168.20.70 | 255.255.255.192 (`/26`) | 192.168.20.65 | FastEthernet |
| **PC4** | Fa0 | 192.168.20.140 | 255.255.255.192 (`/26`) | 192.168.20.129 | FastEthernet |

---

## 3. Practical 15 Network Topology Diagram
![Practical 15 Topology](./p15_topology.png)

---

## 4. Cisco IOS Configurations (RIPv2)

### Router R0 Configuration
```text
enable
configure terminal
hostname R0

interface fa2/0
 ip address 192.168.20.1 255.255.255.192
 no shutdown
 exit

interface se0/0
 ip address 192.168.20.193 255.255.255.252
 clock rate 64000
 no shutdown
 exit

router rip
 version 2
 no auto-summary
 network 192.168.20.0
 end
write memory

```

### Router R1 Configuration

```text
enable
configure terminal
hostname R1

interface fa2/0
 ip address 192.168.20.65 255.255.255.192
 no shutdown
 exit

interface se1/0
 ip address 192.168.20.194 255.255.255.252
 no shutdown
 exit

interface se0/0
 ip address 192.168.20.197 255.255.255.252
 clock rate 64000
 no shutdown
 exit

router rip
 version 2
 no auto-summary
 network 192.168.20.0
 end
write memory

```

### Router R2 Configuration

```text
enable
configure terminal
hostname R2

interface fa2/0
 ip address 192.168.20.129 255.255.255.192
 no shutdown
 exit

interface se1/0
 ip address 192.168.20.198 255.255.255.252
 no shutdown
 exit

router rip
 version 2
 no auto-summary
 network 192.168.20.0
 end
write memory

```

---

## 5. Practical 15 Verification & Results

### 5.1 Routing Table Output (`show ip route`)

The routing table verifies that RIPv2 routes are learned dynamically without automatic classful summarization, indicated by the prefix **`R`**.

```text
R0#show ip route
Gateway of last resort is not set

     192.168.20.0/24 is variably subnetted, 5 subnets, 2 masks
C       192.168.20.0/26 is directly connected, FastEthernet2/0
R       192.168.20.64/26 [120/1] via 192.168.20.194, 00:00:04, Serial0/0
R       192.168.20.128/26 [120/2] via 192.168.20.194, 00:00:04, Serial0/0
C       192.168.20.192/30 is directly connected, Serial0/0
R       192.168.20.196/30 [120/1] via 192.168.20.194, 00:00:04, Serial0/0

```

### 5.2 End-to-End Ping Test

Successful ping verification from PC0 to remote LAN subnets (`192.168.20.70` and `192.168.20.140`):

```text
C:\>ping 192.168.20.70
Pinging 192.168.20.70 with 32 bytes of data:
Reply from 192.168.20.70: bytes=32 time=1ms TTL=126
Reply from 192.168.20.70: bytes=32 time=1ms TTL=126

C:\>ping 192.168.20.140
Pinging 192.168.20.140 with 32 bytes of data:
Reply from 192.168.20.140: bytes=32 time=2ms TTL=125
Reply from 192.168.20.140: bytes=32 time=2ms TTL=125

```

---

---

# Practical 16: Configuring Single-Area OSPFv2 Routing

## 1. Objective

To transition the network protocol from RIPv2 to Single-Area OSPFv2 (Open Shortest Path First) in Area 0, configuring manual Router IDs and Wildcard Masks for classless subnets in Cisco Packet Tracer.

---

## 2. OSPF Area & Wildcard Addressing Table

| Router | Interface | Subnet Network | Subnet Mask | Wildcard Mask | OSPF Area | Router ID |
| --- | --- | --- | --- | --- | --- | --- |
| **R0** | Fa2/0 | 192.168.20.0 | 255.255.255.192 | 0.0.0.63 | Area 0 | 1.1.1.1 |
|  | Se0/0 | 192.168.20.192 | 255.255.255.252 | 0.0.0.3 | Area 0 | 1.1.1.1 |
| **R1** | Fa2/0 | 192.168.20.64 | 255.255.255.192 | 0.0.0.63 | Area 0 | 2.2.2.2 |
|  | Se1/0 | 192.168.20.192 | 255.255.255.252 | 0.0.0.3 | Area 0 | 2.2.2.2 |
|  | Se0/0 | 192.168.20.196 | 255.255.255.252 | 0.0.0.3 | Area 0 | 2.2.2.2 |
| **R2** | Fa2/0 | 192.168.20.128 | 255.255.255.192 | 0.0.0.63 | Area 0 | 3.3.3.3 |
|  | Se1/0 | 192.168.20.196 | 255.255.255.252 | 0.0.0.3 | Area 0 | 3.3.3.3 |

---

## 3. Practical 16 Network Topology Diagram

---

## 4. Cisco IOS Configurations (OSPFv2)

### Router R0 Configuration

```text
enable
configure terminal
hostname R0

no router rip
router ospf 1
 router-id 1.1.1.1
 network 192.168.20.0 0.0.0.63 area 0
 network 192.168.20.192 0.0.0.3 area 0
 end
write memory

```

### Router R1 Configuration

```text
enable
configure terminal
hostname R1

no router rip
router ospf 1
 router-id 2.2.2.2
 network 192.168.20.64 0.0.0.63 area 0
 network 192.168.20.192 0.0.0.3 area 0
 network 192.168.20.196 0.0.0.3 area 0
 end
write memory

```

### Router R2 Configuration

```text
enable
configure terminal
hostname R2

no router rip
router ospf 1
 router-id 3.3.3.3
 network 192.168.20.128 0.0.0.63 area 0
 network 192.168.20.196 0.0.0.3 area 0
 end
write memory

```

---

## 5. Practical 16 Verification & Results

### 5.1 OSPF Routing Table Output (`show ip route`)

The routing table confirms OSPF path selection with Administrative Distance **110**, marked with prefix **`O`**.

```text
R0#show ip route
Gateway of last resort is not set

     192.168.20.0/24 is variably subnetted, 5 subnets, 2 masks
C       192.168.20.0/26 is directly connected, FastEthernet2/0
O       192.168.20.64/26 [110/65] via 192.168.20.194, 00:00:20, Serial0/0
O       192.168.20.128/26 [110/129] via 192.168.20.194, 00:00:10, Serial0/0
C       192.168.20.192/30 is directly connected, Serial0/0
O       192.168.20.196/30 [110/128] via 192.168.20.194, 00:00:20, Serial0/0

```

### 5.2 Connectivity Verification (`ping`)

Verification of full end-to-end communication across OSPF Area 0 from PC0 to remote subnets:

```text
C:\>ping 192.168.20.70
Pinging 192.168.20.70 with 32 bytes of data:
Reply from 192.168.20.70: bytes=32 time=1ms TTL=126

C:\>ping 192.168.20.140
Pinging 192.168.20.140 with 32 bytes of data:
Reply from 192.168.20.140: bytes=32 time=2ms TTL=125

```

---

## Conclusion

Both Practical 15 (RIPv2 Routing Protocol) and Practical 16 (Single-Area OSPFv2 Routing Protocol) have been successfully configured, verified, and documented. Full reachability and proper link-state metric evaluation were confirmed across all subnets in Cisco Packet Tracer.

```

```
