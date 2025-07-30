# task-3-micrtotic-Routers-RIP


# MikroTik RIP Routing Configuration Guide

This guide covers the configuration of three MikroTik routers using both **SSH (Terminal)** and **Winbox GUI**, with **RIP v2 routing protocol** for dynamic routing between routers.

---

## 🖧 Network Topology

nginx
Copy
Edit
       INTERNET
           │
    [ether1: DHCP]
     Router 1 (R1)
     192.168.88.1/24
     192.168.1.1/24
           │
 ┌─────────┘
 ▼
Router 2 (R2)
192.168.1.2/24
192.168.2.1/24
│
▼
Router 3 (R3)
192.168.2.2/24
DNS: 8.8.8.8

csharp
Copy
Edit

---

## ⚙️ Router 1 - Gateway Router

### Terminal (SSH)
```bash
ip address add address=192.168.88.1/24 interface=ether1
ip address add address=192.168.1.1/24 interface=ether2
ip dhcp-client add interface=ether1 use-peer-dns=yes use-peer-ntp=yes
ip firewall nat add chain=srcnat out-interface=ether1 action=masquerade
routing rip interface add interface=ether2 receive=v2 send=v2
routing rip network add network=192.168.1.0/24
Winbox GUI
IP > Addresses:

192.168.88.1/24 on ether1

192.168.1.1/24 on ether2

IP > DHCP Client:

Interface: ether1, Peer DNS ✅, Peer NTP ✅

IP > Firewall > NAT:

Chain: srcnat, Out Interface: ether1, Action: masquerade

Routing > RIP > Interface: ether2

Routing > RIP > Networks: 192.168.1.0/24

🔁 Router 2 - Middle Router
Terminal (SSH)
bash
Copy
Edit
ip address add address=192.168.1.2/24 interface=ether1
ip address add address=192.168.2.1/24 interface=ether2
routing rip interface add interface=ether1 receive=v2 send=v2
routing rip interface add interface=ether2 receive=v2 send=v2
routing rip network add network=192.168.1.0/24
routing rip network add network=192.168.2.0/24
Winbox GUI
IP > Addresses:

192.168.1.2/24 on ether1

192.168.2.1/24 on ether2

Routing > RIP > Interface: ether1, ether2

Routing > RIP > Networks: 192.168.1.0/24, 192.168.2.0/24

🧑‍💻 Router 3 - Client Router
Terminal (SSH)
bash
Copy
Edit
ip address add address=192.168.2.2/24 interface=ether1
ip dns set servers=8.8.8.8 allow-remote-requests=yes
routing rip interface add interface=ether1 receive=v2 send=v2
routing rip network add network=192.168.2.0/24
Winbox GUI
IP > Addresses: 192.168.2.2/24 on ether1

IP > DNS: 8.8.8.8, enable ✅ "Allow Remote Requests"

Routing > RIP > Interface: ether1

Routing > RIP > Networks: 192.168.2.0/24

🧪 Connectivity Test (from Router 3)
bash
Copy
Edit
ping 8.8.8.8
ping google.com
