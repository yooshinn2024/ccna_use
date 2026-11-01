# CCNA Subnet Master v3.0.0 - System Architecture & Engineering Specification

## 1. Vision & Exam Engineering Objectives

**CCNA Subnet Master** is a high-speed IPv4 subnetting drill engine and procedural exam simulation environment engineered specifically for Cisco Certified Network Associate (CCNA 200-301) candidates.

### 1.1 The CCNA 200-301 Time-Constraint Challenge
* **Exam Reality**: The CCNA 200-301 exam presents approximately 100 questions within a 120-minute window, providing an average of ~72 seconds per item.
* **Subnetting as a Foundational Skill**: Subnet calculations are rarely tested in isolation; they are embedded prerequisites for:
  - Routing table evaluations (`show ip route` Longest Prefix Match lookups).
  - Multi-floor VLSM address assignments and router interface configurations.
  - Access Control List (ACL) and OSPF wildcard mask statements.
  - Layer 2 / Layer 3 gateway mismatch troubleshooting.
* **Speed Target**: Candidates must resolve IPv4 subnet boundaries, usable host limits, and routing decisions in **15 to 25 seconds** using pure mental math without physical scratch paper or calculators.

### 1.2 Limitations of Static Question Banks vs Procedural Generation
* **Static Question Banks**: Encourage superficial pattern recognition and rote memorization rather than deep algorithmic understanding.
* **Procedural Engine (v3.0.0)**: Procedurally synthesizes billions of unique, mathematically verified topologies, routing tables, and interface configurations on demand. Every drill is paired with a multi-step human mental math explanation based on Cisco's **Magic Number (Block Size)** method.

---

## 2. System Architecture

```
+-------------------------------------------------------------------------------------------------+
|                                      Browser Client (PWA)                                       |
|                                                                                                 |
|   +-------------------------------------+     +---------------------------------------------+   |
|   |         UI Presentation Layer       |     |           State & Event Controller          |   |
|   | - Cisco IOS Terminal Exhibit Blocks |     | - Single-Source-of-Truth App State          |   |
|   | - 4-Option Multiple Choice ([A]-[D])|     | - Debounced Event Loop (350ms anti-jump)    |   |
|   | - Flexible Text Input (IP/CIDR)     |     | - Optional Stopwatch Speed Drill Timer      |   |
|   | - Flashcard Mental Math Toggle      |     | - Streak Counter & Accuracy Metrics         |   |
|   | - Multi-Octet Cheat Sheet (/8~/32)  |     | - In-Place Reactive i18n Translation        |   |
|   +------------------^------------------+     +----------------------^----------------------+   |
|                      |                                               |                          |
|   +------------------v-----------------------------------------------v----------------------+   |
|   |                         Procedural Question Generation Engine                           |   |
|   |   Suite 1: Core Speed Drills (1–6)         |   Suite 2: Real CCNA Scenarios (7–12)      |   |
|   |   1. Network Address (.0/.255 traps)       |   7. Longest Prefix Match (show ip route)  |   |
|   |   2. Directed Broadcast Address            |   8. CCNA Simulation Lab (Interface IP)    |   |
|   |   3. Usable Host Bounds (DHCP Gateway)     |   9. VLSM Capacity & Alignment Planning    |   |
|   |   4. Mask / CIDR / 32-Bit Binary           |   10. Wildcard Mask & OSPF / ACL Matching  |   |
|   |   5. Usable Hosts (2^H-2) & Subnets (2^S)  |   11. CIDR Route Summarization             |   |
|   |   6. Same Subnet L2 Check                  |   12. Cisco IOS 'Bad mask' Troubleshooter  |   |
|   +---------------------------------------------^-------------------------------------------+   |
|                                                 |                                               |
|   +---------------------------------------------v-------------------------------------------+   |
|   |                              Bitwise Algorithmic Core                                   |   |
|   |   - Unsigned 32-Bit Math (uint32 bitwise operations via >>> 0)                          |   |
|   |   - Multi-Octet Magic Number Engine (Class A, B, and C Octet Localization)              |   |
|   |   - Longest Prefix Match (LPM) Evaluation Hierarchy (LPM > AD > Metric)                 |   |
|   |   - Route Summarization Analyzer (Bitwise XOR Common Prefix Extractor)                  |   |
|   |   - Inverted Wildcard Mask & Bitwise ACL Matching Rules                                 |   |
|   |   - RFC 3021 Point-to-Point (/31) and Host Route (/32) Edge Case Evaluators             |   |
|   +-----------------------------------------------------------------------------------------+   |
+-------------------------------------------------------------------------------------------------+
```

