## OAI installation
**ref** \
https://chatgpt.com/share/6852793b-5a48-8000-907f-2ad799131ab0 \
\
**Preparatory work** 
```c
sudo apt update && sudo apt upgrade -y
sudo apt install git cmake build-essential pkg-config -y
```
**Build workspace**  `openairinterface`
```c
mkdir -p ~/openairinterface
cd ~/openairinterface
```
**Installing the 5G Core Network (OAI 5GC)** 

* Download the OAI 5G Core source code
```
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-fed.git
cd oai-cn5g-fed
git checkout develop

```
* Set up the Docker environment
```
./scripts/syncComponents.sh  # 下載各核心模組
docker-compose -f docker compose/docker compose-basic-vpp.yaml up --build  # 開始建構並啟動 OAI 所有 container
```

>[!Caution]
>`Command 'docker-compose' not found, but can be installed with: ... ` \
>![image](https://github.com/user-attachments/assets/5d68b84e-6451-4979-ab60-191f001d3e95)

**solution :** \
follow the advice blew the warning `sudo snap install docker`

Again, Enter the command \
`docker compose -f docker-compose/docker-compose-basic-vpp.yaml up --build  # 開始建構並啟動 OAI 所有 container`

>[!Caution]
>表示執行的指令語法是正確的，但系統找不到指定的 docker-compose-basic-vpp.yaml 檔案
```
codebind@codebind:~/openairinterface/oai-cn5g-fed$ docker compose -f docker-compose/docker-compose-basic-vpp.yaml up --build
open /home/codebind/openairinterface/oai-cn5g-fed/docker-compose/docker-compose-basic-vpp.yaml: no such file or directory
```
**solution :** \
執行以下指令 `ls -l docker-compose` 列出 docker-compose 資料夾的內容 

![image](https://github.com/user-attachments/assets/125b98fb-a9db-4c1d-804f-34147f469207)
證實 docker-compose-basic-vpp.yaml 確實不存在 ，隨機選取資料夾中的 docker-compose-basic-vpp-pcf-steering.yaml 啟動，確認core是否可以成功運行 \
`docker compose -f docker-compose/docker-compose-basic-vpp-pcf-steering.yaml up --build`

![image](https://github.com/user-attachments/assets/00311281-57e7-4556-8749-651afca808d0)
>[!WARNING] 
>docker 會「前景模式」運行所有 container，並把 container 的 log 寫到你的終端機上（如上圖），導致**無法在同一個終端機再輸入其他指令**。

按 Ctrl + C 停止 (以下為成功停止 docker compose 的畫面)
![image](https://github.com/user-attachments/assets/f566575e-bcdb-48c6-aed7-3d692921525f)

**solution :** \
改用以下指令可以在背景模式下運行容器，僅顯示運行狀態 \
`docker compose -f docker-compose/docker-compose-basic-vpp-pcf-steering.yaml up -d`
![image](https://github.com/user-attachments/assets/59e60734-8867-42fd-9c32-2a282089bfc6)

enter `docker ps`check the containers status (the result is blew)
![image](https://github.com/user-attachments/assets/7b53a69a-5905-4e35-96c0-81dfa201378c)

目前的 OAI 5G Core 所有 container 都已經：

*  啟動成功 
*  健康檢查狀態為 (healthy) 
*  開啟了關鍵 Port，包括： 

AMF: SCTP 38412

SMF: GTPU 8805

UPF: GTPU 2152

其他服務皆啟用 8080 / 9090（管理與 API）\
這代表現在的 5G 核心網路平台已經完全準備好，接下來你可以模擬「基地台（gNB）」與「用戶裝置（UE）」的附著與上網行為。

## 啟動 gNB + UE 模擬器（gnbsim）
輸入  `ls -l docker-compose` 可以見到以下幾種gNB模擬器yaml檔

-rw-rw-r-- 1 codebind codebind  1388 Jun 20 09:44 docker-compose-gnbsim-ebpf.yaml  \
-rw-rw-r-- 1 codebind codebind  2686 Jun 20 09:44 docker-compose-gnbsim-vpp-additional.yaml    \
-rw-rw-r-- 1 codebind codebind  1409 Jun 20 09:44 docker-compose-gnbsim-vpp.yaml    \
-rw-rw-r-- 1 codebind codebind  6185 Jun 20 09:44 docker-compose-gnbsim.yaml  \

這裡選最基礎的 docker-compose-gnbsim.yaml 開始：
```
docker compose -f docker-compose/docker-compose-gnbsim.yaml up -d
```
>[!Caution]
>docker-compose 檔案中指定的 gnbsim 映像名稱為 gnbsim，但電腦端或 Docker Hub 找不到這個映像
![image](https://github.com/user-attachments/assets/0a1cbca7-e1b7-4ff2-8b96-85d074120e98)

**solution :** \
ref : https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-fed/-/blob/master/docs/DEPLOY_SA5G_MINI_WITH_GNBSIM.md?utm_source=chatgpt.com#6-getting-a-gnbsim-docker-image 

follow steps blew : 
* 拉取 image：
```
docker pull rohankharade/gnbsim
```
* 建立 tag 以符合 docker-compose 使用的名稱：
```
docker image tag rohankharade/gnbsim:latest gnbsim:latest
```
* 執行模擬器：
```
docker compose -f docker-compose/docker-compose-gnbsim.yaml up -d
```
這樣 docker-compose 裡寫的 image: gnbsim 就會成功對應到本機的 gnbsim:latest  (下圖為成功啟動gNB畫面)。
![image](https://github.com/user-attachments/assets/04f2380f-8f2e-4286-82c7-b3b64dea5bf6)
成功啟動 gnbsim 模擬器，畫面顯示：

*  6 個 gnbsim 容器都已啟動（包括 gnbsim、gnbsim2～5、gnbsim-fqdn）
* 沒有錯誤
* 所有容器都顯示 Started，這代表 gnbsim 映像設定正確，Docker Compose 執行成功！

**查看 UE 是否成功與 AMF 建立連線（UE attach）**
執行以下兩條指令觀察 log：

1. 查看 gnbsim 的 log
```
docker logs gnbsim
```
result : [ gnbsim log .txt ]https://github.com/shuchu11/MS-NUK-Leo/blob/77fc5a8b66d33b298418a05ee4a493e50176f7ad/oai_installation/gNB_log.txt

![image](https://github.com/user-attachments/assets/4dbb0bfe-4a2e-48dc-b9a4-db027856412f)

這段 gnbsim log 顯示，成功完成了一次完整的 5G UE 附著與 PDU Session 建立流程！

UE 成功註冊（Registration Complete），並完成以下訊息交換：
```
* Message Type: Registration Accept (0x42)
5G-GUTI: ...
```
1. Initial UE Message
2. Authentication Request → Response
3. Security Mode Command → Complete
4. Registration Accept
5. InitialContextSetup (AMF to gNB)

* PDU Session 建立成功，獲得 IP 位址
```
PDU session type: IPv4(1)
PDU address information: 12.1.1.4
```
1. PDU Session Establishment Accept
2. UE 被指派 IP：12.1.1.4（IPv4）


GTP-U 通道建立成功
```
GTP-U local addr: 192.168.70.3
GTP-U peer addr : 192.168.72.201
GTP-U Peer TEID: 10876129
UE address: 12.1.1.4
```
1. GTP-U peer address（UPF）：192.168.72.201
2. GTP-U TEID（上下行）：已成功交換

**總結：成功完成了一次完整的模擬 UE 附著、建立 PDU Session 並獲得 IP，核心網路與 gnbsim 功能都正常！**


2. 查看 oai-amf 的 log（這邊會顯示 UE 註冊事件）
```
docker logs oai-amf
```
https://github.com/shuchu11/MS-NUK-Leo/blob/2ce4c607800a411633a9814f2df17188eb3866bb/oai_installation/oai_amf_log.txt
![image](https://github.com/user-attachments/assets/a7137769-f356-49da-b30a-404f9fa50320)
>[!Caution]
>1. AMF 收到 SCTP 關閉事件（SCTP Shutdown）數次。
>2. AMF 回報 No NAS context、No existing nas_context with amf_ue_ngap_id。
>3. 多個 UE 顯示為 5GMM-DEREGISTERED 狀態。
>4. 多個 gNB 顯示為 Disconnected。

可能是註冊失敗， 查詢 log 是否送出 Registration Complete
```
docker logs gnbsim | grep -i "Registration Complete"
```
result : 指令 `docker logs gnbsim | grep -i "Registration Complete"` 沒有任何符合的輸出，代表 UE 根本沒有送出 Registration Complete (0x43) 這個訊息。
**嘗試以下解法**
1. 開啟 gnbsim 的 YAML 設定檔，並確保在每一個 UE 區段中都啟用了 `SendRegComplete: true`

用 `ls -l gnbsim.yaml`查看是否有 nbsim.yml 類似名稱的設定檔  --> 無 \
用 `ls -l docker-compose`查看docker compose中是否有 nbsim.yml 類似名稱的設定檔 
--> 顯示 `-rw-rw-r-- 1 codebind codebind  6185 Jun 20 09:44 docker-compose-gnbsim.yaml` \
它是 Docker Compose 配置檔（.yaml 結尾），不是 gnbsim 的 UE 設定檔 gnbsim.yaml ，仍需要建立 UE 設定檔 gnbsim.yaml，放在正確位置，然後將它掛進 docker-compose-gnbsim.yaml 中。

a. 在當前目錄建立 gnbsim.yaml 檔案 (目前位置 : `~/openairinterface/oai-cn5g-fed`)
```
nano gnbsim.yaml
```
貼上以下內容
```
gnbsim:
  UEs:
    - SUPI: "001010000000001"
      DNN: "internet"
      KEY: "8B9D8D5C9A8F6B49D73ACD2E293A6CBE"
      OP_TYPE: "OPC"
      OPC: "E8ED289DEBA952E4283B54E88E6183CA"
      AMF: "8000"
      HPLMN:
        MCC: "208"
        MNC: "95"
      Slice:
        SST: 1
        SD: "000001"
      Session:
        SendRegComplete: true
        DEREG_AFTER: 3600

```
儲存並離開（ nano 中按 `Ctrl + O` 存檔，`Enter`，`Ctrl+X` 離開）

b.  編輯 docker-compose-gnbsim.yaml
```
ls
cd docker-compose
nano docker-compose-gnbsim.yaml
docker logs oai-amf | grep -i "Initial Registration"

```
發現總共有 gnbsim1 ~ gnbsim5 的多個模擬 UE 實例
![螢幕擷取畫面 2025-06-21 104946](https://github.com/user-attachments/assets/9266078c-9d30-423f-9e52-4d6e76698243)

為每個 gnbsim 的 enviroment 內加上`- SendRegComplete=true `  \
Example : 
```
        environment:
            - MCC=208
            - MNC=95
            - GNBID=1
            - TAC=0x00a000
            - SST=222
            - SD=00007b
            - PagingDRX=v32
            - RANUENGAPID=0
            - SendRegComplete=true  # 👈 add this line !!!!
            - IMEISV=35609204079514

```
需要重新啟動 gnbsim 容器並確保變數生效 (目前位置 : `~/openairinterface/oai-cn5g-fed/docker-compose`)
```
docker-compose -f docker-compose-gnbsim.yaml down
docker-compose -f docker-compose-gnbsim.yaml up -d
```
![image](https://github.com/user-attachments/assets/de5b42bf-3241-453d-93d7-c020cf62bb3f)

檢查以下 : 
* 查看是否有產生 `Registration Complete`
```
docker logs oai-amf | grep -i "Registration Complete"
```
![image](https://github.com/user-attachments/assets/de6695c1-3930-472c-a970-1d80aa3b616c)
代表所有 5 個 gnbsim UE 都已成功送出並由 AMF 接收到 Registration Complete 訊息。這表示gnbsim 配置中已正確加入 SendRegComplete=true 環境變數，且流程無誤。
* 查看 AMF 是否顯示 UE 狀態為 `5GMM-REGISTERED`（表示註冊成功）
```
docker logs oai-amf | grep -i "5GMM"
```
![image](https://github.com/user-attachments/assets/d97579a0-4260-4e18-ac4d-dfa1148b9f72)
出現大量 `5GMM-DEREGISTERED`（表示註冊失敗）
**代表在docker-compose 的 enviroment 中加入`- SendRegComplete=true`沒有用**
* 檢查是否有錯誤或 AMF 拒絕註冊
```
docker logs oai-amf | grep -i "deregister\|context\|registration"
```
顯示 (以下為部分訊息)
```

[2025-06-20 11:26:12.731] [amf_n1] [debug] Set 5GMM state to 5GMM-DEREGISTERED
   |  Index |     5GMM State     |        IMSI        |        GUTI        |   RAN UE NGAP ID   |   AMF UE NGAP ID   |        PLMN        |       Cell Id      |
   |    1   |   5GMM-REGISTERED  |   208950000000031  |20895010041372800554|        0x00        |        0x03        |       208,95       |      000004001     |
   |    2   |   5GMM-REGISTERED  |   208950000000032  |20895010041977047435|        0x00        |        0x04        |       208,95       |      000008001     |
   |    3   |  5GMM-DEREGISTERED |   208950000000033  |20895010041290854175|        0x00        |        0x01        |       208,95       |      00000c001     |
   |    4   |   5GMM-REGISTERED  |   208950000000034  |20895010041219315990|        0x00        |        0x02        |       208,95       |      000010001     |
   |    5   |   5GMM-REGISTERED  |   208950000000035  |20895010041159606502|        0x00        |        0x05        |       208,95       |      000014001     |
[2025-06-20 11:26:12.731] [amf_app] [debug] The UE's state (IMSI 208950000000033, State 5GMM-DEREGISTERED) has been successfully updated!
[2025-06-20 11:26:12.937] [amf_app] [debug] The UE's state (IMSI 208950000000034, State 5GMM-DEREGISTERED) has been successfully updated!
[2025-06-20 11:26:12.937] [amf_n1] [debug] Set 5GMM state to 5GMM-DEREGISTERED
   |  Index |     5GMM State     |        IMSI        |        GUTI        |   RAN UE NGAP ID   |   AMF UE NGAP ID   |        PLMN        |       Cell Id      |
   |    1   |   5GMM-REGISTERED  |   208950000000031  |20895010041372800554|        0x00        |        0x03        |       208,95       |      000004001     |
   |    2   |   5GMM-REGISTERED  |   208950000000032  |20895010041977047435|        0x00        |        0x04        |       208,95       |      000008001     |
   |    3   |  5GMM-DEREGISTERED |   208950000000033  |20895010041290854175|        0x00        |        0x01        |       208,95       |      00000c001     |
   |    4   |  5GMM-DEREGISTERED |   208950000000034  |20895010041219315990|        0x00        |        0x02        |       208,95       |      000010001     |
   |    5   |   5GMM-REGISTERED  |   208950000000035  |20895010041159606502|        0x00        |        0x05        |       208,95       |      000014001     |
[2025-06-20 11:26:12.937] [amf_app] [debug] The UE's state (IMSI 208950000000034, State 5GMM-DEREGISTERED) has been successfully updated!
[2025-06-20 11:26:13.145] [amf_app] [debug] The UE's state (IMSI 208950000000031, State 5GMM-DEREGISTERED) has been successfully updated!
[2025-06-20 11:26:13.145] [amf_n1] [debug] Set 5GMM state to 5GMM-DEREGISTERED
   |  Index |     5GMM State     |        IMSI        |        GUTI        |   RAN UE NGAP ID   |   AMF UE NGAP ID   |        PLMN        |       Cell Id      |
   |    1   |  5GMM-DEREGISTERED |   208950000000031  |20895010041372800554|        0x00        |        0x03        |       208,95       |      000004001     |
   |    2   |   5GMM-REGISTERED  |   208950000000032  |20895010041977047435|        0x00        |        0x04        |       208,95       |      000008001     |
   |    3   |  5GMM-DEREGISTERED |   208950000000033  |20895010041290854175|        0x00        |        0x01        |       208,95       |      00000c001     |
   |    4   |  5GMM-DEREGISTERED |   208950000000034  |20895010041219315990|        0x00        |        0x02        |       208,95       |      000010001     |
   |    5   |   5GMM-REGISTERED  |   208950000000035  |20895010041159606502|        0x00        |        0x05        |       208,95       |      000014001     |
[2025-06-20 11:26:13.145] [amf_app] [debug] The UE's state (IMSI 208950000000031, State 5GMM-DEREGISTERED) has been successfully updated!
[2025-06-20 11:26:13.349] [amf_app] [debug] The UE's state (IMSI 208950000000032, State 5GMM-DEREGISTERED) has been successfully updated!
[2025-06-20 11:26:13.349] [amf_n1] [debug] Set 5GMM state to 5GMM-DEREGISTERED
   |  Index |     5GMM State     |        IMSI        |        GUTI        |   RAN UE NGAP ID   |   AMF UE NGAP ID   |        PLMN        |       Cell Id      |
   |    1   |  5GMM-DEREGISTERED |   208950000000031  |20895010041372800554|        0x00        |        0x03        |       208,95       |      000004001     |
   |    2   |  5GMM-DEREGISTERED |   208950000000032  |20895010041977047435|        0x00        |        0x04        |       208,95       |      000008001     |
   |    3   |  5GMM-DEREGISTERED |   208950000000033  |20895010041290854175|        0x00        |        0x01        |       208,95       |      00000c001     |
   |    4   |  5GMM-DEREGISTERED |   208950000000034  |20895010041219315990|        0x00        |        0x02        |       208,95       |      000010001     |
   |    5   |   5GMM-REGISTERED  |   208950000000035  |20895010041159606502|        0x00        |        0x05        |       208,95       |      000014001     |
```
 表示 gnbsim 容器有成功傳送 Registration Complete，但 AMF 仍然將 UE 狀態顯示為 5GMM-DEREGISTERED。這表示「表面上完成註冊流程，但 AMF 沒有認為該 UE 真正完成註冊」。

 需要確認 `Security Mode Complete` 是否有成功，\
 目前只 ` grep 了 Registration Complete`，但 AMF 還需要：\
`Authentication Request` \
`Authentication Response`\
`Security Mode Command`\
`Security Mode Complete`\

使用以下指令確認是否有 `Security Mode Complete`
```
docker logs oai-amf | grep -i "Security Mode\|Authentication\|Initial Context\|Registration"
```
## problem 
顯示 : 目前看到所有 UE 都停在 `5GMM-DEREGISTERED`，而且 AMF log 也顯示收到了 `Registration Complete`，表示 AMF 收到 UE 註冊完成的訊息，但狀態沒有更新成 `5GMM-REGISTERED`。

**嘗試以下方法** 
1. 將`DERED_AFTER=3600`調大以維持註冊狀態。\
   因為gnbsim.yaml (`~/openairinterface/oai-cn5g-fed`)中，`DERED_AFTER=3600` 表示模擬的 UE 在註冊後 3600 秒會自動發送 `Deregistration Request`
   
   ```
   DERED_AFTER=86400  # 一整天
   ```

重啟 gnbsim 容器群組   
```
docker-compose -f docker-compose/docker-compose-gnbsim.yaml down
docker-compose -f docker-compose/docker-compose-gnbsim.yaml up -d
```
再次查看 AMF 是否顯示 UE 狀態為 5GMM-REGISTERED（表示註冊成功）
```
docker logs oai-amf | grep -i "5GMM"
```
以下為部分截圖(從某一時間開始 `REGISTERED`狀態 變為 `DEREGISTERED`狀態 )
![image](https://github.com/user-attachments/assets/af42a901-56b2-431e-9910-caef6aa9eac2)
## *
2. 使用 **Wireshark** 驗證是否是 AMF 或其他 CN 元件主動發送 `Deregistration`
install wireshark
```
sudo apt update
sudo apt install wireshark -y
```
讓目前使用者可以抓取封包
```
sudo usermod -aG wireshark $USER
newgrp wireshark
```
啟動 Wireshark 圖形介面
```
wireshark &
```
![image](https://github.com/user-attachments/assets/1eb924af-8283-440d-9681-045e44e4e037)

雙擊 `any` ，開始從所有介面擷取封包(以下為擷取結果)
![image](https://github.com/user-attachments/assets/f07a3161-074d-489c-b2d3-17bd65ee50c8)

目前擷取到的封包主要是：

1. DNS 封包（向 connectivity-check.ubuntu.com 查詢）
2. TCP 封包與 HTTP 回應（ubuntu 預設連線檢查）
3. ICMPv6 封包與 ARP 封包
4. 沒有任何 SCTP、NGAP、NAS_5GS、PFCP、GTP-U 相關封包（也就是與 5G Core 相關的訊息）

啟動gnbsim ( wireshark保持開啟狀態 )
```
docker-compose -f docker-compose-gnbsim.yaml up -d
```
![image](https://github.com/user-attachments/assets/a1ef413d-cf95-4623-897f-846283d69612)
>[!Caution]
>* 所有封包都是從 127.0.0.1 → 127.0.0.1（loopback）
>* * 大量的 TCP [SYN] 嘗試連線，但立即收到 [RST, ACK]（連線重置）
這代表：\
Docker container 中的某些服務（如 AMF、NRF、UDM 等）在 gnbsim 嘗試連接時並未正確啟動或在 localhost 上沒有對應 port 在 listen，導致 gnbsim 建立連線失敗。

**solution :**\
打開` /docker-compose/omec-gnbsim-config.yaml `(https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-fed/-/blob/master/docker-compose/omec-gnbsim-config.yaml?ref_type=heads)\
把 AMF 的 IP 改成與 gnbsim 不同( 填 `127.0.0.1` 會變成 gnbsim 嘗試連接「自己」的 localhost ) \
以下為`omec-gnbsim-config.yaml`目前內容
```
defaultAmf:
  hostName: oai-amf # Host name of AMF
  ipAddr: # AMF IP address
  port: 38412 # AMF port
```
 `AMF IP address` 是空的，請改成以下
```
defaultAmf:
  hostName: oai-amf
  ipAddr: oai-amf   # 或用實際 AMF container IP
  port: 38412   # AMF port
```
開啟 customProfile 中的測試用戶
```
customProfiles:
  customProfiles1:
    enable: true
```
gNB 和 UE Profile 的 PLMN 不一致，統一修改成以下
```
mcc: 208  
mnc: 95
```

sequenceNumber 長度改為 8 bytes
```
sequenceNumber: "16f3b3f70fc2abcd"
```
有時 OAI 期望為 8 bytes（64bit），如果長度不符合可能導致 NAS 計數同步有問題。

開啟 `httpServer` 觀察 gnbsim 內部狀態( 可進一步在外部呼叫 HTTP API，例如查詢註冊狀態 )
```
httpServer:
  enable: true    # <--- 此為修改處
  ipAddr: "<gnbsim 容器 IP>"
  port: 8080
```

Docker-compose 網路要一致\
確認 docker-compose-gnbsim.yaml 中
```
networks:
  public_net:   # 必須與 oai-amf 所屬的 network 相同
```
oai_amf (  [docker-compose-mini-nonrf.yaml](https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-fed/-/blob/master/docker-compose/docker-compose-mini-nonrf.yaml?ref_type=heads) )
```
 oai-amf:
        container_name: "oai-amf"
        image: oaisoftwarealliance/oai-amf:v2.1.0
        volumes:
            - ./conf/mini_nonrf_config.yaml:/openair-amf/etc/config.yaml
        environment:
            - TZ=Europe/Paris
        depends_on:
            - mysql
        networks:
            public_net:
                ipv4_address: 192.168.70.132
```

**確認 AMF 與 gnbsim 確實一致**

將[omec-gnbsim-config.yaml](https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-fed/-/blob/master/docker-compose/omec-gnbsim-config.yaml?ref_type=heads)
中 customProfiles1 的 iteration3 註解掉 ( `#` 表註解 )
```
iterations:
          ...
         # "next":  "iteration3"
         #- "name": "iteration3"
         # "1": "UE-INITIATED-DEREGISTRATION-PROCEDURE 10"

```

開始重新測試

檢查 Core Network container 是否正常啟動，執行以下指令，確保以下 container 都在「 up 」狀態
```
cd ~/openairinterface/oai-cn5g-fed/docker-compose

# 停止舊容器
docker-compose -f docker-compose-gnbsim.yaml down

# 重新啟動
docker-compose -f docker-compose-gnbsim.yaml up -d

# 等待10秒後查看 log
docker ps                             # 查看容器名稱
docker logs gnbsim                    
```
| 用途	|過濾器 |
|觀察 SCTP / NGAP	|sctp|
|觀察 UE 註冊流程	| ngap|
|觀察與 gnbsim IP 互動 | ip.addr == 192.168.70.3|
|觀察 SCTP 38412 port| tcp.port == 38412|

## 做不出來，換下一個方法

ref : https://gitlab.eurecom.fr/oai/openairinterface5g/-/blob/develop/doc/NR_SA_Tutorial_OAI_CN5G.md?ref_type=heads \

OAI CN5G pre-requisites
```
sudo apt install -y git net-tools putty

# https://docs.docker.com/engine/install/ubuntu/
sudo apt update
sudo apt install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Add your username to the docker group, otherwise you will have to run in sudo mode.
sudo usermod -a -G docker $(whoami)
reboot
```

OAI CN5G configuration files
```
wget -O ~/oai-cn5g.zip https://gitlab.eurecom.fr/oai/openairinterface5g/-/archive/develop/openairinterface5g-develop.zip?path=doc/tutorial_resources/oai-cn5g
unzip ~/oai-cn5g.zip
mv ~/openairinterface5g-develop-doc-tutorial_resources-oai-cn5g/doc/tutorial_resources/oai-cn5g ~/oai-cn5g
rm -r ~/openairinterface5g-develop-doc-tutorial_resources-oai-cn5g ~/oai-cn5g.zip
```
Pull OAI CN5G docker images
```
cd ~/oai-cn5g
docker compose pull
```
![image](https://github.com/user-attachments/assets/4b11bb19-071b-4009-a5f3-19f79b6d9e6b)
Start OAI CN5G
```
cd ~/oai-cn5g
docker compose up -d
```
![image](https://github.com/user-attachments/assets/85891516-4fdd-4946-a7ee-f9deae8d1a42)

Stop OAI CN5G
```
cd ~/oai-cn5g
docker compose down
```
**Run 5G NR SA end-to-end setup with OAI gNB**
## 1.Testing with COTS UE ?????????????????????/

## 2. Testing with OAI nrUE
OAI gNB and OAI nrUE pre-requisites
```
# https://files.ettus.com/manual/page_build_guide.html
sudo apt install -y autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool g++ git inetutils-tools libboost-all-dev libncurses-dev libusb-1.0-0 libusb-1.0-0-dev libusb-dev python3-dev python3-mako python3-numpy python3-requests python3-scipy python3-setuptools python3-ruamel.yaml

git clone https://github.com/EttusResearch/uhd.git ~/uhd
cd ~/uhd
git checkout v4.8.0.0
cd host
mkdir build
cd build
cmake ../
make -j $(nproc)
make test # This step is optional
sudo make install
sudo ldconfig
sudo uhd_images_downloader
```
![image](https://github.com/user-attachments/assets/aa7483e5-ccce-474b-bd91-54e7fd2b64b2)


Build OAI gNB and OAI nrUE
```
# Get openairinterface5g source code
git clone https://gitlab.eurecom.fr/oai/openairinterface5g.git ~/openairinterface5g
cd ~/openairinterface5g
git checkout develop

# Install OAI dependencies
cd ~/openairinterface5g/cmake_targets
./build_oai -I

# nrscope dependencies
sudo apt install -y libforms-dev libforms-bin
```
```
# Build OAI gNB
cd ~/openairinterface5g/cmake_targets
./build_oai -w USRP --ninja --nrUE --gNB --build-lib "nrscope" -C

```

AVX 無法在 VirtualBox 中啟用
ref : https://forums.virtualbox.org/viewtopic.php?t=109776

在主機(非虛擬機)找到 Windows PowerShell ，按右鍵選擇以系統管理員身分執行，輸入以下
```
bcdedit /set hypervisorlaunchtype off
bcdedit /set xsavedisable 1
```
設定 VirtualBox 虛擬機參數（使用 VBoxManage） ，在主機(非虛擬機)輸入
```
VBoxManage.exe modifyvm "Ubuntu22.04" --paravirtprovider none --hwvirtex off --nested-hw-virt off
VBoxManage.exe setextradata "Ubuntu22.04" VBoxInternal/CPUM/IsaExts/AVX 1
VBoxManage.exe setextradata "Ubuntu22.04" VBoxInternal/CPUM/IsaExts/AVX2 1
```
>[!Caution]
>失敗
從以下步驟開始試錯(目前使用 : Ubuntu24 , )，
1. ****
