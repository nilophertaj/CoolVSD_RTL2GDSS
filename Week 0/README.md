# 🛠️ VLSI Tool Installation Verification

This README documents the **installation process for all open-source EDA tools** used in the VLSI System Design (VSD) Program. Screenshots of successful installations can be attached to provide proof of setup.

---

## 📌 Tools Installation Overview

| Tool | Purpose | Status |
|------|---------|--------|
| 🧠 **Yosys** | RTL Synthesis & Logic Optimization | ✅ Installed | 
| 📟 **Iverilog** | Verilog Simulation & Compilation | ✅ Installed | 
| 📊 **GTKWave** | Waveform Viewer & Analysis | ✅ Installed | 
| ⚡ **Ngspice** | Analog & Mixed-Signal Simulation | ✅ Installed | 
| 🎨 **Magic VLSI** | Layout Design & DRC Verification | ✅ Installed | 
| 🐳 **Docker** | Containerization Platform | ✅ Installed | 
| 🌊 **OpenLane** | Complete RTL-to-GDSII Flow | ✅ Installed | 

---

## 🧠 Yosys Installation

```bash
sudo apt-get update
git clone https://github.com/YosysHQ/yosys.git
cd yosys
sudo apt install make       # If make is not installed
sudo apt-get install build-essential clang bison flex \
 libreadline-dev gawk tcl-dev libffi-dev git \
 graphviz xdot pkg-config python3 libboost-system-dev \
 libboost-python-dev libboost-filesystem-dev zlib1g-dev
make config-gcc
make
sudo make install
yosys -V   # Verify installation
```
Snapshot : https://github.com/nilophertaj/VSD_RTL2GDS/blob/main/Snapshots/Yosys.png?raw=true

## 📟 Iverilog Installation
```bash
sudo apt-get update
sudo apt-get install iverilog
iverilog -v   # Verify installation
```
Snapshot : https://github.com/nilophertaj/VSD_RTL2GDS/blob/main/Snapshots/iverilog.png?raw=true

## 📊 GTKWave Installation
```bash
sudo apt-get update
sudo apt install gtkwave
gtkwave --version   # Verify installation
```
Snapshot : https://github.com/nilophertaj/VSD_RTL2GDS/blob/main/Snapshots/GTKWave.png?raw=true

## ⚡ Ngspice Installation
```bash
# Download ngspice tarball from SourceForge
tar -zxvf ngspice-37.tar.gz
cd ngspice-37
mkdir release
cd release
../configure --with-x --with-readline=yes --disable-debug
make
sudo make install
ngspice -v   # Verify installation
```
Snapshot : https://github.com/nilophertaj/VSD_RTL2GDS/blob/main/Snapshots/ngSpice.png?raw=true

## 🎨 Magic VLSI Installation
```bash
sudo apt-get install m4 tcsh csh libx11-dev tcl-dev tk-dev libcairo2-dev mesa-common-dev libglu1-mesa-dev libncurses-dev
git clone https://github.com/RTimothyEdwards/magic
cd magic
./configure
make
sudo make install
magic -d   # Verify installation
```
Snapshot : https://github.com/nilophertaj/VSD_RTL2GDS/blob/main/Snapshots/magic.png?raw=true

## Advanced Installations 
## 🐳 Docker Installation
```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git 
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world
sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot   # After reboot, verify
docker run hello-world
docker --version
```
Snapshot : https://github.com/nilophertaj/VSD_RTL2GDS/blob/main/Snapshots/docker.png?raw=true

## 🌊 OpenLane Installation
```bash
cd $HOME
git clone https://github.com/The-OpenROAD-Project/OpenLane
cd OpenLane
make
make test
python3 --version
python3 -m pip --version
make --version
python3 -m venv -h
```
Snapshot : https://github.com/nilophertaj/VSD_RTL2GDS/blob/main/Snapshots/openLane.png?raw=true

## INSTALLATIONS COMPLETED SUCCESSFULLY ✅ 