### Architectural Principles
1. **Zero Backend & 100% Offline Capable**: Zero server-side API requests, zero npm bundles, and zero third-party script dependencies. The entire application runs inside the client browser sandbox.
2. **Pure Bitwise Arithmetic**: All IP calculations, prefix manipulations, routing lookups, and wildcard matches execute as unsigned 32-bit integers (`uint32`).
3. **Viewport Stability**: DOM updates are strictly localized. Revealing solutions or cycling questions dynamically expands content without triggering window scrolling or viewport jumping.
4. **Strict Privacy**: Zero tracking cookies, zero analytics beacons, and zero telemetry.

---

## 3. Prefix Scope & Exam Distribution Model

The procedural generator samples prefixes across the IPv4 space according to real CCNA 200-301 exam weightings:

| Scope | Prefix Range | Focus Octet | Exam Representation & Weighting |
| :--- | :--- | :--- | :--- |
| **Class C** | `/24` ~ `/30` | 4th Octet | Core speed drills, branch office subnets, WAN point-to-point links. |
| **Class B** | `/16` ~ `/23` | 3rd Octet | High-frequency exam target. Enterprise campus VLANs, multi-floor SVIs, `.0` and `.255` valid host address traps. |
| **Class A** | `/8` ~ `/15` | 2nd Octet | Enterprise core backbones, ISP supernetting, large-scale route summarization. |
| **Mixed Mode** | `/8` ~ `/30` | Multi-Octet | Dynamically weighted distribution matching CCNA 200-301 domain percentages. |
| **Edge Cases** | `/0`, `/31`, `/32` | Boundary Cases | Routing and ACL scenarios: `/0` default route (`0.0.0.0/0`), `/31` RFC 3021 point-to-point links (2 usable hosts, 0 network/broadcast IDs reserved), and `/32` host routes / loopback interfaces. |

---

## 4. Mathematical & Algorithmic Foundation

### 4.1 Unsigned 32-Bit Integer Representation
Because JavaScript bitwise operators coerce operands into signed 32-bit two's complement integers, the engine utilizes the unsigned right-shift operator (`>>> 0`) after every bitwise operation:

$$\text{IP}_{\text{int}} = \left( (O_1 \ll 24) \mid (O_2 \ll 16) \mid (O_3 \ll 8) \mid O_4 \right) \ggg 0$$

### 4.2 Standard Bitwise IPv4 Formulas
Given a prefix length $n$ ($0 \le n \le 32$):
* **Subnet Mask**:
  $$\text{Mask} = \begin{cases} 0 & n = 0 \\ (\sim 0 \ll (32 - n)) \ggg 0 & 1 \le n \le 32 \end{cases}$$
* **Wildcard Mask**:
  $$\text{Wildcard} = (\sim \text{Mask}) \ggg 0$$
* **Network Address (Network ID)**:
  $$\text{Network} = (\text{IP} \;\&\; \text{Mask}) \ggg 0$$
* **Directed Broadcast Address**:
  $$\text{Broadcast} = (\text{Network} \mid \text{Wildcard}) \ggg 0$$
* **Usable Host Bounds**:
  $$\text{FirstHost} = \begin{cases} \text{Network} & n = 31 \\ \text{Network} + 1 & n \le 30 \end{cases}$$
  $$\text{LastHost} = \begin{cases} \text{Broadcast} & n = 31 \\ \text{Broadcast} - 1 & n \le 30 \end{cases}$$
* **Usable Host Count**:
  $$\text{UsableHosts} = \begin{cases} 1 & n = 32 \\ 2 & n = 31 \quad (\text{RFC 3021}) \\ 2^{(32 - n)} - 2 & 0 \le n \le 30 \end{cases}$$
