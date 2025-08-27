# Issue Info

1. **Date :** 2025-08-27
2. **Server's model :** Supermicro-HS119-R12H13

- **Issue 1 :** The server is unable to detect or read any HDD/SSD installed inside it and fail to install Ubuntu.
  - **method :** Enter the RAID controller and configure the drives as [JBOD]
 
- **Issue 2 :** The BMC is experiencing severe latency
  - **method :** Unplug the server’s power cable, discharge it, and then restart.

## Method of Issue 1 ( Steps )

**Method :** Enter the RAID controller and configure the drives as [JBOD]
**Reason :** This machine has an additional hardware RAID card installed. The front hard drives are all managed by this RAID card, so it is normal that they are not visible under SATA in the BIOS. Regardless of whether you intend to use RAID, you must enter the RAID card and configure the drives as [RAID] or [JBOD] before they will appear in the installation screen. The same process applies when adding hard drives in the future.

### 1. Run Setup
Restart the server, and at the screen shown below, press DEL to enter the **Setup** menu.
<img width="817" height="533" alt="image" src="https://github.com/user-attachments/assets/7c99a40e-7a56-470d-9561-e4a2abf41ced" />

### 2. Enter "Advanced" > " Broadcom SAS 9360 Configuration Utility - 07.31.02.00 "
<img width="860" height="689" alt="1" src="https://github.com/user-attachments/assets/4c6d1237-eb94-4bf6-b6e6-9a4ecaad419d" />

### 3. To set `Driver = 1` , `JBODs = 1`  ， Enter" Main menu "  
<img width="839" height="667" alt="2" src="https://github.com/user-attachments/assets/00fe6447-2ce9-4a7b-97d6-7c0747387979" />

### 4. Enter " Drive Management "
<img width="1006" height="750" alt="image" src="https://github.com/user-attachments/assets/c83887db-0ddb-4540-9721-36e2b11d3894" />


### 5. Here are all the hard drives you have installed
<img width="896" height="717" alt="4" src="https://github.com/user-attachments/assets/15d68d42-3311-42b1-965b-af2533d4f27d" />

### 6. "Operation" > "Make JBOD" 
<img width="864" height="704" alt="5" src="https://github.com/user-attachments/assets/daa55e1d-8489-4435-b55c-3ca4e4e01535" />

### 7. Go
<img width="857" height="655" alt="6" src="https://github.com/user-attachments/assets/731c40cc-7d36-439f-8d58-4774700f2309" />

### 8. Success to set `Driver = 1` , `JBODs = 1`
<img width="867" height="678" alt="7" src="https://github.com/user-attachments/assets/43e7901b-ddff-4b09-a47f-0f55a5f0a417" />
