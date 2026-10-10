<div align="center">

<img src="https://capsule-render.vercel.app/api?type=rect&amp;color=0:0b132b,60:1c2541,100:ff9f1c&amp;height=190&amp;section=header&amp;text=MAPLETECH%20NOC&amp;fontSize=58&amp;fontColor=ffffff&amp;fontAlignY=42&amp;desc=Enterprise%20Network%20Design%20and%20Configuration&amp;descSize=18&amp;descAlignY=68&amp;animation=twinkling" alt="MapleTech banner" />

<img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&amp;weight=700&amp;size=20&amp;duration=2800&amp;pause=900&amp;color=FF9F1C&amp;background=0B132B&amp;center=true&amp;vCenter=true&amp;width=760&amp;height=50&amp;lines=MapleTech%23+show+topology;4+branches+%7C+6+WAN+links+%7C+RIP+routing;Star+and+extended+star+LANs;Central+DHCP+%2B+DNS+with+IP+helper" alt="Typing SVG" />

![Branches](https://img.shields.io/badge/Branches-4-ff9f1c?style=flat-square&amp;labelColor=0b132b)
![WAN links](https://img.shields.io/badge/WAN%20links-6-ff9f1c?style=flat-square&amp;labelColor=0b132b)
![Routing](https://img.shields.io/badge/Routing-RIP-ff9f1c?style=flat-square&amp;labelColor=0b132b)
![Simulator](https://img.shields.io/badge/Simulator-Cisco%20Packet%20Tracer-1ba0d7?style=flat-square&amp;labelColor=0b132b)
![Module](https://img.shields.io/badge/APU-Introduction%20to%20Networking-8a1538?style=flat-square&amp;labelColor=0b132b)

</div>

---

## 📑 Contents

[Overview](#-overview) · [Who uses this](#-who-uses-this-design-and-why) · [WAN map](#-wan-map) · [Branches](#-branches-and-addressing) · [Services](#-services) · [Configuration](#-configuration-cheat-sheet) · [Improvements](#-what-i-would-improve) · [Team](#-team) · [References](#-references)

---

## 🧭 Overview

A group project for **Introduction to Networking** at Asia Pacific University (2023). The team designed the network for **MapleTech**, a fictional company with four Malaysian branches: **Kuala Lumpur (KL), Ipoh, Melaka and Penang**.

Every branch gets its own LAN. The branches are joined by a WAN so staff share resources and talk to each other inside the company's own network.

```text
+---------------------- PROJECT AT A GLANCE ----------------------+
| Company        : MapleTech                                      |
| Sites          : KL | Ipoh | Melaka | Penang                     |
| LAN design     : star (rooms) + extended star (larger rooms)    |
| WAN design     : full mesh, serial point-to-point, RIP          |
| Addressing     : private Class C per branch, VLSM per room      |
| Central DHCP   : Melaka    |  Central DNS : Melaka              |
| Servers        : web (Penang), email (Ipoh), IoT (KL)           |
| Built with     : Cisco Packet Tracer + Visual Paradigm          |
+------------------------------------------------------------------+
```

**Deliverables:** floor plans, logical topologies with justification, IP schemes with justification, switch and router configuration, DHCP and DNS, and working email, web and IoT servers.

---

## 🔍 Who Uses This Design and Why

| Who | What they do with it | Why it matters |
|---|---|---|
| 🛠️ **Network engineers** | Plan subnets, pick topologies, configure routing for multi-site firms | A clean addressing plan and a redundant WAN avoid painful renumbering and outages |
| 🎧 **IT support** | Lean on DHCP, DNS and consistent device naming | Central services make faults quicker to trace and fix |
| 📊 **IT managers** | Compare cost, scalability and resilience of star, extended star and mesh | The report spells out the trade-off behind each choice |
| 🛡️ **Security teams** | Review device hardening, segmentation and IoT exposure | One subnet per room limits how far a compromise can spread |
| 🎓 **Students and trainers** | Practise subnetting, DHCP relay and RIP | A complete scenario from floor plan to working services |

---

## 🌐 WAN Map

Four branch routers, each linked to every other one. If a single link fails, traffic can still reach every site.

```text
                    [ MELAKA  192.168.60.0 ]
                   /           |            \
         210.100.4.0     210.100.2.0     210.100.1.0
                 /             |              \
 [ KL  192.168.90.0 ]--210.100.6.0--[ PENANG  192.168.80.0 ]
                 \             |              /
         210.100.5.0           |        210.100.3.0
                   \           |            /
                    [ IPOH  192.168.70.0 ]
```

| Link | Network | Link | Network |
|---|---|---|---|
| Melaka to Penang | `210.100.1.0` | KL to Melaka | `210.100.4.0` |
| Melaka to Ipoh | `210.100.2.0` | KL to Ipoh | `210.100.5.0` |
| Ipoh to Penang | `210.100.3.0` | KL to Penang | `210.100.6.0` |

### Why these topologies

| Where | Topology | Reason |
|---|---|---|
| Inside rooms | ⭐ Star | Simple to manage, a failed device does not affect the rest, easy to add devices |
| Larger rooms | 🌟 Extended star | Extra switches hang off the main one, so the network grows and faults stay local |
| Between branches | 🕸️ Full mesh | Redundant paths, so one broken link does not cut a site off |

The report is honest about the costs: a star depends on its central switch, extended stars add cabling and can congest the core, and a full mesh is expensive to build.

---

## 🏢 Branches and Addressing

Each branch uses a private Class C range, split per room with variable-length masks. Private addressing keeps internal traffic off the public internet.

<details>
<summary><b>🇲🇾 KL — 192.168.90.0</b> · designed by Mahmoud Samy Aly Ibrahim Shahin</summary>

| Subnet | Network | Gateway |
|---|---|---|
| Finance | `192.168.90.0/25` | `192.168.90.1` |
| HR | `192.168.90.128/26` | `192.168.90.129` |
| Server room | `192.168.90.192/27` | `192.168.90.193` |
| CEO room | `192.168.90.224/28` | `192.168.90.225` |

</details>

<details>
<summary><b>🇲🇾 Penang — 192.168.80.0</b> · designed by Mohamed Abdurrazag Jaber Seklani</summary>

| Subnet | Network | Gateway |
|---|---|---|
| Staff office | `192.168.80.0/26` | `192.168.80.1` |
| Reception | `192.168.80.64/27` | `192.168.80.65` |
| Manager | `192.168.80.96/27` | `192.168.80.97` |
| Server room | `192.168.80.128/28` | `192.168.80.129` |

</details>

<details>
<summary><b>🇲🇾 Ipoh — 192.168.70.0</b> · designed by Shady Ehab Assad Ibrahim</summary>

| Subnet | Network | Gateway |
|---|---|---|
| Customer service office | `192.168.70.0/26` | `192.168.70.1` |
| Sales office | `192.168.70.64/27` | `192.168.70.65` |
| IT office | `192.168.70.96/28` | `192.168.70.97` |
| Server office | `/28` (see the report for the exact block) | see report |

</details>

<details>
<summary><b>🇲🇾 Melaka — 192.168.60.0</b> · designed by Mohammed Rasheed Yehia Al Haddad (leader)</summary>

| Subnet | Network | Gateway |
|---|---|---|
| Office room | `192.168.60.0/26` | `192.168.60.1` |
| Break room | `192.168.60.64/27` | `192.168.60.65` |
| Server room | `192.168.60.96/28` | `192.168.60.97` |

</details>

---

## 🖥️ Services

| | Service | Where | What it does |
|---|---|---|---|
| 🟢 | **DHCP** | Melaka server room | One server for all branches, with a pool per room (gateway, DNS, start address, mask) |
| 🟢 | **DNS** | Melaka server room | Name resolution, so users type names instead of IP addresses |
| 🟢 | **Web** | Penang server room | Hosts the MapleTech website |
| 🟢 | **Email** | Ipoh | SMTP and POP3 for the company mail domain, one account per team member |
| 🟢 | **IoT** | KL | Dashboard for cameras, smoke detectors, sensors and appliances |

**DHCP relay:** each router uses `ip helper-address` on its LAN-facing interface to forward requests to the central DHCP server.

**IoT devices in the design:** webcams, smoke detectors, an RFID reader for the server room, trip sensors, air conditioners, fans, doors and appliances.

---

## ⌨️ Configuration Cheat Sheet

Values in angle brackets are placeholders.

<details>
<summary><b>Switch: management IP, hostname, banner and passwords</b></summary>

```text
enable
configure terminal
hostname <Branch>_switch
interface vlan 1
 ip address <switch-ip> <mask>
 no shutdown
 description <room> switch
exit
ip default-gateway <gateway-ip>
banner motd $ <room name> $
line console 0
 password <console-password>
 login
exit
service password-encryption
enable secret <privilege-password>
end
copy running-config startup-config
```

</details>

<details>
<summary><b>Router: DHCP relay and RIP</b></summary>

```text
enable
configure terminal
interface <lan-interface>
 ip helper-address <dhcp-server-ip>
 no shutdown
exit
router rip
 network <connected-network>
 network <connected-network>
end
copy running-config startup-config
```

</details>

> 🔒 The report lists the lab's device passwords. They are intentionally not repeated here.

---

## 🔧 What I Would Improve

- Use `enable secret` everywhere and move management access to SSH
- Add VLANs and access control lists between departments
- Move from RIP to RIPv2 or OSPF for faster convergence and VLSM support
- Add a second DHCP and DNS server so Melaka is not a single point of failure
- Add a redundant core switch in each branch

---

## 🧰 Tools

![Packet Tracer](https://img.shields.io/badge/Cisco%20Packet%20Tracer-1ba0d7?style=for-the-badge&amp;logo=cisco&amp;logoColor=white)
![Cisco IOS](https://img.shields.io/badge/Cisco%20IOS-049fd9?style=for-the-badge&amp;logo=cisco&amp;logoColor=white)
![Visual Paradigm](https://img.shields.io/badge/Visual%20Paradigm-1c2541?style=for-the-badge&amp;logoColor=ff9f1c)
![RIP](https://img.shields.io/badge/RIP-1c2541?style=for-the-badge&amp;logoColor=ff9f1c)
![DHCP](https://img.shields.io/badge/DHCP-1c2541?style=for-the-badge&amp;logoColor=ff9f1c)
![DNS](https://img.shields.io/badge/DNS-1c2541?style=for-the-badge&amp;logoColor=ff9f1c)

---

## 👥 Team

| Member | Branch and work |
|---|---|
| **Mohammed Rasheed Yehia Al Haddad** (leader) | Melaka: floor plan, topology, addressing, configuration, DHCP and DNS |
| **Mohamed Abdurrazag Jaber Seklani** | Penang: floor plan, topology, addressing, configuration, web server |
| **Shady Ehab Assad Ibrahim** | Ipoh: floor plan, topology, addressing, configuration, email server |
| **Mahmoud Samy Aly Ibrahim Shahin** | KL: floor plan, topology, addressing, configuration, IoT server |

Each member also connected their branch router to the other branches.

---

## 📚 References

1. Techopedia (2023). *What is Star Topology?* <https://www.techopedia.com/definition/13335/star-topology>
2. Bhardwaj, R. (2021). *Extended star network topology.* Network Interview. <https://networkinterview.com/extended-star-network-topology/>
3. Keary, T. (2023). *6 best network topologies explained, pros and cons.* Comparitech. <https://www.comparitech.com/net-admin/network-topologies-advantages-disadvantages/>
4. Sunbird DCIM (2021). *Star Topology.* <https://www.sunbirddcim.com/glossary/star-topology>
5. EDUCBA (2020). *Extended Star Topology, Importance and Uses.* <https://www.educba.com/extended-star-topology/>
6. Prasanna (2023). *Star Topology Advantages and Disadvantages.* A Plus Topper. <https://www.aplustopper.com/star-topology-advantages-and-disadvantages/>
7. Mesh (2018). *What is Mesh Topology?* Computerhope. <https://www.computerhope.com/jargon/m/mesh.htm>

---

<div align="center">

### 📫 Let's Connect

**Shady** · Cyber Security student, Asia Pacific University

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&amp;logo=linkedin&amp;logoColor=white)](https://www.linkedin.com/in/shady-ehab-07666a255)
[![Email](https://img.shields.io/badge/Email-Say%20Hello-ff9f1c?style=for-the-badge&amp;logo=gmail&amp;logoColor=0b132b)](mailto:shadysoliman3322@gmail.com)

<img src="https://capsule-render.vercel.app/api?type=rect&amp;color=0:ff9f1c,40:1c2541,100:0b132b&amp;height=70&amp;section=footer" alt="footer" />

</div>