* **Subnet Count ($2^S$)**:
  Given $S$ borrowed subnet bits:
  $$\text{Subnets} = 2^S$$

---

### 4.3 Longest Prefix Match (LPM) Routing Table Evaluation Algorithm
In Category 7, the engine simulates the Cisco IOS packet forwarding decision process. When a router evaluates a destination IP against routing table entries, it executes the following strict hierarchy:

$$\text{Decision} = \text{LPM (Most Specific Prefix Length)} \succ \text{Administrative Distance (AD)} \succ \text{Metric / Cost}$$

```
                          [ Incoming Destination IP ]
                                       |
                   +-------------------v-------------------+
                   |   Filter Matching Routes in Table     |
                   |      (DestIP & RouteMask == RouteNet) |
                   +-------------------+-------------------+
                                       |
                   +-------------------v-------------------+
                   |     Find Longest Prefix Match (LPM)   |
                   |      Select Route(s) with Max Prefix  |
                   +-------------------+-------------------+
                                       |
                     +-----------------+-----------------+
                     | Multiple routes with SAME prefix? |
                     +-----------------+-----------------+
                                       |
                    YES                |                NO
                     |                 |                 |
         +-----------v------------+    |     +-----------v------------+
         | Compare Admin Distance |    |     | Forward to Next-Hop /  |
         | (Lowest AD Wins)       |    |     | Exit Interface         |
         +-----------+------------+    |     +------------------------+
                     |                 |
         +-----------v------------+    |
         | If ADs are Equal,      |    |
         | Lowest Metric Wins     |    |
         | (ECMP if Identical)    |    |
         +-----------+------------+    |
                     |                 |
                     +-----------------+
```

#### Administrative Distance (AD) Reference Values
* Directly Connected (`C`): `0`
* Static Route (`S`): `1`
* External BGP (`B`): `20`
* EIGRP (`D`): `90`
* OSPF (`O`): `110`
* RIP (`R`): `120`
* Gateway of Last Resort (`S*` / `0.0.0.0/0`): Match-all fallback when no specific route matches.

#### Exam Trap Mechanics in LPM Drills
1. **LPM Overrides AD**: A candidate might see a Static Route (`AD 1`) for `10.1.0.0/16` and an OSPF Route (`AD 110`) for `10.1.1.0/24`. For destination `10.1.1.55`, the packet is forwarded via **OSPF** because `/24 > /16`, regardless of Administrative Distance.
2. **Non-Matching `/31` & `/32` Routes**: Traps where a candidate assumes a `/32` host route matches because the first three octets look similar, but the 4th octet fails bitwise comparison.

---

### 4.4 CIDR Route Summarization (Supernetting) Algorithm
Category 11 evaluates the aggregation of multiple contiguous IP subnets into a single compact CIDR route advertisement:

1. **Bitwise Difference Range**:
   Given a set of $M$ contiguous subnets with minimum IP $\text{IP}_{\min}$ and maximum IP $\text{IP}_{\max}$:
   $$\Delta_{\text{diff}} = (\text{IP}_{\min} \oplus \text{IP}_{\max}) \ggg 0$$
2. **Common Prefix Length ($K$)**:
   The number of common leading binary bits is determined by the position of the most significant bit in $\Delta_{\text{diff}}$:
   $$K = \begin{cases} 32 & \Delta_{\text{diff}} = 0 \\ 32 - \lfloor \log_2(\Delta_{\text{diff}}) + 1 \rfloor & \Delta_{\text{diff}} > 0 \end{cases}$$
3. **Summary Network Address**:
   $$\text{SummaryNet} = (\text{IP}_{\min} \;\&\; (\sim 0 \ll (32 - K))) \ggg 0$$
4. **Optimal Route Advertisement**:
   The algorithm outputs the aggregated route in standard $\text{SummaryNet}/K$ format while verifying that all subnets fall within the block and no foreign subnets are unintentionally advertised.

---

### 4.5 Wildcard Mask & OSPF / Extended ACL Matching Algorithm
Category 10 assesses inverse wildcard masks used in Cisco IOS OSPF configurations and Access Control Lists:

