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









