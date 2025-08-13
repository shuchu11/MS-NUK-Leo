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
>**Downlink carrier frequency** `37497` 100kHz  ---->   **FR1-n78** (Table 1)
>
>**System bandwidth** `100` 100kHz  ---->  Channel bandwidth compliant with n78 (Table 2)




*  **Table 1**

  
| Category | Abbreviation      | Frequency Range        | Description                                                                                                                           |
|:--------:|:-----------------:|:----------------------:|:-------------------------------------------------------------------------------------------------------------------------------------:|
| **FR1**  | Frequency Range 1 | **410 MHz – 7125 MHz** | Also known as **Sub-6 GHz**, covering most early commercial 5G bands; strong penetration, suitable for wide-area coverage.            |
| **FR2**  | Frequency Range 2 | **24.25 GHz – 71 GHz** | Also known as **millimeter wave (mmWave)**; offers large bandwidth and high speed but has weak penetration and limited coverage area. |

* **Table 2** \
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


- **Downlink carrier frequency** `3749.7`MHz  ------> 5G NR ARFCN `649980`MHz
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



### Parameter Help: Force PLMN

| Field     | Value  |
|-----------|--------|
| DataType  | String |
| Min       | 5      |
| Max       | 6      |
| Default   |        |

---

**Parameter:** Force PLMN  
**Description:**  
- Force use of a particular PLMN. Initial registration is attempted to the specified PLMN.  
- PLMN is provided in a string of **5 or 6 decimal digits**.  
- The **leading 3 digits** provide the **Mobile Country Code (MCC)**.  
- The **next 2 or 3 digits** (depending on *MNC length*) provide the **Mobile Network Code (MNC)**.




### Parameter Help: Radio Access Type


|   Field     |   Value  |
|:-----------:|:--------:|
| DataType    |   Enum   |
| Min         |   0      |
| Max         |   4      |
| Default     |   3      |

---

**Parameter:** Radio Access Type  
**Description:**  
- Values **0**, **1**, and **2** are reserved for future use.  
- **3** = NG-RAN  
- **4** = SATELLITE NG-RAN  
- **Note:** This parameter is **not used for LTE (4G)**.





# Config NR NAS Capabilities

<pr>
<img width="1746" height="823" alt="image" src="https://github.com/user-attachments/assets/22c28d9f-eec6-47d1-9899-159877115449" />
<pr>

## file introduction

| DataType | none |
|----------|------|
| **Min**  | none |
| **Max**  | none |
| **Default** | Not Applicable |

**NrNasAptConfigCapability**  

Configures the NAS capabilities, including authentication and security options.  

**SCOPE:**  
Activation time is controlled by the Activate command.  

**DESCRIPTION:**  
APT command to configure the NAS capabilities and the ciphering/integrity algorithms supported as reported to the network.  

**SYNTAX:** 
FORW MTE NRNASAPTCONFIGCAPABILITY

**TAG:** 
Not Applicable


## parameters

- **Parameter:** Nas Release Version
  
| DataType | Min | Max | Default |
| -------- | --- | --- | ------- |
| Int      | 0   | 2   | 0       |

- Sets the NAS layer 3GPP release version
    - 0 = Release 15,
    - 1 = Release 16,
    - 2 = Release 17

Note this provides a per-UE value overriding any default value or global value defined by a SETP.

- **Parameter:** NAS Security options

| DataType | Enum |
|----------|------|
| **Min**  | 0    |
| **Max**  | 4    |
| **Default** | 3 |

- **0** - No Authentication procedure and no NAS security mode procedure is expected (any received Authentication Request or Security Mode Command will be ignored)  
- **1** - No NAS security mode procedure is expected. Any received Security Mode Command will be ignored. Authentication procedure is expected.  
- **2** - NAS security mode is expected but message contents are ignored. Security mode procedure will complete but no NAS ciphering or integrity is started. Authentication procedure is expected. NAS security headers are not used.  
- **3** - NAS security mode is expected and UE will act on contents starting NAS integrity and ciphering if configured. Authentication procedure is expected. NAS security headers are used. This is normal 3GPP defined behaviour and is the default if the command is not used.  
- **4** - NAS security mode is expected. Authentication procedure is not expected.


