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
## Installing the 5G Core Network (OAI 5GC)

* Download the OAI 5G Core source code
```
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-fed.git
cd oai-cn5g-fed
git checkout develop

```
* Set up the Docker environment
```
sudo snap install docker    # 安裝 docker-compose
./scripts/syncComponents.sh  # 下載各核心模組
```
執行以下指令 `ls -l docker-compose` 列出 docker-compose 資料夾的內容 

![image](https://github.com/user-attachments/assets/125b98fb-a9db-4c1d-804f-34147f469207)

以下指令可以在背景模式下運行容器，僅顯示運行狀態 \
`docker compose -f docker-compose/docker-compose-basic-vpp-pcf-steering.yaml up -d     # 開始建構並啟動 OAI 所有 container`
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





