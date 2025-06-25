ref : 

1. https://gitlab.eurecom.fr/oai/openairinterface5g/-/blob/develop/doc/NR_SA_Tutorial_OAI_CN5G.md?ref_type=heads 
2. https://gitlab.eurecom.fr/oai/openairinterface5g/-/blob/develop/doc/NR_SA_Tutorial_OAI_nrUE.md 


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

1. Testing with OAI nrUE
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
![螢幕擷取畫面 2025-06-25 100641](https://github.com/user-attachments/assets/ad38ba45-dbd7-41c4-aa98-d1376a03f3b1)


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

>[!Caution]
> `yaml-cpp`未被正確安裝

![螢幕擷取畫面 2025-06-24 175425](https://github.com/user-attachments/assets/266534ca-9182-4f41-b446-9db887736d6f)

清除錯誤的 `yaml-cpp` 安裝
```
sudo rm -rf /usr/local/lib/libyaml-cpp* /usr/local/include/yaml-cpp
```

用 apt 安裝正確版本 `yaml-cpp`
```
sudo apt update
sudo apt install libyaml-cpp-dev
```
查看是否有正確的 CMake config 檔案
```
ls /usr/lib/x86_64-linux-gnu/cmake/yaml-cpp/
```
確認出現 `yaml-cpp-config.cmake  yaml-cpp-config-version.cmake `

>[!Caution]
> 仍無法執行。 CMake 找到 yaml-cpp-config.cmake，它產生的是一個 IMPORTED 而非 GLOBAL target，所以仍無法建立 ALIAS。
>修改 OAI 原始碼
```
nano ~/openairinterface5g/common/config/yaml/CMakeLists.txt
```
修改為以下
```
find_package(yaml-cpp)

if (NOT yaml-cpp_FOUND)
  CPMAddPackage("gh:jbeder/yaml-cpp#yaml-cpp-0.7.0@0.7.0")
  set(YAML_TARGET yaml-cpp::yaml-cpp)
else()
  if (TARGET yaml-cpp::yaml-cpp)
    set(YAML_TARGET yaml-cpp::yaml-cpp)  
  elseif(TARGET yaml-cpp)
    set(YAML_TARGET yaml-cpp)
  else()
    message(FATAL_ERROR "yaml-cpp target not found.")
  endif()
endif()

add_library(params_yaml_static config_yaml.cpp)
target_link_libraries(params_yaml_static PUBLIC UTIL ${YAML_TARGET})

if (ENABLE_TESTS)
  add_subdirectory(tests)
endif()

add_library(params_yaml MODULE config_yaml.cpp)
target_link_libraries(params_yaml PUBLIC UTIL ${YAML_TARGET})
set_target_properties(params_yaml PROPERTIES LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
```
編譯成功
![螢幕擷取畫面 2025-06-24 184319](https://github.com/user-attachments/assets/1e1e5dba-6fd5-4707-be24-1b9703d460ce)
**成功解決** ，下一步 \
Run OAI CN5G and OAI gNB  **( 開啟第二個Terminal分頁，分別執行以下兩步驟 )**

* **CN**
```
# Run OAI CN5G
cd ~/oai-cn5g
docker compose up -d

# RFsimulator
cd ~/openairinterface5g/cmake_targets/ran_build/build
sudo ./nr-softmodem -O ../../../targets/PROJECTS/GENERIC-NR-5GC/CONF/gnb.sa.band78.fr1.106PRB.usrpb210.conf --gNBs.[0].min_rxtxtime 6 --rfsim
```
**下圖中持續輸出slot表示未連上UE (在下一步開啟UE)**
![螢幕擷取畫面 2025-06-24 185833](https://github.com/user-attachments/assets/c0490e4d-5964-4c7a-a892-97f28c1f4821)
**成功連上AMF**
![螢幕擷取畫面 2025-06-24 185924](https://github.com/user-attachments/assets/b7e00b92-480c-4b31-a1c0-853121ac29be)

* **Run OAI nrUE**
```
cd ~/openairinterface5g/cmake_targets/ran_build/build
sudo ./nr-uesoftmodem -r 106 --numerology 1 --band 78 -C 3619200000 --uicc0.imsi 001010000000001 --rfsim
```
**下面兩圖為CN**
![螢幕擷取畫面 2025-06-24 194615](https://github.com/user-attachments/assets/5f1283c8-2a38-4566-81c2-a06f6c961707)
![螢幕擷取畫面 2025-06-24 192654](https://github.com/user-attachments/assets/7cab53e1-d9e7-42eb-a6ff-c62593dc8308)


>[!Warning]
> 若想停止CN和UE( `Ctrl + c`) 卻無法停止，可以透過以下程式碼找到正在進行的ip並強制刪除 (將`{ip 名稱}`換成找到的ip名稱)

```
 ps aux | grep softmodem   # 找 ip
 sudo kill -9 {ip 名稱}
```
**開啟第三個Terminal分頁，執行 End-to-end connectivity test** \
Ping test from the UE host to the CN5G
```
ping 192.168.70.135 -I oaitun_ue1
```
![螢幕擷取畫面 2025-06-24 194035](https://github.com/user-attachments/assets/70368fa0-f16f-42df-a917-406fe524a3a8)