- **Parameter:** Supported Ciphering Algorithms  

| DataType | Int  |
|----------|------|
| **Min**  | 0    |
| **Max**  | 0xFF |
| **Default** | 0xE0 |


A bitfield of 8 bits indicating the supported ciphering algorithm(s).  
The bit allocation is as follows, where setting the bit indicates that the algorithm is supported:  

| Bit Position | Algorithm                |
|--------------|--------------------------|
| Bit 7 (MSB)  | 5G-EA0 (no ciphering)     |
| Bit 6        | 5G-EA1 (SNOW 3G)          |
| Bit 5        | 5G-EA2 (AES)              |
| Bit 4        | 5G-EA3 (ZUC)              |
| Bit 3        | 5G-EA4 (reserved)         |
| Bit 2        | 5G-EA5 (reserved)         |
| Bit 1        | 5G-EA6 (reserved)         |
| Bit 0 (LSB)  | 5G-EA7 (reserved)         |


- **Parameter:** Supported Integrity Algorithms  

| DataType | Int  |
|----------|------|
| **Min**  | 0    |
| **Max**  | 0xFF |
| **Default** | 0xE0 |


A bitfield of 8 bits indicating the supported integrity algorithm(s).  
The bit allocation is as follows, where setting the bit indicates that the algorithm is supported:  

| Bit Position | Algorithm                 |
|--------------|---------------------------|
| Bit 7 (MSB)  | 5G-IA0 (Null integrity)    |
| Bit 6        | 5G-IA1 (SNOW 3G)           |
| Bit 5        | 5G-IA2 (AES)               |
| Bit 4        | 5G-IA3 (ZUC)               |
| Bit 3        | 5G-IA4 (reserved)          |
| Bit 2        | 5G-IA5 (reserved)          |
| Bit 1        | 5G-IA6 (reserved)          |
| Bit 0 (LSB)  | 5G-IA7 (reserved)          |

- **Parameter:** Preferred RAT  

| DataType | Int |
|----------|-----|
| **Min**  | 0   |
| **Max**  | 1   |
| **Default** | 1 |

Selects preferred RAT:  
- **0** = LTE (S1 Mode)  
- **1** = NR (N1 Mode)  


- **Parameter:** Drx Parameters

| DataType | Enum |
|----------|------|
| **Min**  | 0    |
| **Max**  | 4    |
| **Default** | 0 |

Drx parameters to be negotiated. Indicates that the UE wants to use DRX and for the network to indicate the DRX cycle value to be used at paging.  

- **0** = Drx value not specified  
- **1** = Drx cycle parameter T = 32  
- **2** = Drx cycle parameter T = 64  
- **3** = Drx cycle parameter T = 128  
- **4** = Drx cycle parameter T = 256  

Corresponds to 3GPP spec 24.501 section 9.11.3.2A.  

- **Parameter:** Reflective QoS

| DataType | Enum |
|----------|------|
| **Min**  | 0    |
| **Max**  | 1    |
| **Default** | 0 |

**Parameter:** Reflective QoS  

Enables the Reflective QoS support.  
- **0** = Reflective QoS support disabled  
- **1** = Reflective QoS support enabled  

- **Parameter:** Downlink/Uplink Integrity protection maximum data rate  

| DataType | Enum |
|----------|------|
| Min      | 0    |
| Max      | 1    |
| Default  | 0    |

Specifies the maximum data rate per UE for user-plane integrity protection for downlink/uplink.  
0 = 64 kbps  
1 = Full Data rate  
If not provided or [], the default value of 64 kbps is used.

# NrPhyCalibrateUlPowerScaling   

