# Problem
Blew are the pics in TMA - cell search ， I want to know what do the parameters mean ? 

<br>

<img width="1416" height="717" alt="螢幕擷取畫面 2025-07-20 134239" src="https://github.com/user-attachments/assets/abf18574-91a6-4813-9039-cb9143506b55" />

<br>

# Q / A

|            :**Parameter Name**:          |                                          :**Description**:                               |      :**Affected Entity**:    |                          :**Note**:                           |
|:----------------------------------------:|:----------------------------------------------------------------------------------------:|:-----------------------------:|:-------------------------------------------------------------:|
| **Downlink carrier frequency** `37497`   | Downlink carrier frequency (MHz); UE must scan this frequency to sync and connect to gNB |      gNB & UE                 | Starting point for UE scanning and sync                       |
| **System bandwidth** `100`               | System bandwidth (MHz); defines available RBs                                            |      gNB & UE                 | Larger bandwidth → higher throughput but harder sync          |
| **AbsoluteFrequencyPointA** `37005.6`    | Basic reference frequency PointA in NR FR1/FR2 spectrum                                  |         UE                    | UE uses this to calculate PRB, SSB, and frequency offsets     |
| **AbsoluteFrequencySSB** `37084.8`       | Actual frequency of the SSB (Synchronization Signal Block)                               |      gNB & UE                 | UE searches SSB for initial synchronization                   |
| **SSB Subcarrier Spacing** `1`           | SSB subcarrier spacing (0=15kHz, 1=30kHz, ...)                                           |      gNB & UE                 | Affects UE's resolution in sync signal detection              |
| **Cell ID** `0`                          | NR Cell Identity; used for cell identification                                           |         UE                    | Used to identify gNB after synchronization                    |
| **Number of receive antennas** `4`       | Number of receive antennas at gNB                                                        |         gNB                   | Affects MIMO capability and link performance                  |
| **Uplink carrier frequency** *(not set)* | Uplink carrier frequency                                                                 |    UE (if uplink is required) | If different from DL, UE needs config for uplink transmission |


## Downlink carrier frequency & System bandwidth 


