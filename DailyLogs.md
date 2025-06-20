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

*  啟動成功 \
*  健康檢查狀態為 (healthy) \
*  開啟了關鍵 Port，包括： \

AMF: SCTP 38412

SMF: GTPU 8805

UPF: GTPU 2152

其他服務皆啟用 8080 / 9090（管理與 API）\
這代表現在的 5G 核心網路平台已經完全準備好，接下來你可以模擬「基地台（gNB）」與「用戶裝置（UE）」的附著與上網行為。
