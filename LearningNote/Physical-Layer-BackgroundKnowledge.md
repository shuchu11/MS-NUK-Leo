<img width="1322" height="737" alt="image" src="https://github.com/user-attachments/assets/3a337a10-c276-426b-9f49-b7dffa8558f1" />

# Frame Structure
Unlike LTE’s fixed frame design, **5G NR adapts dynamically** to each service’s requirements.\
This flexibility allows the network to optimize performance for both high-throughput and ultra-low-latency use cases.

🧱 **Time-Frequency Resource Allocation**
- The time-frequency domain is divided into:

    - Resource Blocks (RBs): 12 subcarriers in frequency

    - OFDM Symbols: units in time

    - Together they form the **resource grid**, the basic unit for scheduling and transmission.

This structure enables simultaneous support for:

- High-bandwidth services (e.g., 4K streaming)

- Low-latency applications (e.g., autonomous driving)

⏱️ **Slot-Based Design**
Each slot contains 14 OFDM symbols (normal CP)

Slot duration varies with numerology (μ):

Higher μ → larger subcarrier spacing → shorter slot duration

## 📐 Hierarchy of 5G Frame Structure
> [!tip]
> *Resource Grid* : A 2D grid formed by **frequency domin**(RBs) and **time domin**(OFDM symbols)
- **time domin**
  
Hierarchy of Frame Structure : Frame(Radio Frame) > SubFrame > Slot (Same as in LTE) > OFDM Symbol

| Layer             | Description                                                                 |
|:-----------------:|:---------------------------------------------------------------------------:|
| **Radio Frame**   | Fixed length of **10 ms**                                                   |
| **Subframe**      | Each radio frame contains **10 subframes**, each of **1 ms**                |
| **Slot**          | Number of slots per subframe **varies depending on numerology (μ)**         |
| **OFDM Symbol**   | Each slot contains **14 symbols** (for normal cyclic prefix)                |

- **frequency domin**

| Layer             | Description                                                                |
|:-----------------:|:--------------------------------------------------------------------------:|
| **Resource Block**| Each RB contains **12 subcarriers** (same as LTE)                          |

**RB Duration** :  One RB spans **12 subcarriers × 14 OFDM symbols** in one slot        


## Numerology - Subcarrier Spacing
🔍 Key Differences from LTE
Unique Feature: Support for multiple numerologies (i.e., different subcarrier spacings)

Subcarrier Spacing Options: 15, 30, 60, 120, 240, 480, 960 kHz (LTE only supports 15 kHz)

<pr>
<img width="592" height="383" alt="image" src="https://github.com/user-attachments/assets/6f998fef-a548-468f-b782-d826c3a02b53" />
<pr>

- **Higher μ → shorter slot duration**

- Enables low-latency services and real-time control

**Numerology and Channel Support**
<pr>
<img width="876" height="437" alt="image" src="https://github.com/user-attachments/assets/a17471ab-ab68-4fe1-bf14-212d6a0f0359" />
<pr>

## OFDM Symbol Duration

The table below lists the **key parameters of the 5G network's physical layer frame structure** under different numerologies (denoted by μ).\
In 5G, **"numerology"** is used to define the following parameters:

- Subcarrier Spacing
- OFDM Symbol Duration
- Cyclic Prefix Duration
- Total OFDM Symbol Duration (including cyclic prefix)

The numerology values range from 0 to 4, corresponding to subcarrier spacings of 15, 30, 60, 120, and 240 kHz, respectively.\
**A higher numerology indicates a wider subcarrier spacing.** As the numerology increases, **both the OFDM symbol duration and**\
**the cyclic prefix duration become shorter**, resulting in a shorter overall OFDM symbol duration (including the cyclic prefix).

<pr>
<img width="1002" height="132" alt="image" src="https://github.com/user-attachments/assets/592b6e3c-b3a4-449d-bce0-29f2343705d3" />
<pr>

Here, we briefly derive the mathematical relationships between them.\
**EX : Numerlogy = 0 ( Subcarrier Spacing = 15(kHz) )**

**Q.** Why OFDM Symbol Duration = 66.67 (us) ? \
**A.**
T = 1 / freq \
--> OFDM Symbol Duration = 1 / Subcarrier Spacing \
    = 1 / 15kHz = 66.67 (us)
    
**Q.** Why Cyclic Prefix Duration = 4.69 (us)\
**A.** Cyclic Prefix Duration = OFDM Symbol Duration x 7%

## Radio Frame Structure
Regardless of the numerology, the length of a Radio Frame is always 10 ms, and the length of a Subframe is always 1 ms.

So, what should be changed to accommodate the physical characteristics of different numerologies?

The answer is to **adjust the number of slots** within each Subframe.

A Slot with a Normal Cyclic Prefix (CP) contains 14 symbols, while a Slot with an Extended CP contains 12 symbols.