1. **Inverse Mask Formula**:
   $$\text{Wildcard} = 255.255.255.255 - \text{Subnet Mask}$$
2. **Bitwise Wildcard Evaluation**:
   An IP address $\text{IP}_{\text{test}}$ matches an ACL/OSPF rule $(\text{IP}_{\text{rule}}, \text{Wildcard})$ if and only if:
   $$\left( \text{IP}_{\text{test}} \;\&\; (\sim \text{Wildcard}) \right) == \left( \text{IP}_{\text{rule}} \;\&\; (\sim \text{Wildcard}) \right)$$
3. **OSPF Area Matching Simulation**:
   The engine tests statements such as `network 172.16.16.0 0.0.7.255 area 0` to identify which interface IPs fall within the wildcard range (`172.16.16.0` through `172.16.23.255`).

---

### 4.6 VLSM Sizing, Growth Buffer & Subnet Alignment Algorithm
Category 9 models enterprise multi-floor VLAN capacity planning:

1. **Growth Buffer Sizing**:
   Given $N_{\text{current}}$ required host devices and a future growth buffer percentage $P$ (e.g. $+25\%$):
   $$N_{\text{req}} = \lceil N_{\text{current}} \times (1 + P) \rceil$$
2. **Minimum Host Bits ($H$)**:
   Find the smallest integer $H$ satisfying the usable host condition ($2^H - 2 \ge N_{\text{req}}$):
   $$H = \lceil \log_2(N_{\text{req}} + 2) \rceil, \quad \text{Prefix} = 32 - H$$
3. **Subnet Boundary Alignment Check**:
   A valid subnet base address must be an exact multiple of the block size ($2^H$):
   $$\text{SubnetIP}_{\text{int}} \pmod{2^H} == 0$$
   If this condition is violated, the generator flags an **Unaligned Subnet Boundary Trap**.

---

### 4.7 Cisco IOS `Bad Mask` Diagnostic Algorithm
Category 12 evaluates Cisco IOS CLI configuration errors:

1. **Network / Broadcast Boundary Rejection**:
   Cisco IOS rejects assigning a subnet's wire network address (all host bits 0) or directed broadcast address (all host bits 1) to a standard `/8`–`/30` router interface:
   $$\text{Bad mask /XX for address } \langle\text{IP}\rangle$$
2. **Interface Subnet Overlap Detection**:
   When configuring two router interfaces (e.g. `GigabitEthernet0/0` and `GigabitEthernet0/1`), the router computes the network ID for both interfaces. If:
   $$(\text{IP}_1 \;\&\; \text{Mask}_1) == (\text{IP}_2 \;\&\; \text{Mask}_2)$$
   Cisco IOS generates `% 192.168.1.0/24 overlaps with GigabitEthernet0/0`.

---

## 5. Detailed Specifications of the 12 CCNA Question Categories

### Suite 1: Core Speed Drills

#### Category 1: Network Address (Network ID)
* **Objective**: Compute the base wire address from an arbitrary IP and prefix.
* **Exam Traps**: Includes valid unicast host addresses ending in `.0` or `.255` on `/16` to `/23` networks (e.g. `172.16.1.0/22` is a valid host IP, with network address `172.16.0.0/22`).

#### Category 2: Directed Broadcast Address
* **Objective**: Calculate the directed broadcast IP for a given subnet.
* **Deduction Steps**: Identify the interesting octet, find the block size, locate the next subnet boundary, and subtract 1.

#### Category 3: Usable Host Bounds
* **Objective**: Rapidly determine the First Usable Host, Last Usable Host, or the DHCP `default-router` Next-to-Last Usable IP commonly configured on Cisco routers.

#### Category 4: Mask / CIDR / 32-Bit Binary Conversion
* **Objective**: Convert seamlessly between prefix lengths (`/XX`), dotted-decimal masks (`255.255.X.X`), and 32-bit binary notation without decimal-to-binary scratch pads.

#### Category 5: Usable Host ($2^H - 2$) & Subnet Count ($2^S$) Calculation
* **Objective**: Calculate the total usable host capacity or number of subnets created when borrowing $S$ subnet bits from a classful boundary.