<pr>
<img width="607" height="455" alt="image" src="https://github.com/user-attachments/assets/64b0da16-bdc8-42d8-b203-42b55844207e" />
<pr>
 
| DataType | none |
|----------|------|
| Min      | none |
| Max      | none |
| Default  | Not Applicable |

**Parameter:** NrPhyCalibrateUlPowerScaling  

Calibrate the UL Power Scaling.  

**SCOPE:**  
This command is available in all modes (PDCP_MODE, NAS_MODE, MTS_MODE). \ 
The effects of this command are immediate.  

**DESCRIPTION:**  
This command is used to calibrate the UL power scaling to be applied to the uplink physical channels as an alternative to using the `NrPhyConfigUlPowerScalingCommon` command.\  
Refer to `NrPhyConfigUlPowerScalingCommon` command description for more details of the operation of the UL power scaling.\  
`NrSetMueRadioContextCell` must be called for the radio context to be calibrated before using this command.  \
The command takes the radio context as a parameter and calibrates the power scaling value for the associated cell based on the measured path loss between the gNB and the TM500.\  
The value calculated by this command is applied to all the UL power scaling values for the specified radio context.\  
The command can be repeated as necessary to calibrate against all the required radio contexts.\  
The calibrated results are provided in the Calibrated Power Scaling indication, with a separate indication being provided for each Radio Context.\  
The calibration formula is as follows:\
Calibrated UL Power Scaling = DLRS Power - RSRP + Delta UL DL Physical Attenuation  

Possible Failure messages:
- "Unable to calibrate ul power scaling" if any of the individual parameter ranges are out of range or if the calibrated power scaling value obtained is out of range.  
- "Reference signal power must not be specified for NAS/MTS modes" if Signalled SSB Reference Signal Power is provided as optional parameter in command for NAS/MTS mode.  
- "Reference signal power must be specified as optional parameter for PDCP mode" if Signalled SSB Reference Signal Power is not provided as optional parameter for PDCP mode.  
- Radio context value provided in radio context is out of range or if the radio context has not been configured.  

Possible warning messages:
- Delta UL DL Physical Attenuation should only be provided if you are using radio card in dedicated port configuration; otherwise, the value will be ignored.  
- TM500 will generate the following warning and not fail the command, instead using a default value of 0 for computation:  
I: CMPI L1 WARNING: Ignoring Delta UL DL Physical Attenuation value for Radio Context <N>.\
This value is only applicable when using radio module in dedicated port configuration\
where the value of N can range from 0 to 47.  

- If FixedSsb Calibration mode is selected and CRC is failing for Fixed SSB index provided by the user in the command, TM500 will generate the following warning and not fail the command.  
It will fallback to using the firstSsb calibration mode:  
I: CMPI L1 NR Calibrate Power Scaling WARN: For Fixed SsblId CRC failed, Calibration is defaulted using first SSB (i.e., default behaviour)

**SYNTAX:**  
FORW MTE NRPHYCALIBRATEULPOWERSCALING  

**TAG:**  
Not Applicable

## parameters

- **Parameter:** Radio Context

| DataType | Int |
|----------|-----|
| Min      | 0   |
| Max      | 47  |
| Default  |     |

By default, all radio context will be selected.  
Radio context for which calibration of UL power scaling value is required.  
Calibration for multiple radio contexts can be performed using the ALL option.

- **Parameter:** Signalled SSB Reference Signal Power

| DataType | Int |
|----------|-----|
| Min      | -60 |
| Max      | 50  |
| Default  |     |

Signalled SSB Reference Signal Power (ss-PBCH-BlockPower) in dBm.  
This parameter must be provided in PDCP_MODE.  
In other modes, a value must not be provided and the cell power is read by RRC from SIB1.

- **Parameter:** Delta UL DL Physical Attenuation

| DataType | Int |
|----------|-----|
| Min      | -30 |
| Max      | 30  |
| Default  | 0   |

