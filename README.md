# 🌐 FortiGate SD-WAN Dual ISP Lab

This project demonstrates how to configure a **dual-ISP SD-WAN solution** using a **FortiGate Firewall** and **Cisco routers** in a virtual lab environment such as **EVE-NG, GNS3, or Proxmox**.

---

## 📁 Project Structure

```
fortigate-sd-wan-dual-isp/
│
├── configs/           # Device configurations (FortiGate & Cisco)
├── docs/              # Detailed documentation
├── screenshots/       # GUI and verification screenshots
├── topology/          # Network topology diagrams
├── troubleshooting/   # Debugging and issue resolution notes
├── verification/      # Testing and validation outputs
└── README.md          # Project overview
```

---

## 🧠 Lab Overview

This lab simulates an enterprise network edge connected to **two ISP links** using Cisco routers acting as upstream providers:

- **ISP 1:** Echotel (Airtel)
- **ISP 2:** Cool-Ideas (BSNL)

The **FortiGate Firewall** uses **SD-WAN** to intelligently route traffic based on link performance and availability.

---

## 🖥️ Topology

> 📌 Topology diagram located in `/topology` or `/screenshots`

```
[ ISP-1 ]      [ ISP-2 ]
    |              |
    |              |
   port1         port2
      \           /
       \         /
        [ FortiGate ]
             |
            port3
             |
           [ LAN ]
```

---

## 🌍 IP Addressing

| Device | Interface | IP Address | Description |
|--------|----------|------------|-------------|
| FGT-FW | port1 | 172.168.1.2/30 | WAN 1 (Echotel) |
| FGT-FW | port2 | 172.168.2.2/30 | WAN 2 (Cool-Ideas) |
| FGT-FW | port3 | 192.168.1.1/24 | LAN Gateway |
| Echotel | Gi0/1 | 172.168.1.1/30 | ISP 1 |
| Cool-Ideas | Gi0/1 | 172.168.2.1/30 | ISP 2 |
| VPC | eth0 | 192.168.1.10/24 | Client |

---

## 🚀 Features Implemented

✅ Dual ISP connectivity  
✅ SD-WAN load balancing & failover  
✅ DHCP configuration for LAN clients  
✅ Firewall policy with NAT  
✅ Performance SLA monitoring  
✅ Automatic failover testing  

---

## 🔧 Configuration Summary

### 1. Cisco ISP Routers

```
interface Gi0/1
 ip address 172.168.x.1 255.255.255.252
 no shutdown
```

---

### 2. FortiGate Interface Setup

```
config system interface
    edit "port1"
        set ip 172.168.1.2 255.255.255.252
    next
    edit "port2"
        set ip 172.168.2.2 255.255.255.252
    next
    edit "port3"
        set ip 192.168.1.1 255.255.255.0
    next
end
```

---

### 3. SD-WAN Configuration

- Create SD-WAN zone (`virtual-wan-link`)
- Add WAN interfaces as members
- Configure gateways per ISP

---

### 4. Performance SLA

- Monitor `8.8.8.8`
- Track latency for path selection

---

### 5. Routing

- Default route via SD-WAN zone

---

### 6. Firewall Policy

```
srcintf: port3
dstintf: virtual-wan-link
action: accept
nat: enable
```

---

## 🧪 Verification & Testing

### ✅ Check SD-WAN Members

```
diagnose sys sdwan member
```

### ✅ Routing Table

```
get router info routing-table static
```

### ✅ DHCP Leases

```
diagnose ip dhcp lease-list
```

---

## 🔥 Failover Test

1. Start continuous ping from client:
```
ping 8.8.8.8
```

2. Disable ISP-1 link

3. ✅ Traffic should automatically failover to ISP-2

---

## 🛠️ Troubleshooting

Common checks:

- Verify interface IPs
- Confirm DHCP range matches subnet
- Ensure SD-WAN members are healthy
- Check firewall policy and NAT

---

## 🎯 Learning Objectives

- Understand SD-WAN fundamentals
- Configure multi-WAN on FortiGate
- Implement automatic failover
- Perform network troubleshooting

---

## 👨‍💻 Author

**Thokozane Mpanza**  
IT Support Engineer  

---

## 📄 License

This project is for educational purposes.