#### Category 6: Same Subnet Check (Layer 2 Direct Communication)
* **Objective**: Evaluate whether two hosts with identical or different subnet masks reside in the same Layer 2 broadcast domain and can communicate directly without a router.

---

### Suite 2: Real CCNA Exam Scenarios

#### Category 7: Longest Prefix Match (LPM) Routing Table Lookup
* **Objective**: Parse a simulated `show ip route` exhibit and determine which next-hop IP or egress interface forwards a packet addressed to destination $X$.
* **Rules Tested**: $\text{LPM} \succ \text{Administrative Distance} \succ \text{Metric}$. Covers default route (`0.0.0.0/0`) fallback and non-matching traps.

#### Category 8: CCNA Simulation Lab (Nth Subnet & Interface IP)
* **Objective**: Given a major network allocation (e.g. `10.20.0.0/19`), subdivide into equal subnets and calculate the exact interface IP and mask for the $N$-th subnet or the first/last usable IP for a router interface.

#### Category 9: VLSM Capacity, Growth Buffer & Subnet Alignment
* **Objective**: Calculate the optimal subnet mask for a VLAN requiring $X$ hosts plus a $Y\%$ growth buffer. Conserve IP address space on WAN point-to-point links and identify invalid, misaligned subnet boundaries.

#### Category 10: Wildcard Mask & OSPF / Extended ACL Matching
* **Objective**: Calculate inverse wildcard masks and determine which IP addresses match specific Cisco IOS OSPF `network` statements or Extended ACL permit/deny statements.

#### Category 11: CIDR Route Summarization (Supernetting)
* **Objective**: Aggregate a list of 4, 8, or 16 contiguous subnets into the most specific single summary route (`IP/CIDR`) for BGP or OSPF route advertisement.

#### Category 12: Cisco IOS `Bad Mask` & Gateway/Subnet Mismatch Troubleshooting
* **Objective**: Analyze Cisco IOS configuration snippets and identify configuration faults: invalid subnet masks, overlapping router interfaces, or host/default-gateway subnet mismatches.

---

## 6. Exam Mental Math Engine: The "Magic Number" Method

To prepare candidates for exam conditions, explanations eschew raw 32-bit binary dumps in favor of Cisco's **Magic Number (Block Size)** mental math framework:

```
                           [ Given IP & Prefix /n ]
                                      |
                     +----------------v---------------+
                     |   1. Octet Boundary Check      |
                     |      n mod 8 == 0 ?            |
                     +----------------+---------------+
                                      |
                    NO                |                YES (/8, /16, /24)
                     |                |                 |
     +---------------v--------------+ |   +-------------v--------------+
     | 2. Identify Interesting      | |   | Subnet falls cleanly on    |
     |    Octet: floor(n / 8) + 1   | |   | boundary. Magic Number = 1 |
     +---------------+--------------+ |   +----------------------------+
                     |                |
     +---------------v--------------+ |
     | 3. Calculate Magic Number:   | |
     |    256 - SubnetMaskOctet     | |
     +---------------+--------------+ |
                     |                |
     +---------------v--------------+ |
     | 4. Localize Subnet Interval: | |
     |    Find largest multiple K   | |
     |    where K * Magic <= IPOctet| |
     +---------------+--------------+ |
                     |                |
                     +----------------+
                                      |
                     +----------------v---------------+
                     | Output Network ID, Host Range, |
                     | Broadcast & Gateway Limits     |
                     +--------------------------------+
```

### Mental Math Steps:
1. **Octet Boundary Detection**: If prefix $n \in \{8, 16, 24\}$, the address falls cleanly on an octet boundary; no borrowing is required.
2. **Interesting Octet Identification**:
   $$\text{OctetIndex} = \lfloor n / 8 \rfloor + 1$$
   - `/8` ~ `/15` $\rightarrow$ 2nd Octet
   - `/16` ~ `/23` $\rightarrow$ 3rd Octet
   - `/24` ~ `/30` $\rightarrow$ 4th Octet
3. **Magic Number (Block Size) Deduction**:
   $$\text{MagicNumber} = 256 - \text{MaskOctetValue}$$
