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
