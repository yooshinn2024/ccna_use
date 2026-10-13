# CCNA Subnet Master (サブネット計算特訓)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Web%20%2F%20PWA-brightgreen.svg)]()
[![Languages](https://img.shields.io/badge/Languages-JA%20%7C%20EN%20%7C%20ZH-orange.svg)]()

> A high-speed, zero-dependency procedural IPv4 subnetting training application tailored for Cisco CCNA 200-301 candidates.  
> 思科 CCNA 200-301 认证考试必备：极速 IPv4 子网划分心算训练工具。

---

## 🌐 线上试玩 (Live Demo)

- **GitHub Pages 网址**：[https://yooshinn2024.github.io/ccna_use/](https://yooshinn2024.github.io/ccna_use/)

---

## ✨ 核心特性 (Key Features)

- **7 大 CCNA 高频题型**：
  1. 求解网络地址 (Network Address / Network ID)
  2. 求解广播地址 (Broadcast Address)
  3. 求解首个可用主机 (First Valid Host)
  4. 求解末个可用主机 (Last Valid Host)
  5. 求解有效主机总数 (Usable Hosts Count)
  6. 子网掩码与 CIDR 前缀互转 (Subnet Mask & CIDR Conversion)
  7. 同子网通信判定 (Same Subnet / Can they ping directly without a router?)
- **前缀难度分级训练**：
  - Class C (`/24` ~ `/30`)：核心速算肌肉记忆
  - Class B (`/16` ~ `/23`)：考场高频易错题（着目第 3 字节）
  - Class A (`/8` ~ `/15`)：进阶拓展
- **考场级魔术数字法 (Magic Number / Block Size) 步骤推导**：
  - 自动识别整字节边界 (Octet Boundary)
  - 锁定着目字节 (Interesting Octet)
  - 步长推导计算 ($256 - \text{Mask}$)
- **三语无缝切换 (Tri-lingual i18n)**：
  - 日本語 (Japanese - 默认符合日本思科 CCNA 考试规范)
  - English (Cisco Global Standard)
  - 中文 (Chinese)
- **可选秒表与全键盘流支持**：
  - 计时器默认关闭，支持一键开启进行 20 秒极限冲刺；
  - `[Enter]` 提交与下一题，`[Space]` 查看解析。
- **内置 CCNA 借位速查表 (Cheat Sheet)**：
  - 包含 `/25` 至 `/32` 掩码、借位数、块大小及主机数对照。

---

## 🛠️ 技术架构 (Technical Stack)

- **纯前端单文件 (Single-file HTML5)**：无任何构建依赖、无任何第三方 NPM 包或 CDN 脚本，100% 离线可用。
- **高精度位运算引擎**：基于 32 位无符号整数按位运算（`>>> 0`）。
- **零隐私追踪**：无埋点、无 Cookie、无网络上传。

详尽架构推导与数学公式请参阅 [DESIGN.md](DESIGN.md)。

---

## 📱 移动端推荐用法 (Mobile PWA)

1. 在手机浏览器（Safari / Chrome）中打开网页；
2. 点击分享按钮，选择 **“添加到主屏幕” (ホーム画面に追加)**；
3. 即可在手机上获得全屏无地址栏的原生 App 体验，随时随地离线刷题。

---

## 📄 许可证 (License)

本项目基于 [MIT License](LICENSE) 开源。