Attenuation difference between UL and DL paths in dB.  
This parameter is applicable when using a dedicated configuration wherein the physical attenuation applied in UL and DL can be different.  
If the physical attenuation is different in UL and DL, the user must provide the delta using the formula:  
Delta UL DL Physical Attenuation = UL Physical Attenuation - DL Physical Attenuation


- **Parameter:** RSRP Measurement Mode

| DataType | Enum |
|----------|------|
| Min      | 0    |
| Max      | 1    |
| Default  |      |

Flag to select which DL RSRP measurement mode to use:  
- `0` = Use maximum measured RSRP across all DL receive antennas (Default).  
- `1` = Use measured RSRP averaged across DL antennas 1 and 2.


# NrPhyConfigUlPowerScalingCommon  

<pr>
<img width="605" height="367" alt="image" src="https://github.com/user-attachments/assets/40668db4-a9f0-4c92-ba6b-e10238e90b46" />
<pr>

| DataType | none |
|----------|------|
| Min      | none |
| Max      | none |
| Default  | Not Applicable |

**Parameter:** NrPhyConfigUlPowerScalingCommon  

Configure the cell common parameters for UL power scaling.  

**SCOPE:**  
This command is available in all modes (PDCP_MODE, NAS_MODE, MTS_MODE).  
The effects of this command are immediate.  

**DESCRIPTION:**  
This command is used to configure the cell common parameters for UL power scaling.  

**SYNTAX:**  
FORW MTE NRPHYCONFIGULPOWERSCALINGCOMMON

> [!TIP]
> Ensure that the UE can be correctly received by the gNB.
During the initial access, the UE must transmit uplink signals during the random access (PRACH) procedure and the subsequent RRC connection establishment process.
If the uplink power is too low, the gNB may fail to detect the signal or misinterpret it; if it is too high, it may cause interference or violate power regulations.
The common parameters configured by NrPhyConfigUlPowerScalingCommon ensure that different UEs transmit within a reasonable power range, thereby improving the success rate of access.

## parameters

- **Parameter:** Mode
 
| DataType | Enum |
|----------|------|
| Min      | 0    |
| Max      | 1    |
| Default  |      |

Specifies the mode of UL power scaling that is applied.  
- `0` = Automatic  
- `1` = Semi-Automatic

- **Parameter:** Global Offset

| DataType | Int   |
|----------|-------|
| Min      | -1000 |
| Max      | 1000  |
| Default  | 0     |

Power scaling offset applied to all the channels.  
Range: [-100, 100] dB in 0.1 dB steps.

- **Parameter:** Radio Context  

| DataType | Enum |
|----------|------|
| Min      | 0    |
| Max      | 47   |
| Default  |      |


By default all radio context will be selected  
The Radio context with which the power scaling common parameters are associated.  
**Note:** The Radio Context specified here should be part of the cell for which the common parameters are being specified.  

- **Parameter:** Global Offset for PUSCH  

| DataType | Int   |
|----------|-------|
| Min      | -1000 |
| Max      | 1000  |
| Default  | 0     |

Power scaling offset applied to PUSCH channel. The offset value applied for PUSCH is this value plus the value specified in 'Global Offset' parameter for backward compatibility.  
**Range:** [-100, 100] dB in 0.1 dB steps.  

- **Parameter:** Global Offset for PUCCH  

| DataType | Int   |
|----------|-------|
| Min      | -1000 |
| Max      | 1000  |
| Default  | 0     |

Power scaling offset applied to PUCCH channel. The offset value applied for PUCCH is this value plus the value specified in 'Global Offset' parameter for backward compatibility.  
**Range:** [-100, 100] dB in 0.1 dB steps.  


- **Parameter:** Global Offset for PRACH

| DataType | Int   |
|----------|-------|
| Min      | -1000 |
| Max      | 1000  |
| Default  | 0     |