### < Normal CP, Numerology = 0 >
<img width="1322" height="737" alt="image" src="https://github.com/user-attachments/assets/3a337a10-c276-426b-9f49-b7dffa8558f1" />

### < Normal CP, Numerology = 1 >
<img width="1247" height="708" alt="image" src="https://github.com/user-attachments/assets/8e6e1e9c-3852-4fdf-8a67-a15958c36f15" />

### < Normal CP, Numerology = 2 >
<img width="1373" height="716" alt="image" src="https://github.com/user-attachments/assets/463268e4-f81e-42a1-a096-f551f5c74a06" />

### < Normal CP, Numerology = 3 >
<img width="1415" height="797" alt="image" src="https://github.com/user-attachments/assets/98a8ab34-7c4a-4ffb-b8d1-93f10d5bec19" />



## Slot Format
<img width="498" height="842" alt="image" src="https://github.com/user-attachments/assets/9e3082d0-4e64-42dc-b0b5-79859249d3e5" />

📌 Definition\
Slot Format defines how each symbol within a slot is used.\
A slot typically consists of 14 OFDM symbols (with Normal Cyclic Prefix).
- Each symbol can be labeled as:
    - D (Downlink): for downlink transmission
    - U (Uplink): for uplink transmission
    - F (Flexible): can be used as either DL or UL depending on the scheduling

- Theoretical flexibility:
    - Almost infinite symbol combinations are possible in theory.
    - 3GPP standardizes only 61 predefined combinations, known as Slot Formats.
    - These Slot Formats are essential for flexible and dynamic scheduling, especially in TDD operation.

## TDD vs FDD
| Characteristic       | TDD                                               | FDD                                           |
|:--------------------:|:-------------------------------------------------:|:---------------------------------------------:|
| Frequency Usage      | UL/DL share the same frequency alternately        | UL and DL use different frequency bands       |
| Hardware Requirement | Simpler (single RF path)                          | More complex (dual RF paths)                  |
| Applicable Scenario  | Asymmetric traffic, high frequency (e.g., mmWave) | Symmetric traffic, low frequency              |





## 📡 supportedSubcarrierSpacingDL Summary
```
FeatureSetDownlinkPerCC ::= SEQUENCE {
   supportedSubcarrierSpacingDL               SubcarrierSpacing,
   supportedBandwidthDL                       SupportedBandwidth,
   channelBW-90mhz                            ENUMERATED {supported} OPTIONAL,
   maxNumberMIMO-LayersPDSCH                  MIMO-LayersDL OPTIONAL,
   supportedModulationOrderDL                 ModulationOrder OPTIONAL
}
```
This field defines the supported **downlink (DL) sub-carrier spacing** by the UE. It describes the UE’s capabilities in Carrier Aggregation (CA) scenarios involving multiple numerologies.

### Key Points

- **General Definition**:
  - Indicates the sub-carrier spacing(s) supported by the UE for DL.
  - Allows simultaneous reception of carriers with **same or different numerologies**.

- **Mandatory Requirements**:
  - UE **shall support all mandatory sub-carrier spacing** values for:
    - **FR1 (Frequency Range 1)**
    - **FR2 (Frequency Range 2)**

- **Intraband NR-CA** (Same band, both contiguous and non-contiguous):
  - **Same numerology is mandatory**.
  - Support required in both **FR1 and FR2**.

- **Inter-band NR-CA** (Different bands: FR1 + FR2):
  - Support for **two mixed numerologies in DL** is **mandatory** **if** UE supports inter-band CA.

- **Optional Support**:
  - In other cases not mentioned above, supporting different numerologies is **optional**.

### Capability Summary

| Scenario                        | Numerology Support | Mandatory | Notes                              |
|---------------------------------|---------------------|-----------|-----------------------------------|
| All FR1/FR2 bands               | All mandated SCS    | ✅        | Baseline requirement             |
| Intraband CA (contiguous/non)   | Same numerology     | ✅        | Applies to both FR1 and FR2      |
| Inter-band CA (FR1 + FR2)       | Mixed numerologies  | ✅        | Only if inter-band CA supported  |
| Other configurations            | Varies              | ❌        | Optional support                 |


## 5G/NR - Waveform
### 📡 OFDM Overview – Orthogonal Frequency Division Multiplexing
OFDM stands for Orthogonal Frequency Division Multiplexing. To understand OFDM, it helps to break down its three core components:

- **Orthogonal** – Subcarriers are mathematically orthogonal, meaning they don’t  with each other.
- **Frequency Division** – A wide frequency band is split into many narrow sub-bands.
- **Multiplexing** – Data is transmitted simultaneously across all subcarriers.

<pr>
<img width="1005" height="657" alt="image" src="https://github.com/user-attachments/assets/8c31868c-e505-4f2c-a148-06f246c4a539" />
<pr>


