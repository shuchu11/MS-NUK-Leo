# Frame Structure
Unlike LTE’s fixed frame design, **5G NR adapts dynamically** to each service’s requirements.\
This flexibility allows the network to optimize performance for both high-throughput and ultra-low-latency use cases.

🧱 **Time-Frequency Resource Allocation**
The time-frequency domain is divided into:

Resource Blocks (RBs): 12 subcarriers in frequency

OFDM Symbols: units in time

Together they form the resource grid, the basic unit for scheduling and transmission.

This structure enables simultaneous support for:

High-bandwidth services (e.g., 4K streaming)

Low-latency applications (e.g., autonomous driving)

⏱️ Slot-Based Design
Each slot contains 14 OFDM symbols (normal CP)

Slot duration varies with numerology (μ):

Higher μ → larger subcarrier spacing → shorter slot duration

# 📐 Hierarchy of 5G Frame Structure (Same as LTE)
Resource Grid : A 2D grid formed by **frequency domin**(RBs) and **time domin**(OFDM symbols)
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
| **RB Duration**	  |   One RB spans **12 subcarriers × 14 OFDM symbols** in one slot            |
| **Resource Block**| Each RB contains **12 subcarriers** (same as LTE)                          |
