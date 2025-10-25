# Placement and Floorplanning using OpenROAD

## Overview
OpenROAD is an open-source EDA tool that enables automated digital design implementation, including placement, floorplanning, and routing. It is widely used for VLSI design flow automation with minimal manual intervention.

## Floorplanning
Floorplanning is the process of defining the chip area and arranging the major blocks/modules within the chip. It determines the chip's **die size, aspect ratio, core area, and placement regions** for standard cells.  
Key steps in floorplanning using OpenROAD:
1. **Define die area**: Set the chip boundaries using the `set_die_area` command.
2. **Define core area**: Specify the area where standard cells will be placed.
3. **Macro placement**: Place large macros and I/O pads according to design constraints.
4. **Aspect ratio and utilization**: Set utilization target and optimize the core area for better performance and routability.

## Placement
Placement assigns the exact locations of standard cells inside the core area. Good placement improves **timing, congestion, and power**.  
In OpenROAD:
- **Global placement**: Initially places cells to optimize wirelength and reduce congestion.
- **Legalization**: Adjusts cells to legal locations without overlaps.
- **Detailed placement**: Final fine-tuning to improve timing and routability.

## Steps to Install OpenROAD and Run GUI
### 1. Clone the OpenROAD Repository
```tcl
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
```
### 2. Run the Setup Script
```tcl
sudo ./setup.sh
```
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/ba28882d14727497b08197a4bfbf850844d0e1ee/Week%205/assets/5installs.png)

### 3. Build OpenROAD
While building this openroad, I have got so many errors so I'll share some of my works to overcome the errors.
```bash
1️⃣ Go to the OpenROAD source directory

cd ~/OpenROAD

2️⃣ (Optional) Remove previous build directory and create a new one
rm -rf build
mkdir build
cd build

3️⃣ Configure with CMake
cmake -DCMAKE_INSTALL_PREFIX=$HOME/OpenROAD-install ..

- .. refers to the root OpenROAD source directory.
- -DCMAKE_INSTALL_PREFIX sets the installation directory.

4️⃣ Build using Make
make -j$(nproc)

- -j$(nproc) uses all CPU cores for faster compilation.

5️⃣ Install OpenROAD
make install

-This installs binaries to $HOME/OpenROAD-install/bin.

6️⃣ Add OpenROAD binaries to PATH
export PATH=$HOME/OpenROAD-install/bin:$PATH

-Now you can run openroad from any terminal without errors.
```
### To make it globally accessible:
Once you know the binary path,add it to your 'PATH' so you can run 'OpenROAD' from anywhere.
```bash
echo 'export PATH=$PATH:/home/ingenious_engineer/OpenROAD/build/bin' >>/.bashrc
source ~/.bashrc
```
then you can just type:' openroad ',
it'll be automatically opens the openroad shell like this : openroad>

### 4. Verify Installation
```tcl
source ./env.sh
yosys -help  
openroad -help
```
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/ba28882d14727497b08197a4bfbf850844d0e1ee/Week%205/assets/5yohelp.png)
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/ba28882d14727497b08197a4bfbf850844d0e1ee/Week%205/assets/5open-help.png)

5. Run the OpenROAD Flow
```bash
cd flow
make
```
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/ba28882d14727497b08197a4bfbf850844d0e1ee/Week%205/assets/5cmake.png)

![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/ba28882d14727497b08197a4bfbf850844d0e1ee/Week%205/assets/5cmakedone.png)

![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/ba28882d14727497b08197a4bfbf850844d0e1ee/Week%205/assets/5make.png)

6. Launch the graphical user interface (GUI) to visualize the final layout
 ```bash
make gui_final
```
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/ba28882d14727497b08197a4bfbf850844d0e1ee/Week%205/assets/5finalgui.png)

✅ Installation Complete! You can now explore the full RTL-to-GDSII flow using OpenROAD.

## ORFS Directory Structure and File formats
OpenROAD-flow-scripts/

```plaintext
├── OpenROAD-flow-scripts             
│   ├── docker           -> It has Docker based installation, run scripts and all saved here
│   ├── docs             -> Documentation for OpenROAD or its flow scripts.  
│   ├── flow             -> Files related to run RTL to GDS flow  
|   ├── jenkins          -> It contains the regression test designed for each build update
│   ├── tools            -> It contains all the required tools to run RTL to GDS flow
│   ├── etc              -> Has the dependency installer script and other things
│   ├── setup_env.sh     -> Its the source file to source all our OpenROAD rules to run the RTL to GDS flow
```

Inside the flow/ Directory

```plaintext
├── flow           
│   ├── design           -> It has built-in examples from RTL to GDS flow across different technology nodes
│   ├── makefile         -> The automated flow runs through makefile setup
│   ├── platform         -> It has different technology note libraries, lef files, GDS etc 
|   ├── tutorials        
│   ├── util            
│   ├── scripts                 
```

