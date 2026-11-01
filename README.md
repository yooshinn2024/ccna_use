# CCNA Subnet Master v3.0.1

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Web%20%2F%20PWA-brightgreen.svg)]()
[![Exam](https://img.shields.io/badge/Exam-Cisco%20CCNA%20200--301-049fd9.svg)]()
[![Languages](https://img.shields.io/badge/Languages-JA%20%7C%20EN%20%7C%20ZH-orange.svg)]()
[![Version](https://img.shields.io/badge/Version-3.0.1-blueviolet.svg)]()

> An enterprise-grade, zero-dependency procedural IPv4 subnetting trainer and algorithmic practice engine purpose-built for networking students and Cisco Certified Network Associate (CCNA 200-301) candidates.

---

## 🌐 Live Demo

- **Custom Domain**: [https://subnet.yooshinngotanidea.com](https://subnet.yooshinngotanidea.com)
- **GitHub Pages**: [https://yooshinn2024.github.io/ccna_use/](https://yooshinn2024.github.io/ccna_use/)

---

## 🚀 What's New in v3.0.1

Version 3.0.1 elevates **CCNA Subnet Master** from an elementary subnet calculator into a comprehensive **CCNA 200-301 Algorithmic Practice Suite**. It introduces authentic Cisco IOS CLI exhibits, real-world routing table evaluations, VLSM capacity planning, wildcard mask ACL matching, and multi-octet mental math across Class A, B, and C subnets.

### Highlights of v3.0.1
- **12 Comprehensive Networking Question Categories**: Split into **Core Speed Drills (1–6)** and **Applied Networking Scenarios (7–12)** covering routing lookup, VLSM design, ACL wildcarding, route aggregation, and Cisco IOS troubleshooting.
- **Full Prefix Scope (`/8` ~ `/30`) & Boundary Edge Cases (`/0`, `/31`, `/32`)**: Realistic weighted distribution across Class A (`/8` ~ `/15`), Class B (`/16` ~ `/23`), and Class C (`/24` ~ `/30`), alongside RFC 3021 point-to-point links (`/31`), host routes (`/32`), and default routes (`0.0.0.0/0`).
- **Interactive Cisco IOS Terminal Exhibits**: Renders authentic `show ip route` tables, Cisco IOS CLI interface configuration snippets, and `% Bad mask` diagnostic errors.
- **Dual Input Modes & Keyboard-First Navigation**: Seamlessly switch between **4-Option Multiple Choice** (with `[A]`–`[D]` / `[1]`–`[4]` shortcuts), **Flexible Text Input** (supporting `IP/CIDR` or `IP Mask` syntax), and **Mental Math Flashcard Mode**.
- **Comprehensive Multi-Octet Cheat Sheet**: Quick-reference drawer expanded to cover `/8` through `/32` with dotted-decimal masks, borrowed bits, Magic Numbers (block sizes), usable host counts, and wildcard mask bytes.

---

## 🎯 12 Comprehensive Networking Practice Categories

Questions are algorithmically generated and partitioned into two distinct preparation tracks:

### Suite 1: Core Speed Drills (1–6)
Designed to build instantaneous mental math reflex under 20 seconds:
1. **Network Address (Network ID)**: Calculate the true wire network address from arbitrary host IPs. Includes tricky `.0` and `.255` valid unicast host address boundary pitfalls on `/8` through `/23` networks.
2. **Directed Broadcast Address**: Two-step deduction to calculate the directed broadcast boundary for any given host IP.
3. **Usable Host Bounds**: Rapidly identify the First Usable Host, Last Usable Host, or DHCP `default-router` Next-to-Last Usable IP.
4. **Mask / CIDR / 32-Bit Binary Mask Conversion**: Bidirectional conversions between prefix length notation (`/XX`), dotted-decimal masks (`255.255.X.X`), and 32-bit binary representations.
5. **Usable Host ($2^H - 2$) & Subnet Count ($2^S$) Calculation**: Instantaneous host capacity and subnet multiplier calculations from prefix lengths and borrowed bits.
6. **Same Subnet Check (Layer 2 Direct Communication)**: Determine whether Host A and Host B can communicate directly at Layer 2 without crossing an intermediate router or default gateway.

### Suite 2: Applied Networking Scenarios (7–12)
Covers multi-step operational and design scenarios relevant to networking students and CCNA 200-301 candidates:
7. **Longest Prefix Match (LPM) Routing Table Lookup**: Evaluate realistic `show ip route` outputs using the strict Cisco decision hierarchy: **Longest Prefix Match (LPM) > Administrative Distance (AD) > Metric**. Tests non-matching `/31` & `/32` boundary pitfalls and `0.0.0.0/0` Gateway of Last Resort fallback.
8. **Multi-Site Subnet Design Lab (Nth Subnet & Interface IP)**: Multi-site address allocation tasks. Calculate the exact interface `IP/CIDR` or next available subnet after subdividing Class A/B/C spaces (including non-octet `/19` blocks).
9. **VLSM Capacity, Growth Buffer & Subnet Alignment**: Size multi-floor SVI subnets given specific host counts and required percentage growth buffers (e.g. $+25\%$). Conserve point-to-point WAN links and detect misaligned subnet ID boundary pitfalls.
10. **Wildcard Mask & OSPF / Extended ACL Matching**: Calculate inverse wildcard masks ($255.255.255.255 - \text{Mask}$) across `/8` to `/32`. Validate OSPF `network <net> <wildcard> area <id>` matching statements and Extended ACL Access Control Entries (ACEs).
11. **CIDR Route Summarization (Supernetting)**: Aggregate contiguous route blocks into optimal summary advertisements (`IP/CIDR`) via common binary prefix bit analysis.
12. **Cisco IOS `Bad mask` & Gateway/Subnet Mismatch Troubleshooting**: Diagnose and resolve `Bad mask /XX for address <IP>` CLI configuration errors, PC-to-gateway subnet mask mismatches, and overlapping router interface assignments.

---

## ⚡ Prefix Difficulty & Practical Networking Distribution

| Scope | Prefix Range | Focus | Practical Networking Distribution |
| :--- | :--- | :--- | :--- |
| **Class C** | `/24` ~ `/30` | 4th Octet Arithmetic | Branch LANs, point-to-point links, fast mental drills |
| **Class B** | `/16` ~ `/23` | 3rd Octet Arithmetic | Enterprise campus subnets, multi-floor SVIs, route summarization |
| **Class A** | `/8` ~ `/15` | 2nd Octet Arithmetic | Large enterprise backbones, ISP address allocations |
| **Mixed (Balanced Scope)** | `/8` ~ `/30` | Practical Networking Distribution | Balanced mix reflecting practical enterprise networking and CCNA 200-301 syllabus topics |
| **Boundary Edge Cases** | `/0`, `/31`, `/32` | Boundary Edge Cases | `/0` default routes, `/31` RFC 3021 p2p links (2 hosts, no broadcast), `/32` host routes |

---

## 🧠 The "Magic Number" Step-by-Step Breakdown

In accordance with Cisco Official Cert Guides (Wendell Odom) and Jeremy's IT Lab, the solution engine breaks down every question using the **Magic Number (Block Size)** mental math technique:

1. **Octet Boundary Check**: Identifies clean octet boundaries (`/8`, `/16`, `/24`) where no bit borrowing occurs.
2. **Interesting Octet**: Pinpoints the active working octet:
   $$\text{OctetIndex} = \lfloor \text{Prefix} / 8 \rfloor + 1$$
3. **Magic Number Calculation**: Determines the exact block increment:
   $$\text{Magic Number} = 256 - \text{Subnet Mask Octet}$$
4. **Subnet Interval Localization**: Calculates the multiples of the Magic Number to pinpoint the base Network Address, usable host span, and Directed Broadcast boundary.

---

## 🖥️ Interactive Cisco IOS Exhibits & Keyboard Workflow

- **Authentic Cisco Terminal Blocks**: Styled terminal panels rendering standard Cisco IOS command outputs (`show ip route`, `show ip interface brief`, and CLI syntax diagnostics).
- **Dual Input Methods**:
  - **Multiple Choice**: Standard 4-option questions with keyboard shortcuts `[A]`, `[B]`, `[C]`, `[D]` (or `[1]`, `[2]`, `[3]`, `[4]`).
  - **Flexible Text Input**: High-speed entry accepting both CIDR notation (`192.168.1.0/24`) and traditional IP/Mask format (`192.168.1.0 255.255.255.0`).
- **Mental Math Flashcard Mode**: Hide inputs and practice pure mental deduction with one-key answer reveals.
- **Speed Drills & Optional Timer**: Stopwatch timer is disabled by default for deliberate study, and can be toggled on for sub-20-second speed drills.
- **Zero-Jump Layout**: Viewport locking and a 350ms anti-bounce mechanism prevent layout shifts when revealing answers.

---

## 📊 Multi-Octet Reference Cheat Sheet

A collapsible in-app reference drawer provides rapid lookups across `/8` to `/32`:
- Prefix length (`/8` through `/32`)
- Dotted-decimal subnet masks
- Borrowed host/subnet bits
- Magic Numbers (block sizes)
- Usable host counts ($2^H - 2$)
- Inverted wildcard mask bytes

---

## 🌍 Tri-Lingual Support (i18n)

Switch languages instantaneously without resetting active streaks or question state:
- **Japanese (`JA` - Default)**: Aligned with standard Japanese Cisco networking and CCNA 200-301 curriculum.
- **English (`EN`)**: Cisco Global Standard networking terminology.
- **Chinese (`ZH`)**: Standard Simplified Chinese network engineering terminology.

---

## 🛠️ Technical Stack & Architecture

- **Zero-Dependency Single-Page Architecture**: Written in pure HTML5, modern CSS3, and vanilla ES6 JavaScript. No build step, no npm packages, no CDN requirements.
- **Bitwise Calculation Engine**: Powered by 32-bit unsigned bitwise operators (`>>> 0`) for guaranteed mathematical precision.
- **100% Offline & Private**: Fully functional without network connectivity. Zero telemetry, zero analytics cookies, and zero external tracking.

For complete mathematical models, routing decision algorithms, and software architecture, see [DESIGN.md](DESIGN.md).

---

## 📱 Mobile & PWA Usage

1. Open the application in **Safari (iOS)** or **Chrome (Android)**.
2. Tap the **Share** / **Menu** button and select **"Add to Home Screen"**.
3. Launch full-screen directly from your home screen for rapid daily study.

---

## ⚖️ Disclaimer

> **Independent Educational Tool**: All exercises, IP addresses, routing tables, and CLI scenarios in this project are **100% algorithmically synthesized at runtime** using standard RFC IPv4 bitwise arithmetic and public networking principles. This repository contains zero actual certification exam items or proprietary materials.
>
> This project is an independent open-source study aid and is not affiliated with, endorsed by, or sponsored by Cisco Systems, Inc. *Cisco* and *CCNA* are registered trademarks or trademarks of Cisco Systems, Inc. and/or its affiliates in the United States and certain other countries.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