4. **Subnet Interval Localization**:
   Given host octet value $V$, find multiplier $K = \lfloor V / \text{MagicNumber} \rfloor$:
   $$\text{SubnetBaseOctet} = K \times \text{MagicNumber}$$
   $$\text{NextSubnetBaseOctet} = (K + 1) \times \text{MagicNumber}$$
   $$\text{BroadcastOctet} = \text{NextSubnetBaseOctet} - 1$$

---

## 7. Internationalization (i18n) Architecture

The application provides instantaneous tri-lingual switching across English (`EN`), Japanese (`JA`), and Chinese (`ZH`):

* **Parameterized Dictionary Interpolation**: All UI strings, terminal headers, question templates, and step-by-step mental math explanations are stored as functions that accept dynamic network parameters.
* **In-Place Reactive Re-Rendering**: Language changes trigger immediate in-place DOM updates without re-generating the active question, resetting streak metrics, or triggering page reloads.

### Core Terminology Comparison Matrix

| Concept | English (Cisco Global) | Japanese (Cisco JA) | Chinese (Simplified) |
| :--- | :--- | :--- | :--- |
| **Network Address** | Network Address | ネットワークアドレス | 网络地址 |
| **Directed Broadcast** | Directed Broadcast Address | ディレクテッドブロードキャスト | 定向广播地址 |
| **First Usable Host** | First Usable Host | 最初の使用可能ホスト | 第一个可用主机 |
| **Last Usable Host** | Last Usable Host | 最後の使用可能ホスト | 最后一个可用主机 |
| **Magic Number** | Magic Number (Block Size) | マジックナンバー (ブロックサイズ) | 魔术数字 (步长/块大小) |
| **Interesting Octet** | Interesting Octet | 着目オクテット | 关键字节 |
| **Longest Prefix Match** | Longest Prefix Match (LPM) | 最長一致 (LPM) | 最长前缀匹配 |
| **Admin Distance** | Administrative Distance (AD) | アドミニストレーティブディスタンス | 管理距离 |
| **Wildcard Mask** | Wildcard Mask | ワイルドカードマスク | 反掩码 / 通配符掩码 |
| **Route Summarization**| Route Summarization | ルート集約 | 路由汇总 |
| **Same Subnet Check** | Same Subnet Check | 同一サブネット判定 | 同子网判定 |

---

## 8. UI/UX & Interactive Exhibit Engineering

### 8.1 Cisco IOS Terminal Exhibits
The UI includes a high-fidelity Cisco terminal renderer styled after standard dark-mode network consoles:
- Renders `Router# show ip route` output with authentic protocol codes (`C`, `S`, `O`, `D`, `B`, `S*`).
- Formats interface configuration blocks and diagnostic `% Bad mask` alerts with fixed-width typography.

### 8.2 Dual Input System & Keyboard Workflows
- **Multiple Choice Mode**: 4 options rendered with keyboard hotkeys `[A]`, `[B]`, `[C]`, `[D]` (and numerical equivalents `[1]`, `[2]`, `[3]`, `[4]`).
- **Flexible Text Input**: Tolerant parser accepting both CIDR notation (`10.0.1.0/24`) and IP/Mask notation (`10.0.1.0 255.255.255.0`).
- **Flashcard Mode**: Pure mental math mode for rapid 10-second drill loops.
- **Keyboard Navigation**:
  - `[Enter]`: Submits typed answers or advances to the next question.
  - `[Space]`: Reveals solution breakdown in Flashcard mode.

### 8.3 Anti-Jitter Viewport Stability
- Submitting an answer or toggling explanations locks the input container height and smoothly animates the explanation drawer below.
- Built-in **350ms keydown debounce** prevents accidental double-submissions when holding down `[Enter]`.

### 8.4 Multi-Octet Reference Drawer
The cheat sheet drawer covers `/8` through `/32` with columns for:
- Prefix Length (`/XX`)
- Subnet Mask (`255.X.X.X`)
- Borrowed Bits ($S$)
- Magic Number (Block Size)
- Usable Hosts ($2^H - 2$)
- Wildcard Byte
