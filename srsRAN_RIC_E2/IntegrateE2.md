**ref :** https://docs.srsran.com/projects/project/en/latest/tutorials/source/near-rt-ric/source/index.html

install git & docker
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
ORAN SC RIC deployment is performed as follows ( 需下載`i-release`版本 ) 
> p.s `i-release` 代表最早發行的版本

## Install sr-ric
(目前位置 : `~`)
```
git clone https://github.com/srsran/oran-sc-ric  
cd ./oran-sc-ric
docker compose up
```

```
docker compose ps
```

`????????????????`


在剛開始的`git clone https://github.com/srsran/oran-sc-ric  `
可以用git log 找到想要的 commit / 查看之間版本
```
git tag
```
會看到類似這樣的輸出
```
commit 1234abcd5678ef90...    ← 這是 commit hash（你只需要前7碼）
Author: ...
Date: ...
Message: 修正 E2 接口的錯誤
```
切換（checkout）到該 commit
```
git checkout -b my-feature-branch <commit-hash>
```

<commit-hash> : 772f910

## ZeroMQ
(目前位置 : `~`)
On Ubuntu, ZeroMQ development libraries can be installed with:
```
sudo apt-get install libzmq3-dev
```
Finally, you need to compile srsRAN Project and srsRAN 4G (assuming you have already installed all the required dependencies).

## srsRAN Project
(目前位置 : `~`)
For srsRAN Project, the following commands can be used to download and build from source:
```
git clone https://github.com/srsran/srsRAN_Project.git
cd srsRAN_Project
mkdir build
cd build
cmake ../ -DENABLE_EXPORT=ON -DENABLE_ZEROMQ=ON
```
> [!WARNING]  
>
```
make -j`nproc`

```