Power scaling offset applied to PRACH channel. The offset value applied for PRACH is this value plus the value specified in 'Global Offset' parameter for backward compatibility.  
**Range:** [-100, 100] dB in 0.1 dB steps.  

- **Parameter:** Global Offset for SRS

| DataType | Int   |
|----------|-------|
| Min      | -1000 |
| Max      | 1000  |
| Default  | 0     |

Power scaling offset applied to SRS channel. The offset value applied for SRS is this value plus the value specified in 'Global Offset' parameter for backward compatibility.  
**Range:** [-100, 100] dB in 0.1 dB steps.  

- **Parameter:** UL Carrier Type

| DataType | Enum |
|----------|------|
| Min      | 0    |
| Max      | 2    |
| Default  | 0    |

The UL carrier type (of the above mentioned radio context) with which the power scaling common parameters are associated.  
- `0` = apply the parameters to non-supplementary UL carrier.  
- `1` = apply the parameters to supplementary UL carrier. All related radio context should support SUL.  
- `2` = apply the parameters to non-supplementary and supplementary UL carrier. All related radio context should support SUL.  



# Select UE0

<img width="706" height="257" alt="image" src="https://github.com/user-attachments/assets/52f2daf4-97d3-4e62-a041-27c065091a67" />

# Configure USIM

<img width="830" height="575" alt="image" src="https://github.com/user-attachments/assets/7349881e-ef6a-4d25-a24e-1ac0c783f854" />
<img width="832" height="382" alt="image" src="https://github.com/user-attachments/assets/4cf3086f-424f-4836-82dc-fafeabded596" />


| DataType | none           |
|----------|----------------|
| Min      | none           |
| Max      | none           |
| Default  | Not Applicable |

**Parameter:** UsimConfig  

Configures the USIM.  

**SCOPE:**  
Activation time is controlled by the Activate command. The command is only valid if the UE is not registered to the network.  
The command can only be issued once after STRT.  

**DESCRIPTION:**  
Configures a real or simulated USIM.  

**SYNTAX:**  
FORW MTE USIMCONFIG  

**TAG:**  
Not Applicable  

> [!tip]
> -  Importance in SA Access Procedure
> 
>     -  Prerequisite for Authentication
> - USIM configuration is the prerequisite for the UE to register with the network and establish a secure connection, containing information such as IMSI and > > > authentication keys.  
> - If the USIM is not correctly configured, the UE will not be able to pass the network authentication step after completing cell search, leading to access failure.
> 
>     -  Impact on Subsequent Registration and Service Capability
>         - **UsimConfig** determines whether the UE uses a real or simulated USIM, directly affecting the test scenario and access behavior.  
>         - The configuration affects the PLMN and APN lists the UE can register with, determining the feasibility of subsequent data and voice services.
> 
> - Relation to Cell Search
>     - Cell search is the first step for the UE to access the network (synchronization and obtaining MIB/SIB).  
>     - While **UsimConfig** does not participate in the radio layer search and synchronization, it determines whether the UE can initiate NAS layer registration after > finding the target cell.  
>     - Without correct USIM configuration, even if cell search succeeds and system information is read, the UE cannot complete the subsequent random access and > registration procedure.  



## parameters

- 1.**Parameter:** USIM Type (Conditional)

| DataType | Choice |
|----------|--------|
| Min      | 0      |
| Max      | 1      |
| Default  |        |

- `0` = Real  
- `1` = Simulated  

*Note:* Real USIM is only supported for UE 0.  


- 1.1. **Parameter:** USIM Type_EQUAL_TO_1  

| DataType | Union         |
|----------|---------------|
| Min      | 1             |
| Max      | 1             |
| Default  | Not Applicable |

Configures the USIM.  

**SCOPE:**  
Activation time is controlled by the Activate command. The command is only valid if the UE is not registered to the network.  
The command can only be issued once after STRT.  

**DESCRIPTION:**  
Configures a real or simulated USIM.  

**SYNTAX:**  
FORW MTE USIMCONFIG  

**TAG:**  
Not Applicable  


