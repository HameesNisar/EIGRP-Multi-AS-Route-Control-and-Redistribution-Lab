# 🧠 EIGRP Multi-AS Route Control and Redistribution Lab

## 📘 Overview
This GNS3 lab demonstrates advanced **EIGRP multi-domain routing control** using a large-scale network topology containing **multiple EIGRP Autonomous Systems (ASNs)** and **RIP integration**.  
The configuration showcases how to securely interconnect multiple routing domains while maintaining **stability, scalability, and control** over route propagation.

---

## 📸 Topology Screenshot
<img width="1861" height="895" alt="topology" src="https://github.com/user-attachments/assets/c92915ee-492e-4e0e-a0b4-ef5bc494b7ef" />

---

## 🎯 Objectives

### **1. EIGRP Multi-AS Communication**
- Implemented several EIGRP autonomous systems (e.g., `101`, `102`, `103`, `108`, `123`, `125`, `134`, `154`, `155`, `167`, `189`, `190`, `191`, `192`, etc.).
- Established route redistribution between EIGRP ASNs through **border routers**.

### **2. RIP–EIGRP Redistribution**
- Integrated **RIP** with selected EIGRP domains.
- Used **metric translation** to maintain compatibility between distance-vector (RIP) and hybrid (EIGRP) protocols.

### **3. Authentication**
- Configured both **plain-text** and **MD5 authentication** for EIGRP on selected routers to secure neighbor adjacencies.
- Authentication was primarily applied at the **switch level** in the topology.

### **4. Route Summarization**
- Performed **manual route summarization** on key routers to reduce routing table size and improve convergence efficiency.

### **5. Route Filtering (Major Objective)**
- Implemented **Access Control Lists (ACLs)** combined with **route-maps** to filter unwanted or overlapping routes between ASNs.
- Prevented specific loopback or subnet routes from being advertised into particular autonomous systems.

**Example:**  
On **Router 5 (ASN-154)**, routes like `132.192.0.0/16` and `154.18.24.0/24` were denied using `access-list 1` and applied through `route-map box1` during redistribution with ASN-108.  
This ensured certain routes never reached the restricted domain.

---

## 🧩 Network Features Demonstrated

| **Feature** | **Description** |
|--------------|----------------|
| **EIGRP Multi-AS Redistribution** | Inter-AS route exchange using controlled redistribution. |
| **EIGRP–RIP Interoperation** | Redistribution with metric tuning for compatibility. |
| **Route Filtering via ACLs** | Blocking specific prefixes to prevent routing loops and policy violations. |
| **Authentication** | Plain-text and MD5 neighbor authentication for EIGRP sessions. |
| **Summarization** | Manual summarization at boundary routers to improve scalability. |

---

## 🔍 Key Example: Router R5 (ASN-154)

```
access-list 1 deny 132.192.0.0 0.0.255.255
access-list 1 deny 154.18.24.0 0.0.0.255
access-list 1 deny 192.168.16.32 0.0.0.0
access-list 1 permit any

route-map box1
 match ip address 1

router eigrp 154
 redistribute eigrp 108 route-map box1
router eigrp 108
 redistribute eigrp 154
```

### 🔎 Explanation
This configuration allows redistribution between **EIGRP ASNs 108 and 154** but filters out specific subnets using **ACL 1**.  
The filtered prefixes are those **not permitted to reach AS-154** as per the design policy.

---

## 🗺️ Topology Summary
- The network consists of multiple **EIGRP domains**, each with its own loopbacks (representing internal subnets).
- **Red-labeled loopbacks** are *restricted networks* — routes that must **not** propagate into certain ASNs.
- **RIP-based routers** (bottom and upper-right sections) exchange selected routes with neighboring EIGRP systems under **controlled redistribution** rules.
- **Core routers** (e.g., **R4, R5, R15, R16, R27**) act as **inter-AS border routers** where route filtering and redistribution policies are applied.

---

## 🧮 Skills Demonstrated
- Advanced **EIGRP configuration and optimization**
- **Multi-AS redistribution** and route policy design
- **EIGRP–RIP integration**
- **Route filtering** using ACLs and route-maps
- **Routing loop prevention**
- **Summarization** and **authentication** techniques

---

## 🧰 Tools Used
- **GNS3 Network Simulator**
- **Cisco IOSv Routers**
- **Switches** (for authentication configuration)
- **Loopback interfaces** for testing and route advertisement

---

## 💡 Key Learning Outcome
This lab shows how to maintain **control and security** in large EIGRP-based enterprise networks by combining:

- **Access-control route filtering**
- **Controlled redistribution policies**
- **Summarization**
- **Authentication**

Together, these techniques prevent **routing loops**, **unauthorized route propagation**, and **unnecessary routing overhead** across interconnected autonomous systems.
