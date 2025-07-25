# Problem
- Blew are the pics in TMA - cell search ， I want to know what do the parameters mean ? 

<br>

<img width="1416" height="717" alt="螢幕擷取畫面 2025-07-20 134239" src="https://github.com/user-attachments/assets/abf18574-91a6-4813-9039-cb9143506b55" />

<br>



- Here is Parameter Help
- 
: You may notice that the frequency **unit in the table is 100 kHz**, rather than MHz.
<br>

<img width="787" height="636" alt="image" src="https://github.com/user-attachments/assets/2e1fb008-0eb0-4d6d-a882-a12c67496896" />

<br>

# Q / A
 
|             **Parameter Name**           |                                           **Description**                                |       **Affected Entity**     |                           **Note**                            |
|:----------------------------------------:|:----------------------------------------------------------------------------------------:|:-----------------------------:|:-------------------------------------------------------------:|
| **Downlink carrier frequency** `37497`   | Downlink carrier frequency (MHz); UE must scan this frequency to sync and connect to gNB |      gNB & UE                 | Starting point for UE scanning and sync                       |
| **System bandwidth** `100`               | System bandwidth (MHz); defines available RBs                                            |      gNB & UE                 | Larger bandwidth → higher throughput but harder sync          |
| **AbsoluteFrequencyPointA** `37005.6`    | Basic reference frequency PointA in NR FR1/FR2 spectrum                                  |         UE                    | UE uses this to calculate PRB, SSB, and frequency offsets     |
| **AbsoluteFrequencySSB** `37084.8`       | Actual frequency of the SSB (Synchronization Signal Block)                               |      gNB & UE                 | UE searches SSB for initial synchronization                   |
| **SSB Subcarrier Spacing** `1`           | SSB subcarrier spacing (0=15kHz, 1=30kHz, ...)                                           |      gNB & UE                 | Affects UE's resolution in sync signal detection              |
| **Cell ID** `0`                          | NR Cell Identity; used for cell identification                                           |         UE                    | Used to identify gNB after synchronization                    |
| **Number of receive antennas** `4`       | Number of receive antennas at gNB                                                        |         gNB                   | Affects MIMO capability and link performance                  |


## Downlink carrier frequency & System bandwidth 

**Downlink carrier frequency** `37497`kHz  ---->   **FR1-n78** (Table 1)

**System bandwidth** `100`   ---->  Channel bandwidth compliant with n78 (Table 2)




*  **Table 1**
| Category | Abbreviation      | Frequency Range        | Description                                                                                                                           |
|:--------:|:-----------------:|:----------------------:|:------------------------------------------------------------------------------------------------------------------------------------- |
| **FR1**  | Frequency Range 1 | **410 MHz – 7125 MHz** | Also known as **Sub-6 GHz**, covering most early commercial 5G bands; strong penetration, suitable for wide-area coverage.            |
| **FR2**  | Frequency Range 2 | **24.25 GHz – 71 GHz** | Also known as **millimeter wave (mmWave)**; offers large bandwidth and high speed but has weak penetration and limited coverage area. |

* **Table 2**
ref 1 : https://5g-tools.com/5g-nr-gscn-calculator/   \
ref 2: https://zh.wikipedia.org/zh-tw/5G_NR%E9%A2%91%E6%AE%B5 \
The table below lists the frequency bands and channel bandwidths of the 5G NR standard in the latest released 3GPP technical specification TS 38.101, Release 18.

| **NR operating band** | **SS Block SCS** | **SS Block pattern** | **Range of GSCN** *(First–<Stepsize>–Last)*      |         Channel Bandwidth (MHz)                  |
|:---------------------:|:----------------:|:--------------------:|:------------------------------------------------:|:------------------------------------------------:|
| n76                   | 15 kHz           | Case A                | 3572 – <1> – 3574                               |                   5                              |
| n77                   | 30 kHz           | Case C                | 7711 – <1> – 8329                               | 10, 15, 20, 25, 30, 40, 50, 60, 70, 80, 90, 100  |
| n78                   | 30 kHz           | Case C                | 7711 – <1> – 8051                               |  10, 15, 20, 25, 30, 40, 50, 60, 70, 80, 90, 100 |
| n79                   | 30 kHz           | Case C                | 8480 – <16> – 8880<sup>7</sup>                  | 	10, 20, 30, 40, 50, 60, 70, 80, 90, 100         |
|                       |                  |                       | 8475 – <1> – 8884<sup>8</sup>                   |                      //                          |