- 1.1.1.**Parameter:** IMSI (Optional)   
| DataType | Bool          |
|----------|--------------|
| Min      | 1            |
| Max      | 1            |
| Default  | Not Applicable |

Configures the USIM.  

**SCOPE:**  
Activation time is controlled by the Activate command. The command is only valid if the UE is not registered to the network.  
The command can only be issued once after STRT.  

**DESCRIPTION:**  
Configures a real or simulated USIM.  

**SYNTAX:**  
FORW MTE USIMCONFIG  

**TAG:**  
Not Applicable  

- 1.1.1.1 **Parameter:** IMSI

| DataType | String |
|----------|--------|
| Min      | 6      |
| Max      | 15     |
| Default  |        |

Maximum 15 decimal digits providing the IMSI.  
- The leading 3 digits provide the **Mobile Country Code (MCC)**.  
- The next 2 or 3 digits (depending on *MNC length*) provide the **Mobile Network Code (MNC)**.  
- The remaining digits provide the **Mobile Subscriber Identification Number (MSIN)**.  

In **MTS_MODE**, collective configuration is required by using the `SETUEGROUPCONTEXT` command and defining a rule for generating a unique IMSI per UE. Refer to the MTS user guide for details.  

*Note:* These two parameters are shown in an optional block but are in fact mandatory.  



- 1.1.1.2 **Parameter:** MNC length

| DataType | Int |
|----------|-----|
| Min      | 2   |
| Max      | 3   |
| Default  |     |

Provides the length of the MNC in digits.  


- 1.1.1.2 **Parameter:** Access control class (Optional)

| DataType | Bool           |
|----------|----------------|
| Min      | Not Applicable |
| Max      | Not Applicable |
| Default  | Not Applicable |

Configures the USIM.  

**SCOPE:**  
Activation time is controlled by the Activate command. The command is only valid if the UE is not registered to the network.  
The command can only be issued once after STRT.  

**DESCRIPTION:**  
Configures a real or simulated USIM.  

**SYNTAX:**  
FORW MTE USIMCONFIG  

**TAG:**  
Not Applicable

---------

| DataType | Enum |
|----------|------|
| Min      | 0    |
| Max      | 1    |
| Default  | 0    |

**Parameter:** Last RPLMN use indication  

- `0` = The UE shall attempt registration on the last registered PLMN.  
- `1` = The UE shall attempt registration on the home network.  


- 1.1.1.3 **Parameter:** Number of APN list entries (Optional)  

| DataType | Bool           |
|----------|----------------|
| Min      | Not Applicable |
| Max      | Not Applicable |
| Default  | Not Applicable |

Configures the USIM.  

**SCOPE:**  
Activation time is controlled by the Activate command. The command is only valid if the UE is not registered to the network.  
The command can only be issued once after STRT.  

**DESCRIPTION:**  
Configures a real or simulated USIM.  

**SYNTAX:**  
FORW MTE USIMCONFIG  

**TAG:**  
Not Applicable  

- 1.1.1.4 **Parameter:** Frequency Type (Optional)

| DataType | Bool           |
|----------|----------------|
| Min      | Not Applicable |
| Max      | Not Applicable |
| Default  | Not Applicable |

Configures the USIM.  

**SCOPE:**  
Activation time is controlled by the Activate command. The command is only valid if the UE is not registered to the network.  
The command can only be issued once after STRT.  

**DESCRIPTION:**  
Configures a real or simulated USIM.  

**SYNTAX:**  
FORW MTE USIMCONFIG  

**TAG:**  
Not Applicable  

---------

- **Parameter:** UAC Access Identities Configuration
  
| DataType | Int  |
|----------|------|
| Min      | 0    |
| Max      | 0x01 |
| Default  |      |

- `b1 = 0`: UE is not configured for Multimedia Priority Service in the HPLMN, EHPLMN, or a visited PLMN of the home country.  
- `b1 = 1`: UE is configured for Multimedia Priority Service in the HPLMN, EHPLMN, or a visited PLMN of the home country.  

