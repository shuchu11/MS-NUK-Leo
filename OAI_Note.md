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
**Installing the 5G Core Network (OAI 5GC)** \

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
以下指令可以在背景模式下運行容器，僅顯示運行狀態 \
`docker compose -f docker-compose/docker-compose-basic-vpp-pcf-steering.yaml up -d     # 開始建構並啟動 OAI 所有 container`
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