### Orthogonality
Orthogonality ensures that each subcarrier does not interfere with others, even though they are closely spaced.

- At the sampling points of one subcarrier, all other subcarriers have zero amplitude.
- This allows dense packing of subcarriers without mutual interference.

### Frequency Drift & Challenges

**Key Summary**

- **OFDM efficiency** depends on **precise subcarrier spacing**.
- **Frequency drift** (e.g., from Doppler effect or channel fading) can **disrupt subcarrier alignment** and **break orthogonality**.
- **Design consideration**: account for **tolerable frequency drift** to maintain performance and **minimize inter-carrier interference (ICI)**.

**frequency drift :**
<img width="1143" height="737" alt="image" src="https://github.com/user-attachments/assets/ee91b7f3-9884-496d-8af6-82113762ef88" />


### Inter Symbol Interference (ISI) in the time domain
Two consecutive OFDM symbols are ideally transmitted without issue.\
  However, if the **first symbol is slightly delayed**, its **tail may overlap** with the next symbol.\
  This type of interference between different symbols is called **Inter Symbol Interference (ISI)**.

<pr>
<img width="783" height="247" alt="image" src="https://github.com/user-attachments/assets/9a6061e8-b884-4c1a-b698-8ad501a1f561" />
<pr>

### SA
<img width="836" height="802" alt="image" src="https://github.com/user-attachments/assets/10ac9b27-002f-42e8-9470-6d81d73b6723" />

### SSB
# 📡 SS Block（同步訊號區塊）簡介

## 🔍 它的用途是什麼？
SS Block（SSB）主要用來建立**下行同步**，是 UE 接入 5G NR 網路的**第一步**。

---

## 🧩 SSB 的組成元件

| 元件 | 說明 |
|------|------|
| **PSS** (Primary Synchronization Signal) | 主要同步訊號 |
| **SSS** (Secondary Synchronization Signal) | 次要同步訊號 |
| **PBCH** (Physical Broadcast Channel) | 實體廣播通道 |

這三個部分共同構成 **SSB**，並作為一個整體在無線框架中傳送。

---

## 📐 頻域中的位置
- SSB 在頻域中的位置是**可配置的**，不一定要位於中心頻率。  
- 這種設計提供了**頻率配置的彈性**，特別適合**多頻段部署**。

---

## ⏱️ 傳送週期（Periodicity）
- 可選週期：`5ms`、`10ms`、`20ms`、`40ms`、`80ms`、`160ms`  
- **最常見配置為 `20ms`**（即每 20 毫秒傳送一次 SSB）

---

## 🔢 最多連續 SSB 數量

| 頻段 | 最大連續 SSB 數量 |
|------|------------------|
| **FR1**（Sub-6GHz） | 最多 4 或 8 個 |
| **FR2**（mmWave） | 最多 64 個 |

---

> [!tip]
>  🎯 **連續 SSB 的主要用途**
> 用於 **波束管理（Beam Management）**
> 每個 SSB 可以透過**不同波束方向**傳送
> 協助 UE **尋找最佳接收方向**

簡易圖 :
<pr>
<img width="622" height="255" alt="image" src="https://github.com/user-attachments/assets/a8b55e94-46c1-4e0d-91ff-ae52d2c55411" />
<pr>
---

## 📘 RRC 配置參數

| 參數名稱 | 說明 |
|----------|------|
| `ssb-PositionsInBurst` | 指定 SSB 在 burst 中的位置與 bitmap |
| `ssb-periodicityServingCell` | 指定 SSB 傳送週期（如 `ms20`） |

這些參數可在 RRC 訊息中配置，例如：
- `SIB1`（適用於 SA 模式）
- `RRCConnectionReconfiguration`（適用於 NSA 模式）


### 📡 SSB 傳送模式與 ssb-PositionInBurst 的用途
上方圖示所示的 SSB 傳送是「全部 SSB 都傳送」的情況，但實際上並不需要傳送所有 SSB。\根據網路的需求，gNB 可以選擇性地傳送部分 SSB，並透過 RRC 訊息告知 UE 哪些 SSB 有傳送、哪些沒有。



#### 每個 Bitmap 的位元代表一個 SSB 的傳送狀態：

第一個位元 → SSB#0

第二個位元 → SSB#1

## 主要差異：週期性（Periodicity）
- 在 LTE 中：

    - PSS/SSS（同步訊號） 每 5 毫秒傳送一次

    - PBCH（實體廣播通道） 每 10 毫秒傳送一次

→ 同步訊號與廣播訊號的週期不同

- 在 NR 中：

    - SS（PSS + SSS）與 PBCH 是一起傳送的，週期完全一致

→ NR 中的 SS Block（SSB）是同步訊號與 PBCH 的整合傳送
