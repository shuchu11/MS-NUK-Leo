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

To reduce this issue, we copy a part of signal from the end and paste it into this gap. This copied portion prepended at the beginning is called **'Cyclic Prefix'**.
<img width="868" height="363" alt="image" src="https://github.com/user-attachments/assets/72ab3ef5-5d27-49d3-9644-76714561ed42" />

### SA
<img width="836" height="802" alt="image" src="https://github.com/user-attachments/assets/10ac9b27-002f-42e8-9470-6d81d73b6723" />

### SSB (Synchronization Signal Block) Overview

> [!tip]
> 🎯 **Main Purpose of Consecutive SSBs**  
> Used for **Beam Management**  
> Each SSB can be transmitted in a **different beam direction**  
> Helps the UE **find the optimal reception direction**

簡易圖 :
<pr>
<img width="622" height="255" alt="image" src="https://github.com/user-attachments/assets/a8b55e94-46c1-4e0d-91ff-ae52d2c55411" />
<pr>
---

#### 🔍 What Is Its Purpose?
The **SS Block (SSB)** is mainly used to establish **downlink synchronization** and is the **first step** for the UE to access a 5G NR network.

---

#### 🧩 Components of the SSB

| Component | Description |
|----------|-------------|
| **PSS** (Primary Synchronization Signal) | Provides primary synchronization |
| **SSS** (Secondary Synchronization Signal) | Provides secondary synchronization |
| **PBCH** (Physical Broadcast Channel) | Transmits the Master Information Block (MIB) |

These three components form the **SSB** and are transmitted as a whole within the radio frame.

---

#### 📐 Frequency Domain Location
- The SSB’s position in the frequency domain is **configurable** and does not need to be centered.
- This design enables **flexible frequency planning**, especially suitable for **multi-band deployments**.

---

#### ⏱️ Transmission Periodicity
- Available periodicities: `5ms`, `10ms`, `20ms`, `40ms`, `80ms`, `160ms`
- The **most common configuration is `20ms`**, meaning the SSB is transmitted every 20 milliseconds.

---

#### 🔢 Maximum Number of Consecutive SSBs

| Frequency Range | Max Consecutive SSBs |
|-----------------|----------------------|
| **FR1** (Sub-6GHz) | Up to 4 or 8 |
| **FR2** (mmWave)   | Up to 64 |

---

#### 📘 RRC Configuration Parameters

| Parameter | Description |
|-----------|-------------|
| `ssb-PositionsInBurst` | Indicates the position and bitmap of SSBs within a burst |
| `ssb-periodicityServingCell` | Defines the periodicity of SSB transmission (e.g., `ms20`) |

These parameters are included in RRC messages such as:
- `SIB1` (for SA mode)
- `RRCConnectionReconfiguration` (for NSA mode)

---

#### 📡 SSB Transmission Mode & Purpose of `ssb-PositionsInBurst`
In the diagram above, all SSBs are shown as transmitted, but in practice, not all SSBs need to be sent.  
Depending on network needs, the **gNB can selectively transmit a subset of SSBs** and inform the UE via RRC signaling which SSBs are active or inactive.

---

#### 📊 Bit Mapping in the Bitmap Field

Each bit in the bitmap indicates the transmission status of a specific SSB:

- First bit → SSB#0  
- Second bit → SSB#1  
- And so on...

---

#### 🔄 Key Difference: Periodicity in LTE vs NR

- **In LTE**:
  - **PSS/SSS (sync signals)**: transmitted every **5 ms**
  - **PBCH (broadcast channel)**: transmitted every **10 ms**
  → **Sync and broadcast signals have different periodicities**

- **In NR**:
  - **SS (PSS + SSS)** and **PBCH** are transmitted **together** with the **same periodicity**
  → In NR, the **SS Block (SSB)** integrates both sync signals and PBCH into a **unified transmission block**


## MIB / SIB1

<img width="786" height="415" alt="image" src="https://github.com/user-attachments/assets/62a6b92d-2769-44ca-9f66-3b80a1605630" />

## 📡 MIB and SIB1 in 5G NR Standalone (SA) Mode

In 5G NR **Standalone (SA)** mode, the most critical signals for the UE before attempting to connect are **MIB** and **SIB1**.  
To **camp on a cell**, the UE must at minimum be able to successfully decode both MIB and SIB1.

---

## 🔗 How MIB and SIB1 Are Transmitted

- **MIB (Master Information Block)** is transmitted via the physical channel **PBCH**, which is part of the **SSB** (Synchronization Signal Block).
- **SIB1 (System Information Block Type 1)** is transmitted via the physical channel **PDSCH**.

---

## 🧩 Contents and Functions of MIB and SIB1

### MIB Includes:
- Reference **subcarrier spacing**
- **Control resource configuration** (for decoding SIB1 on PDSCH)
- **DMRS position** (downlink reference signal)
- Whether **cell camping is allowed** (`cellBarred`)
- Whether **intra-frequency reselection** is allowed (`intraFreqReselection`)

### SIB1 Includes:
- Essential information for UE to perform **initial attachment**, at least up to **RRC Setup**
- **Scheduling info** for other SIBs (e.g., periodic or on-demand transmission)
- Whether **On-Demand SIB** is supported (UE can request specific SIBs)

---

## 📘 Number of SIBs Defined by 3GPP

- As of now, **3GPP defines 21 SIBs (System Information Blocks)**
- The number may increase as the specification evolves

