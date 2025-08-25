
# PC 

Blew is the screen attach to each server
![114547](https://github.com/user-attachments/assets/16a4a39e-8dc5-4b0b-8d43-b38cd743c7c8)

You can use the buttom here to switch the screen to your server system 
![177724](https://github.com/user-attachments/assets/2b409cd1-48b8-4830-bda3-f710b9004d09)

> [!Caution]
> You need prepare another keyboard for the PC . The keyboard above linking to screen doesn't work. 

# 如何 Re-install os
目前 os : ubuntu v22.04
> [!TIP]
> 無須準備新硬碟，僅需準備 Ubuntu USB 並依照下列步驟 reinstall Ubuntu os

- 將 server 完全關機 (非重新啟動)
- 插上有含SDO的USB後,按下開機鍵後啟動server ，並連續點擊 `DELETE` 進入BIOS > 點擊 `DELETE` 進入Setup

-  --> "Boot" / 確保 ubuntu USB 執行順位在第一(如下圖)

<pr>

<img width="1023" height="816" alt="螢幕擷取畫面 2025-07-30 122810" src="https://github.com/user-attachments/assets/445dadd6-3a78-4412-917e-7d6f2762e8eb" />

<pr>
   
> [!NOTE]
> 若 ubuntu USB 執行順位不在第一 ，請執行以下操作 :
> 利用 上下鍵 + `Enter` 進入第一個選項 > 於深藍色選單中選擇 ubuntu USB > " Yes " > 離開並儲存 `F4`

-  離開並儲存 `F4`
- 系統將自動進入Ubuntu 安裝介面 ，若系統自動跳回BIOS代表從USB請動系統失敗

> [!NOTE]
> 若Ubuntu USB啟動失敗，畫面自動跳回BIOS介面，請嘗試以下指令 --> `F11` Invoke Boot Menu > "USB"




# Change  kernel 

## 1.  open the GRUB menu
```
sudo reboot # Reboot server
```
當看到這個 Supermicro Logo 後，不要按 DEL（那是進 BIOS），而是等待進入 GRUB 階段並按 Shift（BIOS 模式）或 Esc（UEFI 模式）。
<img width="1182" height="828" alt="image" src="https://github.com/user-attachments/assets/83fa17b6-5c40-4d5d-abd2-f07750020148" />


- GRUB menu( 選擇 Advanced options for Ubuntu )
<img width="1591" height="482" alt="image" src="https://github.com/user-attachments/assets/e33413e3-c9bf-435b-b17e-08b59a09773e" />

> [!caution]
> 如果你的系統沒有顯示 GRUB menu ，系統會將你自動導向 UI，這時請你在 Terminal 開啟 GRUB menu 選項，步驟如下:
> ```bash
> # 永久啟用 GRUB 選單
> sudo nano /etc/default/grub
> ```
> 找到：
> ```
> GRUB_TIMEOUT_STYLE=hidden
> GRUB_TIMEOUT=0
> ```
> 改成：
> ```
> GRUB_TIMEOUT_STYLE=menu
> GRUB_TIMEOUT=5   #5 秒可以讓你有時間選 kernel
> ```
> 更新 GRUB
> ```
> sudo update-grub
> ```
> 重新開機
> ```
> sudo reboot
> ```
> 以後在reboot後都會顯示 GRUB menu

## 3.  選 kernel (Ubuntu, with Linux 5.15.0-1032-realtime)
> [!caution]
> **請選擇非Recovery mode** ，因為 Recovery mode 只啟動最小化系統

<img width="1128" height="327" alt="image" src="https://github.com/user-attachments/assets/76243085-be0d-4f0c-9c0d-3768a42675ad" />


## 4. 進入UI
<img width="1742" height="1037" alt="image" src="https://github.com/user-attachments/assets/1b3335b4-6b5a-4aeb-804b-d7521a616a0c" />

在 Terminal 輸入以下確認更新後的 kernel 版本
```
uname -r
```
顯示 `5.15.0-1032-realtime`，更改成功!

<img width="360" height="47" alt="image" src="https://github.com/user-attachments/assets/b9a382a3-0d75-43d7-a385-e89c1c5a4f4a" />


# 製作 Ubuntu 開機隨身碟
https://ithelp.ithome.com.tw/articles/10191497
