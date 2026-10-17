# CCNA Subnet Master - System Architecture & Design Document

## 1. Vision & Objectives

**CCNA Subnet Master** is a high-speed IPv4 subnetting drill engine purpose-built for Cisco Certified Network Associate (CCNA 200-301) candidates.

### Core Challenges & Solutions
* **Exam Time Constraints**: The CCNA exam consists of roughly 100 questions within a 120-minute window (~70 seconds per question). Because subnetting underpins routing tables, ACLs, and Layer 2/3 troubleshooting, candidates must solve subnetting calculations in **15 to 25 seconds using pure mental math**.
* **Limitations of Static Question Banks**: Pre-generated question lists encourage rote memorization rather than algorithmic fluency, and often lack step-by-step mental math explanations.
* **Our Solution**: A **zero-dependency, pure client-side (serverless)** procedural generation engine featuring infinite randomized questions, step-by-step "Magic Number" deductions, and instant tri-lingual (`JA` / `EN` / `ZH`) switching.

---

## 2. System Architecture

```
+-----------------------------------------------------------------+
|                       Browser Client (PWA)                      |
|                                                                 |
|   +-------------------+    +--------------------------------+   |
|   |   UI Presentation |    |      State & Control Engine    |   |
|   |  - Responsive CSS |    |  - Dual Mode (Input/Flashcard) |   |
|   |  - Keyboard Nav   |    |  - Optional Stopwatch Timer    |   |
|   |  - CCNA Drawer    |    |  - Streaks / Accuracy Tracker  |   |
|   +---------^---------+    +----------------^---------------+   |
|             |                               |                   |
|   +---------v-------------------------------v---------------+   |
|   |                   Question Generator Engine             |   |
|   |   - 7 Core CCNA Question Types                          |   |
|   |   - Dynamic IP & Prefix Sampling (/8 ~ /30)             |   |
|   +-----------------------------^---------------------------+   |
|                                 |                               |
|   +-----------------------------v---------------------------+   |
|   |             IPv4 Bitwise Math & Algorithm Core          |   |
|   |   - 32-Bit Unsigned Math (uint32)                       |   |
|   |   - Magic Number (Block Size) Deduction                 |   |
|   |   - Interesting Octet Identification                    |   |
|   +---------------------------------------------------------+   |
+-----------------------------------------------------------------+
```

### Design Principles
1. **Zero Backend & 100% Offline Capable**: All calculations, question synthesis, and answer verifications execute inside the browser sandbox in vanilla JavaScript without external libraries or network requests.
2. **Lightweight & Responsive**: Packaged as a single self-contained HTML file optimized for both desktop keyboard workflows and mobile portrait touchscreens.
3. **Strict Privacy**: Zero telemetry, zero cookies, and zero external API calls.

---

## 3. Mathematical & Algorithmic Foundation

An IPv4 address is fundamentally a 32-bit binary integer. Rather than relying on error-prone string manipulation, the engine performs **unsigned 32-bit bitwise arithmetic**.

### 3.1 IP to Unsigned 32-Bit Integer Conversion
Because JavaScript bitwise operators default to signed 32-bit integers, the unsigned right-shift operator (`>>> 0`) is applied to coerce values into standard unsigned 32-bit integers (`uint32`):
$$\text{IP}_{\text{int}} = ((O_1 \ll 24) \mid (O_2 \ll 16) \mid (O_3 \ll 8) \mid O_4) \ggg 0$$

### 3.2 Subnet Mask & Core Parameter Formulas
Given a CIDR prefix length $n$ ($0 \le n \le 32$):
* **Subnet Mask (Netmask)**:
  $$\text{Mask} = (\sim 0 \ll (32 - n)) \ggg 0$$
* **Wildcard Mask**:
  $$\text{Wildcard} = (\sim \text{Mask}) \ggg 0$$
* **Network Address (Network ID)**:
  $$\text{Network} = (\text{IP} \;\&\; \text{Mask}) \ggg 0$$
* **Broadcast Address**:
  $$\text{Broadcast} = (\text{Network} \mid \text{Wildcard}) \ggg 0$$
* **Usable Host Range**:
  * First Usable Host: $\text{FirstHost} = \text{Network} + 1$
  * Last Usable Host: $\text{LastHost} = \text{Broadcast} - 1$
  * Usable Host Count: $2^{(32 - n)} - 2$ (for $n \le 30$; edge cases handled for `/31` RFC 3021 point-to-point links with 2 hosts and `/32` host routes with 1 host).

---

## 4. Exam Mental Math Engine: The "Magic Number" Method

Aligned with official Cisco curricula and widely used CCNA training (such as Jeremy's IT Lab and Wendell Odom's Official Cert Guide), the explanation engine models the human mental math process rather than dumping raw binary strings:

### Deduction Pipeline
1. **Octet Boundary Detection**:
   If $n \pmod 8 == 0$ (`/8`, `/16`, `/24`), the subnet falls cleanly on an octet boundary. No bit borrowing is required.
2. **Interesting Octet Identification**:
   $$\text{OctetIndex} = \lfloor n / 8 \rfloor + 1$$
   * `/16` ~ `/23` $\rightarrow$ 3rd Octet (Class B focus).
   * `/24` ~ `/30` $\rightarrow$ 4th Octet (Class C focus).
3. **Magic Number (Block Size) Calculation**:
   $$\text{MagicNumber} = 256 - \text{MaskOctetValue}$$
4. **Subnet Block Interval Localization**:
   Given the IP's value $V$ in the interesting octet, find the largest integer multiple $K$ such that $K \times \text{MagicNumber} \le V$:
   $$\text{NetworkOctetValue} = K \times \text{MagicNumber}$$
5. **Full Lifecycle Breakdown**: Outputs the exact network ID, broadcast boundary, and Layer 2 direct communication verdict.

---

## 5. Internationalization (i18n) Architecture

The application supports real-time switching between Japanese (`ja`, default), English (`en`), and Chinese (`zh`):

* **Dictionary-Driven Interpolation**: Question prompts and step-by-step explanations are defined as parameterized template functions.
* **In-Place Reactive Translation**: Switching languages dynamically re-renders all static UI controls, the cheat sheet table, and the active question/explanation in place without resetting the user's current question or streak counter.

| Concept | Japanese (Cisco JA) | English (Cisco Global) | Chinese (Simplified) |
| :--- | :--- | :--- | :--- |
| **Network Address** | ネットワークアドレス | Network Address | 网络地址 |
| **Broadcast Address** | ブロードキャストアドレス | Broadcast Address | 广播地址 |
| **Interesting Octet** | 着目オクテット | Interesting Octet | 关键字节 |
| **Magic Number** | マジックナンバー (ブロックサイズ) | Magic Number (Block Size) | 魔术数字 (步长/块大小) |
| **Same Subnet Check** | 同一サブネット判定 | Same Subnet Check | 同子网判定 |

---

## 6. User Experience (UX) & Interaction Design

1. **Dual Practice Modes**:
   * **Real Exam Input Mode**: Validates typed IP/CIDR answers upon pressing `Enter`.
   * **Flashcard Mode**: Mirrors classic `subnetting.org` mental math drills with one-key answer reveal.
2. **Non-Intrusive Optional Timer**:
   * Disabled by default to allow focused learning;
   * Toggleable at any time for timed speed runs.
3. **Zero-Jump Viewport Stability**:
   * Submitting an answer locks the input in place and smoothly expands the explanation panel below without triggering browser auto-scroll jumps.
   * Built-in `350ms` keydown debounce prevents accidental double-submissions when holding the `Enter` key.
