# CCNA Subnet Master - 系统设计与架构思路文档 (Design Document)

## 1. 项目定位与设计目标 (Vision & Objectives)

**CCNA Subnet Master** 是一款专为思科认证网络工程师（Cisco CCNA 200-301）备考设计的极速子网划分训练引擎。

### 核心痛点与解决策略
* **考场时间瓶颈**：CCNA 考试通常包含约 100 道题目，平均每题作答时间仅 70 秒。子网划分是路由、ACL、网络排错的基石，考生必须在 **15 ~ 25 秒内靠心算得出结果**。
* **传统题库缺陷**：固定题库容易导致考生“死记硬背”，且缺乏即时步骤推导。
* **解决方案**：构建**零依赖、纯客户端（Zero-Backend）**的动态算法生成引擎，支持无限随机题目、魔术数字法步骤推导与多语言（日/英/中）自由切换。

---

## 2. 系统技术架构 (System Architecture)

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

### 核心设计原则
1. **完全零后端与离线运行**：所有计算和题目均在浏览器沙箱内通过原生 JavaScript 完成，无任何外部库或 CDN 依赖，支持断网刷题。
2. **轻量与响应式**：单文件打包，针对桌面端（全键盘流）与移动端（添加到主屏幕 PWA）做双重适配。
3. **数据隐私性**：无数据埋点、无第三方 Cookie、刷题数据仅留存在客户端本地生命周期中。

---

## 3. 算法设计与数学模型 (Mathematical & Algorithmic Foundation)

IPv4 地址是由 32 个二进制位（Bits）组成的数据结构。传统字符串截取方案性能差且容易出错，本项目全面采用 **32 位无符号整数按位运算（Bitwise Operations）**。

### 3.1 IP 与 32 位无符号整数互转
JavaScript 的位移运算符默认处理 32 位有符号整数，通过 `>>> 0`（零填充右移）将其统一转换为标准无符号 32 位整数：
$$\text{IP}_{\text{int}} = (O_1 \ll 24) \mid (O_2 \ll 16) \mid (O_3 \ll 8) \mid O_4 \pmod{2^{32}}$$

### 3.2 掩码与关键参数计算
对于给定的 CIDR 前缀长度 $n$ ($0 \le n \le 32$)：
* **子网掩码 (Netmask)**：
  $$\text{Mask} = (\sim 0 \ll (32 - n)) \ggg 0$$
* **通配符反掩码 (Wildcard Mask)**：
  $$\text{Wildcard} = (\sim \text{Mask}) \ggg 0$$
* **网络地址 (Network Address)**：
  $$\text{Network} = (\text{IP} \;\&\; \text{Mask}) \ggg 0$$
* **广播地址 (Broadcast Address)**：
  $$\text{Broadcast} = (\text{Network} \mid \text{Wildcard}) \ggg 0$$
* **有效主机范围 (Valid Host Range)**：
  * 首有效主机：$\text{FirstHost} = \text{Network} + 1$
  * 末有效主机：$\text{LastHost} = \text{Broadcast} - 1$
  * 容纳有效主机数：$2^{(32 - n)} - 2$（$n \le 30$ 时；特殊前缀 $/31$ 为 2 台，$/32$ 为 1 台）

---

## 4. 思科考场心算法：魔术数字引擎 (Magic Number Method)

为了契合 CCNA 权威教学（如 Jeremy's IT Lab、Wendell Odom 官方认证指南），解析引擎未采用机械的二进制掩码直接输出，而是实现了**魔术数字（步长/块大小）推导算法**：

### 算法推导流程：
1. **判断整字节边界 (Octet Boundary)**：
   若 $n \pmod 8 == 0$（如 `/16`, `/24`），直接判定为整字节边界，网络位与主机位天然分割，无需借位。
2. **定位着目字节 (Interesting Octet)**：
   $$\text{OctetIndex} = \lfloor n / 8 \rfloor + 1$$
   * $/16 \sim /23$ $\rightarrow$ 锁定第 3 字节（Class B 易错区）；
   * $/24 \sim /30$ $\rightarrow$ 锁定第 4 字节（Class C 核心区）。
3. **计算魔术数字 (Block Size / 步长)**：
   $$\text{MagicNumber} = 256 - \text{MaskOctetValue}$$
4. **定位所属子网区间**：
   取该字节实际 IP 值 $V$，寻找满足 $K \times \text{MagicNumber} \le V$ 的最大整数 $K$：
   $$\text{NetworkOctetValue} = K \times \text{MagicNumber}$$
5. **输出全生命周期解析**：包括网络号、广播号及二层通信可行性结论。

---

## 5. 多语言国际化架构 (i18n Architecture)

针对全球备考群体，系统内置日、英、中三种语言运行时，默认采用**日本思科官方考试术语标准（Japanese）**：

* **数据驱动字典 (Dictionary-driven)**：字典中将动态题干与解析步骤封装为模板函数，支持参数化实时插值。
* **无刷新状态持久**：切换语言时，动态更新所有静态 DOM，并在内存中重新计算当前题目的多语言呈现，不丢失答题连胜（Streak）进度。

| 概念 | 日语 (CCNA标准) | 英语 (Cisco Official) | 中文 |
| :--- | :--- | :--- | :--- |
| **网络地址** | ネットワークアドレス | Network Address | 网络地址 |
| **广播地址** | ブロードキャストアドレス | Broadcast Address | 广播地址 |
| **着目字节** | 着目オクテット | Interesting Octet | 关键字节 |
| **魔术数字** | マジックナンバー (ブロックサイズ) | Magic Number (Block Size) | 魔术数字 (步长/块大小) |
| **同子网判定** | 同一サブネット判定 | Same Subnet Check | 同子网判定 |

---

## 6. 交互体验设计 (UX & Gamification)

1. **双练习模式**：
   * **实战输入模式 (Input Mode)**：强制回车提交，做题逻辑闭环，验证考场输入准确度。
   * **翻牌心算模式 (Flashcard Mode)**：还原经典 `subnetting.org` 极简交互，专注口算速度。
2. **非强制性可选秒表**：
   * 默认关闭，减少初始心理压力；
   * 支持一键开启，为考前 1 个月冲刺提供 20 秒极限心算训练支持。
3. **极速键盘流支持**：
   * `Enter`：提交答案 / 切换下一题
   * `Space`：揭晓解析 / 翻牌

