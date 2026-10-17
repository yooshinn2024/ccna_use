# CCNA Subnet Master

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Web%20%2F%20PWA-brightgreen.svg)]()
[![Languages](https://img.shields.io/badge/Languages-JA%20%7C%20EN%20%7C%20ZH-orange.svg)]()

> A high-speed, zero-dependency procedural IPv4 subnetting trainer tailored for Cisco CCNA (200-301) candidates.

---

## 🌐 Live Demo

- **Custom Domain**: [https://subnet.yooshinngotanidea.com](https://subnet.yooshinngotanidea.com)
- **GitHub Pages**: [https://yooshinn2024.github.io/ccna_use/](https://yooshinn2024.github.io/ccna_use/)

---

## ✨ Key Features

- **7 High-Frequency CCNA Question Types**:
  1. **Network Address (Network ID)**
  2. **Broadcast Address**
  3. **First Valid Host**
  4. **Last Valid Host**
  5. **Usable Hosts Count**
  6. **Subnet Mask & CIDR Prefix Conversion**
  7. **Same Subnet Determination** (*Can Host A and Host B communicate directly at Layer 2 without a router?*)
- **Prefix Difficulty Scopes**:
  - **Class C (`/24` ~ `/30`)**: Core mental math fundamentals.
  - **Class B (`/16` ~ `/23`)**: High-weight CCNA exam targets (focusing on the 3rd octet).
  - **Class A (`/8` ~ `/15`)**: Advanced practice.
- **Exam-Grade "Magic Number" (Block Size) Step-by-Step Breakdown**:
  - Automatic detection of octet boundaries (`/8`, `/16`, `/24`).
  - Identifies the **Interesting Octet**.
  - Calculates the **Magic Number** ($\text{Block Size} = 256 - \text{Mask}$).
  - Locates the exact subnet interval and valid host range.
- **Tri-Lingual Support (i18n)**:
  - **Japanese (`日本語`)**: Default language, aligned with Cisco Japan CCNA exam terminology.
  - **English (`English`)**: Cisco Global Standard terminology.
  - **Chinese (`中文`)**: Simplified Chinese terminology.
- **Optional Stopwatch & Keyboard-First Workflow**:
  - Timer is disabled by default for stress-free practice and can be toggled on with a single click for sub-20-second speed drills.
  - `[Enter]` to submit answers and advance to the next question; `[Space]` to reveal solutions.
- **Built-in CCNA Cheat Sheet**:
  - Collapsible quick-reference chart covering `/25` to `/32` dotted decimal masks, borrowed bits, magic numbers, and usable host counts.

---

## 🛠️ Technical Stack

- **Single-File HTML5 Architecture**: Zero build steps, zero external npm packages, and zero CDN dependencies. Works 100% offline.
- **Bitwise Calculation Engine**: Powered by unsigned 32-bit integer bitwise operations (`>>> 0`) in vanilla JavaScript.
- **Privacy First**: No analytics, no tracking cookies, and no server-side data collection.

For detailed mathematical models and architectural design, see [DESIGN.md](DESIGN.md).

---

## 📱 Mobile Usage (PWA-Style)

1. Open the live URL in **Safari (iOS)** or **Chrome (Android)**.
2. Tap the **Share** button and select **"Add to Home Screen"**.
3. Launch it directly from your home screen in full-screen mode for daily practice on the go.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