*Note:* 8-bit value, but only **b1** is supported at present.  


- 1.2 **Parameter:** Number of PLMN lists (Optional)

| DataType | Bool          |
|----------|--------------|
| Min      | 1            |
| Max      | 1            |
| Default  | Not Applicable |

Configures the USIM.  

**SCOPE:**  
Activation time is controlled by the Activate command. The command is only valid if the UE is not registered to the network.  
The command can only be issued once after STRT.  

**DESCRIPTION:**  
Configures a real or simulated USIM.  

**SYNTAX:**  
FORW MTE USIMCONFIG  

**TAG:**  
Not Applicable  

-------

- 1.2.1 **Parameter:** Reset USIM  

| DataType | Enum |
|----------|------|
| Min      | 0    |
| Max      | 1    |
| Default  | 0    |

- `0` = Only update supplied parameters.  
- `1` = Reset all parameters to defaults.  

Only applicable for **Simulated USIM**.  
Defaults are test USIM parameters as defined in **34.108 Section 8**.  

- 1.2.2 **Parameter:** Authentication Key  

| DataType | String[Hex] |
|----------|-------------|
| Min      | 32          |
| Max      | 32          |
| Default  |             |

Only applicable for **Simulated USIM**.  
Overrides secret key `K` for authentication.  

The 128-bit key is specified as a 32 hexadecimal digit number where each hexadecimal digit represents a 4-bit *nibble*.  
Example:  ABCDEF12345678900123456789ABCDEF


Default is test USIM parameters as defined in **34.108 Section 8**.  

- 1.3 **Parameter:** OP (Optional)

| DataType | Bool         |
|----------|--------------|
| Min      | 1            |
| Max      | 1            |
| Default  | Not Applicable |

Configures the USIM.  

**SCOPE:**  
Activation time is controlled by the Activate command. The command is only valid if the UE is not registered to the network.  
The command can only be issued once after STRT.  

**DESCRIPTION:**  
Configures a real or simulated USIM.  

**SYNTAX:**  
FORW MTE USIMCONFIG  

**TAG:**  
Not Applicable  


- 1.4 **Parameter:** SMC_address (Optional)

| DataType | Bool           |
|----------|----------------|
| Min      | Not Applicable |
| Max      | Not Applicable |
| Default  | Not Applicable |

Configures the USIM.  

**SCOPE:**  
Activation time is controlled by the Activate command. The command is only valid if the UE is not registered to the network.  
The command can only be issued once after STRT.  

**DESCRIPTION:**  
Configures a real or simulated USIM.  

**SYNTAX:**  
FORW MTE USIMCONFIG  

**TAG:**  
Not Applicable  


- 1.5 **Parameter:** Number of Allowed-CSG-Entries (Optional)

| DataType | Bool           |
|----------|----------------|
| Min      | Not Applicable |
| Max      | Not Applicable |
| Default  | Not Applicable |

Configures the USIM.  

**SCOPE:**  
Activation time is controlled by the Activate command. The command is only valid if the UE is not registered to the network.  
The command can only be issued once after STRT.  

**DESCRIPTION:**  
Configures a real or simulated USIM.  

**SYNTAX:**  
FORW MTE USIMCONFIG  

**TAG:**  
Not Applicable  

- 1.6 **Parameter:** Protection Scheme (Optional)

| DataType | Bool          |
|----------|--------------|
| Min      | 1            |
| Max      | 1            |
| Default  | Not Applicable |

Configures the USIM.  

**SCOPE:**  
Activation time is controlled by the Activate command. The command is only valid if the UE is not registered to the network.  
The command can only be issued once after STRT.  

**DESCRIPTION:**  
Configures a real or simulated USIM.  

**SYNTAX:**  
FORW MTE USIMCONFIG  

**TAG:**  
Not Applicable  


# 
























