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
./scripts/syncComponents.sh  # 下載各核心模組
docker-compose -f docker compose/docker compose-basic-vpp.yaml up --build  # 開始建構並啟動 OAI 所有 container
```
>[!Caution]
>`Command 'docker-compose' not found, but can be installed with: ... `
> **solution :** `sudo apt install docker-compose-plugin`

--> Again, Enter the command `docker-compose -f docker compose/docker compose-basic-vpp.yaml up --build  # 開始建構並啟動 OAI 所有 container`
