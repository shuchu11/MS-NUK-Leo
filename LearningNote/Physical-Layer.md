# Problem
- Blew are the pics in TMA - cell search ， I want to know what do the parameters mean ? 

<br>

<img width="1416" height="717" alt="螢幕擷取畫面 2025-07-20 134239" src="https://github.com/user-attachments/assets/abf18574-91a6-4813-9039-cb9143506b55" />

<br>



- Here is Parameter Help
 
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


> [!NOTE]
> Useful information that users should know, even when skimming content.
>
>**Downlink carrier frequency** `37497`kHz  ---->   **FR1-n78** (Table 1)
>
>**System bandwidth** `100` kHz  ---->  Channel bandwidth compliant with n78 (Table 2)




*  **Table 1**

  
| Category | Abbreviation      | Frequency Range        | Description                                                                                                                           |
|:--------:|:-----------------:|:----------------------:|:-------------------------------------------------------------------------------------------------------------------------------------:|
| **FR1**  | Frequency Range 1 | **410 MHz – 7125 MHz** | Also known as **Sub-6 GHz**, covering most early commercial 5G bands; strong penetration, suitable for wide-area coverage.            |
| **FR2**  | Frequency Range 2 | **24.25 GHz – 71 GHz** | Also known as **millimeter wave (mmWave)**; offers large bandwidth and high speed but has weak penetration and limited coverage area. |

* **Table 2** \
ref 1 : https://5g-tools.com/5g-nr-gscn-calculator/   \
ref 2: https://zh.wikipedia.org/zh-tw/5G_NR%E9%A2%91%E6%AE%B5 \
The table below lists the frequency bands and channel bandwidths of the 5G NR standard in the latest released 3GPP technical specification TS 38.101, Release 18.

| **NR operating band** | **SS Block SCS** | **SS Block pattern** | **Range of GSCN** *(First–'Stepsize'–Last)*      |         Channel Bandwidth (MHz)                  |
|:---------------------:|:----------------:|:--------------------:|:------------------------------------------------:|:------------------------------------------------:|
| n76                   | 15 kHz           | Case A                | 3572 – <1> – 3574                               |                   5                              |
| n77                   | 30 kHz           | Case C                | 7711 – <1> – 8329                               | 10, 15, 20, 25, 30, 40, 50, 60, 70, 80, 90, 100  |
| n78                   | 30 kHz           | Case C                | 7711 – <1> – 8051                               |  10, 15, 20, 25, 30, 40, 50, 60, 70, 80, 90, 100 |
| n79                   | 30 kHz           | Case C                | 8480 – <16> – 8880<sup>7</sup>                  | 	10, 20, 30, 40, 50, 60, 70, 80, 90, 100         |
|                       |                  |                       | 8475 – <1> – 8884<sup>8</sup>                   |                      //                          |

## point A 
Tool 1 : [5G NR SSref (SSB) Frequency to GSCN Calculator](https://5g-tools.com/5g-nr-gscn-calculator/) \
Tool 2 : [NR Reference Point A](https://www.sqimway.com/nr_refA.php) \


- **Downlink carrier frequency** `3708.48`MHz  ------> 5G NR ARFCN `649980`MHz
<pr>
<img width="1140" height="281" alt="image" src="https://github.com/user-attachments/assets/e3aa25f8-9533-46a6-9f9a-d66bb312c61b" />
<pr>
<img width="1467" height="865" alt="image" src="https://github.com/user-attachments/assets/a0e7470e-9a89-4355-90cc-92be1ae3fb69" />
<pr>

As the result, you can find this Point A `3708.48 MHz` compliant to the list.  

<pr>
<img width="1387" height="877" alt="image" src="https://github.com/user-attachments/assets/87e979a1-c9d2-4478-8ba5-7f9f5a7c87d6" />
<pr>
<img width="930" height="870" alt="image" src="https://github.com/user-attachments/assets/7c9e6592-fe2f-41d3-bd79-9d52dc4ccbd8" />
<pr>

# Config NAS PLMN Slection 

<pr>
<img width="1697" height="711" alt="螢幕擷取畫面 2025-08-01 155126" src="https://github.com/user-attachments/assets/d2fddd39-4002-4c0f-9e3c-e171ee4bd8b9" />
<pr>

| 參數名稱                                                | 說明                                                        |
|:---------------------------------------------------:|:--------------------------------------------------------------:|
| **NrNasAptConfigCapability**                        | 主設定項目（Required），代表UE的NR NAS能力配置                     |
| **Nas Release Version**                             | NAS協議版本，通常從0開始對應3GPP版本                              |
| **NAS Security options**                            | NAS安全選項（例如完整性保護、加密等）                              |
| **Supported Ciphering Algorithms**                  | 支援的NAS加密演算法（如 0xe0 表示128-NEA0, NEA1, NEA2, NEA3）     |
| **Supported Integrity Algorithms**                  | 支援的完整性保護演算法（如 0xe0 表示128-NIA0, NIA1, NIA2, NIA3）   |
| **Preferred RAT**                                   | 偏好的無線接入技術（Radio Access Technology），1 通常代表 NR（5G）  |
| **Drx Parameters**                                  | 不連續接收（Discontinuous Reception）參數                         |
| **Reflective QoS**                                  | 是否啟用 Reflective QoS 功能                                      |
| **Uplink Integrity protection maximum data rate**   | 使用者面上行完整性保護的最大速率（0 = 64 kbps，1 = Full Data Rate） |
| **Downlink Integrity protection maximum data rate** | 使用者面下行完整性保護的最大速率（0 = 64 kbps，1 = Full Data Rate） |
| **Reserved**                                        | 保留欄位，通常不啟用                                              |


- **加密與完整性保護演算法對應（Mask 說明）**
0xe0` = 支援所有 128-bit 安全演算法

 - Ciphering: NEA0, NEA1, NEA2, NEA3
 - Integrity: NIA0, NIA1, NIA2, NIA3

- **Command Preview 範例**
範例配置生成的 MTE 指令如下：
```
FORW MTE NRNASAPTCONFIGCAPABILITY 0 [] [0xe0] [0xe0] [] [] [] [1] [1] [] []
```
這部分設定屬於 5G UE 的 NAS（Non-Access Stratum）能力配置，主要用來定義 UE 在註冊到網路時通報的安全與協定功能，包括 支援的加密與完整性演算法（如 NEA/NIA 系列）、NAS 協定版本、使用者平面的完整性保護速率（上/下行），以及是否啟用 Reflective QoS 與 DRX（省電接收）參數。這些設定將會影響 UE 與核心網（如 AMF）之間的訊息處理與安全機制，並且透過底部的 Command Preview 生成實際指令發送給模擬或測試平台（如 TM500 或 OAI UE 模擬器）。掌握這些參數有助於測試各種網路場景下的註冊、保安與 QoS 行為。

# Config NR NAS Capabilities
<pr>
<img width="1746" height="823" alt="image" src="https://github.com/user-attachments/assets/22c28d9f-eec6-47d1-9899-159877115449" />
<pr>


