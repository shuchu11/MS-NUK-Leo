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

**solution :** follow the advice blew the warning `sudo snap install docker`

--> Again, Enter the command \
`docker compose -f docker-compose/docker-compose-basic-vpp.yaml up --build  # 開始建構並啟動 OAI 所有 container`

>[!Caution]
>表示你執行的指令語法是正確的，但系統找不到指定的 docker-compose-basic-vpp.yaml 檔案
```
codebind@codebind:~/openairinterface/oai-cn5g-fed$ docker compose -f docker-compose/docker-compose-basic-vpp.yaml up --build
open /home/codebind/openairinterface/oai-cn5g-fed/docker-compose/docker-compose-basic-vpp.yaml: no such file or directory
```
**solution :** 執行以下指令，列出 docker-compose 資料夾的內容 \
`ls -l docker-compose`

![image](https://github.com/user-attachments/assets/125b98fb-a9db-4c1d-804f-34147f469207)
