# msvsd8bitcount

# Week-0
# Week-1
# Week-2
# Week-3


## WEEK-0 - Getting the tools

To adequately utilize the open source skywater130 pdk and understand the design flow, we first require to install all the tools, which are
- open_pdk
- magic
- ngspice
- xschem
- netgen


### Magic
![image](https://user-images.githubusercontent.com/49194847/138071384-a2c83ba4-3f9c-431a-98da-72dc2bba38e7.png)

 [Magic](http://opencircuitdesign.com/magic/) is a VLSI layout tool.

Install steps:
```
$  git clone git://opencircuitdesign.com/magic
$  cd magic
$	 ./configure
$  make
$  sudo make install
```


### Netgen
![image](https://user-images.githubusercontent.com/49194847/138073573-a819cc67-7643-4ecf-983d-454d99ec5443.png)

[Netgen](http://opencircuitdesign.com/netgen/) is a tool for comparing netlists, a process known as LVS, which stands for "Layout vs. Schematic". This is an important step in the integrated circuit design flow, ensuring that the geometry that has been laid out matches the expected circuit.
Install steps:
```
$  git clone git://opencircuitdesign.com/netgen
$  cd netgen
$	./configure
$  make
$  sudo make install
```


### Xschem
![image](https://user-images.githubusercontent.com/43693407/143311382-8cd3c1c9-dd07-4179-892d-52e9cf71e5a7.png)

[Xschem](http://repo.hu/projects/xschem/xschem_man/xschem_man.html) is a schematic capture program that allows to interactively enter an electronic circuit using a graphical and easy to use interface. When the schematic has been created a circuit netlist can be generated for simulation.

Install steps:
```
$  git clone https://github.com/StefanSchippers/xschem.git 
$ sudo apt-get install flex
$ sudo apt-get install bison
$ sudo apt-get install libxpm-dev
$ cd xschem.git
$ ./configure
$ make
$ sudo make install
```


### Ngspice
![image](https://user-images.githubusercontent.com/49194847/138070431-d95ce371-db3b-43a1-8dbe-fa85bff53625.png)

[Ngspice](http://ngspice.sourceforge.net/devel.html) is the open source spice simulator for electric and electronic circuits.
Install steps:<br />

After downloading the tarball from [https://sourceforge.net/projects/ngspice/files/](https://sourceforge.net/projects/ngspice/files/) to a local directory, unpack it using:
```
 $ tar -zxvf ngspice-37.tar.gz
 $ cd ngspice-37
 $ mkdir release
 $ cd release
 $ ../configure  --with-x --with-readline=yes --disable-debug
 $ make
 $ sudo make install
```


Please note that to view the simulation graphs of ngspice, xterm is required and can be installed using.
```
$ sudo apt-get update
$ sudo apt-get install xterm
```

### open_pdk

A process design kit (PDK) is a set of files used within the semiconductor industry to model a fabrication process for the design tools used to design an integrated circuit. The PDK is created by the foundry defining a certain technology variation for their processes. It is then passed to their customers to use in the design process.

The PDK we are going to use for this Project is Google Skywater-130 (130 nm) PDK.
![image](https://user-images.githubusercontent.com/49194847/138075630-d1bdacac-d37b-45d3-88b5-80f118af37cd.png)

Install steps:
```
$  git clone git://opencircuitdesign.com/open_pdks
$  open_pdks
$	./configure --enable-sky130-pdk
$  make
$  sudo make install
```


#### Installing ALIGN:

Use the following commands to install ALIGN tool.

```
export CC=/usr/bin/gcc
export CXX=/usr/bin/g++
git clone https://github.com/ALIGN-analoglayout/ALIGN-public
cd ALIGN-public
sudo apt -y install python3.10-venv
sudo apt-get -y install python3-pip
python3 -m venv general
source general/bin/activate
python3 -m pip install pip --upgrade
pip install pandas
pip install scipy
pip install nltk
pip install gensim
pip install setuptools wheel pybind11 scikit-build cmake ninja
pip install -v .
pip install -e .
pip install -v -e .[test] --no-build-isolation
pip install wheel setuptools pip --upgrade
pip3 install wheel setuptools pip --upgrade
pip install -v --no-build-isolation -e . --no-deps --install-option='-DBUILD_TESTING=ON'
```


#### Running ALIGN TOOL

Everytime we start running tool in new terminal run following commands.

```
python3 -m venv general
source general/bin/activate
```
Commands to run ALIGN (goto ALIGN-public directory)


```
mkdir work
cd work
```
General syntax to give inputs
```
schematic2layout.py <NETLIST_DIR> -p <PDK_DIR> -c
```

Running a EXAMPLE:
```
schematic2layout.py ../examples/telescopic_ota -p ../pdks/FinFET14nm_Mock_PDK/
```


![align](https://user-images.githubusercontent.com/118599201/218081159-ee9151d3-55d1-4a7c-b54f-7b16d566eb81.PNG)

![allign2](https://user-images.githubusercontent.com/118599201/218081170-3dff266b-c388-458e-98dc-8e220d381315.PNG)




### Verifiying the open_pdk installation
An initial working directory can be made by copying the required files as follows:
```
$ mkdir Lab1_and
$ cd Lab1_and
$ mkdir mag
$ mkdir netgen
$ mkdir xschem
$ cd xschem
$ cp /usr/local/share/pdk/sky130A/libs.tech/xschem/xschemrc .
$ cp /usr/local/share/pdk/sky130A/libs.tech/ngspice/spinit .spiceinit
$ cd ../mag
$ cp /usr/local/share/pdk/sky130A/libs.tech/magic/sky130A.magicrc .magicrc
$ cd ../netgen
$ cp /usr/local/share/pdk/sky130A/libs.tech/netgen//sky130A_setup.tcl .
```
Checking if magic works
![magic](https://user-images.githubusercontent.com/118599201/218081128-4bb784cd-d35d-4bb8-9968-9c845123b25e.PNG)
Checking if xschem works
![1](https://user-images.githubusercontent.com/118599201/218081254-72d2f874-ce10-4036-be1e-cdbe4d58e793.PNG)
Checking if netgen works
![netgen](https://user-images.githubusercontent.com/118599201/218081144-99bbcbba-b2af-4d8f-a7b1-7c5c08db3758.PNG)
Checking if ngspice works
![ngspice](https://user-images.githubusercontent.com/118599201/218081183-d16783d4-ef8d-4983-a94f-7acd80745416.PNG)
### Creating inverter schematic using xschem
An initial schematic is made by placing components from the open_pdk library<br />
The required changes to the properties of the device can be made here and will automatically reflect in the layout
![3](https://user-images.githubusercontent.com/118599201/218081860-e59c0b58-f5f7-4200-bf60-187776081a64.PNG)
Convert the schematic to a symbol

Using the symbol, we can create an independent test bench to simulate the circuit

#  Simulation of Inverter using Xschem and Ngspice
Invoke Xschem by typing `xschem` 


##  Pre-layout Simulation using Xschem and Ngspice

###  DC Analaysis of CMOS inverter

To create the inverter schematic in Xschem, follow these steps:

1 .Open Xschem and create a new schematic file.

2 . Add the PMOS and NMOS components to the schematic using the "Add Component" feature in Xschem.

3 .Assign the process corner information from the TT_MODELS to the PMOS and NMOS components in the schematic.

4 .Connect the PMOS and NMOS components to create the inverter circuit. The PMOS should be connected to the input and the NMOS should be connected to the output.

5 . Add the power supply and ground connections to the schematic.

6 .Verify that the connections and component values are correct and that the schematic meets the desired specifications.

7 .Save the schematic and test the inverter circuit using simulation tools in Xschem.

Note: The exact steps for creating the schematic in Xschem may vary depending on the version of the software and the specific process corner information in the TT_MODELS.
Contents of TT_MODELS
```
name= TT_MODELS1
only_toplevel=true
format="tcleval(@value)"
** opencircuitdesign pdks install
.lib $::SKYWATER_MODELS/sky130.lib.spice tt
"
spice_ignore=false
```
DC analysis is done by using the `.dc` command using `code_shown.sym` from components.
```
.dc Vin 0 1.8 0.01
.save all
```
The schematic is as shown.

![latest](https://user-images.githubusercontent.com/118599201/218233566-2a70767a-5008-4327-bf6d-9960362aa29d.PNG)


Go to `Options> Spice netlist` to set the netlist option. Click on `Netlist` from the menu to generate a spice file for the schematic created. Click on `Simulate` to run the simulation and obtain the voltage-transfer characteristic(VTC) for the inverter.

![plot](https://user-images.githubusercontent.com/118599201/218231976-f97e5396-cbb7-47d0-b54d-cad7ebdbdb50.PNG)


From the VTC.

$V_{OL}$= 0 V, $V_{IL}$= 750 mV V, $V_{IH}$= 921.8 mA V, $V_{OH}$= 1.8 V

Noise margins

NML = $V_{IL}$ - $V_{OL}$= 750 mV

NMH = $V_{OH}$ - $V_{IH}$= 878 mV


###  Transient Analaysis of CMOS inverter
The transient analysis of the inverter can be obtained by adding `.tran ` in the `code_shown.sym` block.

![tran](https://user-images.githubusercontent.com/118599201/218231984-986bd1ef-2f55-4b21-8993-e9a0a0757337.PNG)
Go to `Options> Spice netlist` to set the netlist option. Click on `Netlist` from the menu to generate a spice file for the schematic created. Click on `Simulate` to run the simulation and obtain the out vs time and in vs time.

![tran_out](https://user-images.githubusercontent.com/118599201/218231989-4a6c64a7-f42d-4301-a587-0bbf9a383415.PNG)
The graph shows the input and output variations with time.
![tran-0ut2](https://user-images.githubusercontent.com/118599201/218231992-e00a2f16-2d5d-461b-9af4-0de1233c9142.PNG)

The timing parameters are calculated as

Rise time = time(80 % of Vout) - time(20% of Vout)

Fall time = time(20 % of Vout) - time(80% of Vout)

Cell Rise Delay =Time taken by output to rise to its 50% value - Time taken by the input to fall to its 50% value

Cell Rise Delay =time taken by output to fall to its 50% value - time taken by the input to rise to its 50% value

The timing parameters obtained from pre-layout simulations is tabulated below.

rise time - 10ps
fall time - 10ps
pre-layout inverer delay values delay = 11.29ps

## Creation of Layout using inverter schematic in layout tool MAGIC
If you want to use the following command, you'll need to set up a working directory first. This directory should include the sky130A.tech, .xschemrc, and .sky130magicrc files. Alternatively, you can import these files directly into the MAGIC directory. Once you have set up the working directory, simply open it and enter the command.
```
'MAGIC -T sky130A.tech
```
This opens up the tkcon and layout windows.

In the Layout window import the spice netlist of your inverter(one which has pins and fets, and is the bottomost hierarchy of the inverter testbench)
The metal input and output pins are imported and the nfet and pfet is imported.

![symbol layout](https://user-images.githubusercontent.com/118599201/219821677-17b5ccb3-b216-45bc-b53d-c589f8678e3c.PNG)


To place pins and FETs in the desired location, hover over them and press "i" to select them, then press "m" to place them. Once you have placed them, use the metal1 layer to route the layout in a way that is free of design rule check (DRC) violations.

Identify and fix errors: Review the DRC results and identify any errors or violations that need to be fixed. This may include issues such as overlapping polygons, minimum feature size violations, or other errors.

Re-run DRC: After you have made changes to the layout, re-run the DRC to make sure that it is now DRC clean. Repeat the process until the layout passes the DRC without any errors.


![lay](https://user-images.githubusercontent.com/118599201/219821839-91ea69ce-7a44-4ef7-bcf6-47c64964be96.png)

Now, go to File --> save and select autowrite. Go to the  window and type the following:
```
extract do local
extract all
```
To extract files and save them to a local directory, use the instruction "Extract do local." Then, use the command "extract all" to actually perform the extraction.

If you specifically need to extract files for lvs in the spice format, run the following commands.

```
ext2spice lvs
ext2spice cthresh 0 rthresh 0
ext2spice
```

Now we need to use our pre-layout spice witht he post-layout parasitics netlist and perform spice simulations.
Paste the pre-layout netlist of inverter testbench into the magic generated inverter spice netlist

### Pre- Layout Inverter  Spice Netlist
```
** sch_path: /home/pranay/Desktop/vsd/Lab1_and/xschem/inverter_pr.sch
**.subckt inverter_pr vin vout
*.ipin vin
*.opin vout
x1 net1 vout vin GND inverter_symbol
Vdd net1 GND 1.8
.save i(vdd)
vin1 vin GND pulse(0 1.8 1ns 1ns 1ns 4ns 10ns)
.save i(vin1)
**** begin user architecture code

** opencircuitdesign pdks install
.lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt


.tran 0.01n 60n
.save all

**** end user architecture code
**.ends

* expanding   symbol:  inverter_symbol.sym # of pins=4
** sym_path: /home/pranay/Desktop/vsd/Lab1_and/xschem/inverter_symbol.sym
** sch_path: /home/pranay/Desktop/vsd/Lab1_and/xschem/inverter_symbol.sch
.subckt inverter_symbol vdd vout vin vss
*.opin vout
*.ipin vin
*.iopin vdd
*.iopin vss
XM2 vout vin vdd vdd sky130_fd_pr__pfet_01v8 L=0.15 W=1 nf=1 ad='int((nf+1)/2) * W/nf * 0.29' as='int((nf+2)/2) * W/nf * 0.29'
+ pd='2*int((nf+1)/2) * (W/nf + 0.29)' ps='2*int((nf+2)/2) * (W/nf + 0.29)' nrd='0.29 / W' nrs='0.29 / W'
+ sa=0 sb=0 sd=0 mult=1 m=1
XM1 vout vin vss vss sky130_fd_pr__nfet_01v8 L=0.15 W=1 nf=1 ad='int((nf+1)/2) * W/nf * 0.29' as='int((nf+2)/2) * W/nf * 0.29'
+ pd='2*int((nf+1)/2) * (W/nf + 0.29)' ps='2*int((nf+2)/2) * (W/nf + 0.29)' nrd='0.29 / W' nrs='0.29 / W'
+ sa=0 sb=0 sd=0 mult=1 m=1
.ends

.GLOBAL GND
.end
```
After extracting the netlist from the Magic Tool, selectively paste the desired section into the inverter.spice file. The resulting netlist in inverter.spice will look like this:
```
** sch_path: /home/pranay/Desktop/vsd/Lab1_and/xschem/inverter_pr.sch
**.subckt inverter_pr vin vout
*.ipin vin
*.opin vout
x1 net1 vout vin GND inverter_symbol
Vdd net1 GND 1.8
.save i(vdd)
vin1 vin GND pulse(0 1.8 1ns 1ns 1ns 4ns 10ns)
.save i(vin1)
**** begin user architecture code

** opencircuitdesign pdks install
.lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt



.tran 0.01n 60n
.save all

**** end user architecture code
**.ends

* expanding   symbol:  inverter_symbol.sym # of pins=4
** sym_path: /home/pranay/Desktop/vsd/Lab1_and/xschem/inverter_symbol.sym
** sch_path: /home/pranay/Desktop/vsd/Lab1_and/xschem/inverter_symbol.sch
.subckt inverter_symbol vdd vout vin vss
*.opin vout
*.ipin vin
*.iopin vdd
*.iopin vss
XM2 vout vin vdd vdd sky130_fd_pr__pfet_01v8 L=0.15 W=1 nf=1 ad='int((nf+1)/2) * W/nf * 0.29' as='int((nf+2)/2) * W/nf * 0.29'
+ pd='2*int((nf+1)/2) * (W/nf + 0.29)' ps='2*int((nf+2)/2) * (W/nf + 0.29)' nrd='0.29 / W' nrs='0.29 / W'
+ sa=0 sb=0 sd=0 mult=1 m=1
XM1 vout vin vss vss sky130_fd_pr__nfet_01v8 L=0.15 W=1 nf=1 ad='int((nf+1)/2) * W/nf * 0.29' as='int((nf+2)/2) * W/nf * 0.29'
+ pd='2*int((nf+1)/2) * (W/nf + 0.29)' ps='2*int((nf+2)/2) * (W/nf + 0.29)' nrd='0.29 / W' nrs='0.29 / W'
+ sa=0 sb=0 sd=0 mult=1 m=1
.ends

.GLOBAL GND
.end

.subckt inverter vin vout vdd vss
X0 Y A VP XM1/w_n211_n319# sky130_fd_pr__pfet_01v8 ad=2.9e+11p pd=2.58e+06u as=2.9e+11p ps=2.58e+06u w=1e+06u l=150000u
X1 Y A VN VSUBS sky130_fd_pr__nfet_01v8 ad=2.9e+11p pd=2.58e+06u as=2.9e+11p ps=2.58e+06u w=1e+06u l=150000u
C0 A XM1/w_n211_n319# 0.33fF
C1 VP XM1/w_n211_n319# 0.21fF
C2 VN A 0.33fF
C3 Y XM1/w_n211_n319# 0.14fF
C4 VN Y 0.28fF
C5 VP A 0.29fF
C6 A Y 0.10fF
C7 VP Y 0.24fF
C8 A VSUBS 0.73fF
C9 Y VSUBS 0.90fF
C10 VN VSUBS 0.98fF
C11 VP VSUBS 0.71fF
C12 XM1/w_n211_n319# VSUBS 1.11fF 

.ends
```
Invoke inverter.spice with ngspice
```
ngspice inverter.spice
```
run the following commands
```
plot vin vout
```
plot vout vs vin is
To obtain the required delay (post-layout), follow these steps:

Right-click on the plots for "vin" and "vout."
Click and drag to expand the plots until the "vin" and "vout" pulses are far apart.
Select the 50% rise points (approximately) and expand at those points.
Click on the two plots to display the x-coordinate (time) and y-coordinate (voltage) on ngspice.
Subtract the x-coordinates to get the required delay.

![out](https://user-images.githubusercontent.com/118599201/219823467-7d43b80d-55c9-4f52-9062-318d9d34420d.PNG)

Post Layout Delay = 1.02765 - 1.01551 = 0.01214 (12.14 ps)

### Inspection of pre-LAYOUT  and post-LAYOUT

Input pulse specification in both 
- Rise Time- 10ps
- Fall Time- 10ps
- Pre-Layout Delay Vout-Vin - 11.29ps
- Post-Layout Delay Vout-Vin - 12.14ps



# Week -3

## Intalling OpenROAD

OpenROAD is an integrated chip physical design tool that takes a design from synthesized Verilog to routed layout.

An outline of steps used to build a chip using OpenROAD is shown below:

- Initialize floorplan - define the chip size and cell rows
- Place pins (for designs without pads )
- Place macro cells (RAMs, embedded macros)
- Insert substrate tap cells
- Insert power distribution networkInstalling OpenROAD involves the following steps:

Install the required dependencies such as Tcl/Tk, GCC, Boost, and CMake.
Clone the OpenROAD repository using Git.
Build and install the dependencies for OpenROAD.
Configure and build OpenROAD using CMake.
Once OpenROAD is installed, the following steps are typically used to build a chip:

Initialize the floorplan by defining the chip size and cell rows.
Place pins for designs without pads.
Place macro cells such as RAMs and embedded macros.
Insert substrate tap cells.
Insert the power distribution network.
Place macro cells using macro placement.
Place standard cells using global placement.
Repair violations and optimize timing.
Perform clock tree synthesis.
Optimize setup/hold timing.
Insert fill cells.
Perform global routing and route guides for detailed routing.
Repair any antenna violations.
Perform detailed routing.
Perform parasitic extraction.
Perform static timing analysis.



## Steps to install openROAD are

Here are the steps to install OpenROAD:

Install dependencies: OpenROAD has several dependencies that need to be installed before installation. These include Git, CMake, Python 3, Tcl, and other tools. Install these dependencies using your system's package manager.

Clone OpenROAD repository: Clone the OpenROAD repository from GitHub using the following command:

git clone https://github.com/The-OpenROAD-Project/OpenROAD.git
Build OpenROAD: Change to the OpenROAD directory and build the project using CMake and Make. The following commands will build OpenROAD:


cd OpenROAD
mkdir build
cd build
cmake ..
make
Set up environment variables: Add the OpenROAD binaries to your PATH and set the OPENROAD environment variable to point to the OpenROAD installation directory. The following commands will set up the environment variables:

export PATH=$PATH:/path/to/OpenROAD/bin
export OPENROAD=/path/to/OpenROAD
Verify installation: Verify that OpenROAD is installed correctly by running the following command:


openroad -version
This should print the version number of OpenROAD.

Once OpenROAD is installed, you can use it to perform chip physical design tasks, such as floorplanning, placement, routing, and static timing analysis.



```
cd
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD.git
cd OpenROAD
sudo ./etc/DependencyInstaller.sh
cd
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
./build_openroad.sh –local
export OPENROAD=~/OpenROAD-flow-scripts/tools/OpenROAD
export PATH=~/OpenROAD-flow-scripts/tools/install/OpenROAD/bin:~/OpenROAD-flow-scripts/tools/install/yosys/bin:~/OpenROAD-flow-scripts/tools/install/LSOracle/bin:$PATH
```

## Intalling OpenFASOC

git clone https://github.com/magroski/OpenFASoC.git
This will create a local copy of the OpenFASoC repository on your machine.

Next, navigate into the OpenFASoC directory and run the setup script:

cd OpenFASoC
./setup.sh
This will download and install the required dependencies, including OpenROAD and other EDA tools.

Once the setup is complete, you can generate a sample design by running the following command:
go

make gen_skel
This will generate a skeleton design with a simple ring oscillator. You can modify the design specifications in the config.mk file and run make to generate a custom design.

To run the full OpenFASoC flow on your custom design, run:
go

make openfasc
This will run the entire design flow, from Verilog generation to GDSII layout. The final output will be a GDSII file located in the output directory.

Note that the OpenFASoC flow is highly customizable, and you can modify various aspects of the design flow by editing the configuration files in the config directory.


OpenFASoC is a project focused on automated analog generation from user specification to GDSII with fully open-sourced tools. It is led by a team of researchers at the University of Michigan and is inspired from FASoC which sits on proprietary software.

The tool is comprised of analog and mixed-signal circuit generators, which automatically create a physical design based on user specifications.


- First, cd into a directory of your choice and clone the OpenFASoC repository:

`git clone https://github.com/idea-fasoc/openfasoc`

- Now go to the home location of this repository (where the README.rst file is located) and run `sudo ./dependencies.sh.`
- Or follow the below steps
```
cd
git clone https://github.com/idea-fasoc/openfasoc
cd openfasoc
sudo ./dependencies.sh

```
# Temperature Sensor Auxiliary Cells
An overview of how the Temperature Sensor Generator (temp-sense-gen) works internally in OpenFASoC

## Circuit
This generator creates a compact mixed-signal temperature sensor based on the topology from this paper. It consists of a ring oscillator whose frequency is controlled by the voltage drop over a MOSFET operating in subthreshold regime, where its dependency on temperature is exponential.

![001](https://user-images.githubusercontent.com/118599201/222863355-95233bcc-183f-4009-8086-7a3a812e22a6.png)

The physical implementation of the analog blocks in the circuit is done using two manually designed standard cells:

1. HEADER cell, containing the transistors in subthreshold operation;

2. SLC cell, containing the Split-Control Level Converter.

The gds and lef files of HEADER and SLC cells are pre-created before the start of the Generator flow.

The layout of the HEADER cell is shown below:
![01](https://user-images.githubusercontent.com/118599201/222858509-95dbf998-2c9f-401b-8c36-9aeec45be659.png)

The layout of the SLC cell is shown below:

![02](https://user-images.githubusercontent.com/118599201/222858514-8349ff9e-a589-4d0f-b534-2c64ac573a6b.png)


## OpenFASOC flow for Temperature Sensor Generation
To modify the specifications of a circuit, you can edit the test.json specfile located in the generators/temp-sense-gen/ folder.

To generate the default circuit, navigate to the openfasoc/generators/temp-sense-gen/ directory and execute the following command:

make sky130hd_temp

The generation of the default circuit's physical design can be split into three stages: Verilog generation, RTL-to-GDS flow using OpenROAD, and post-layout verification involving DRC and LVS.

### Verilog Generation
For Verilog generation, execute the command make sky130hd_temp_verilog. The generator first parses the user's requirements into a high-level circuit description or verilog by reading from a JSON spec file located in the temp-sense-gen repository. The JSON file allows for specifying power, area, maximum error (temperature result accuracy), optimization option (to choose which option to prioritize), and operating temperature range (minimum and maximum operating temperature values). The operating temperature range and optimization must be specified, while other items can be left blank.

The generator uses this model file to determine the necessary modifications to meet the specifications, such as the number of headers and slc. The generator references the model file in an iterative process until it meets the specifications or fails. It then substitutes the specifics into the template verilog files to generate a verilog description.

The test.json file shown in the screenshot below corresponds to the temp_sense_gen.

![03](https://user-images.githubusercontent.com/118599201/222858517-2b060900-74dc-46c4-b5c0-5887ed5cba71.png)

The generator calculates the required number of headers and inverters to minimize the error based on the operating temperature range. Temperature is varied from -20 C to 100 C in this file.

To begin the generation process, the generator uses a Verilog template of the temperature sensor circuit located in temp-sense-gen/src/. The template files contain lines marked with @@, which are replaced based on the specifications.

To replace these lines with the correct circuit elements, temp-sense-gen utilizes cells from the selected technology. The nearest-neighbor approach with experimental data from models/modelfile.csv is used to optimize the number of inverters in the ring oscillator and HEADER cells in parallel.

![04](https://user-images.githubusercontent.com/118599201/222859730-9c398538-ad5a-421b-86f4-38804a8cf76c.png)

As depicted in the provided image, the tool aims to minimize the error by iteratively adjusting the number of inverters and headers for the specified temperature range.

Upon executing the make sky130hd_temp_verilog command, the verilog files for counter.v, TEMP_ANALOG_hv.nl.v, TEMP_ANALOG_lv.nl.v are generated in the src folder.

Using the generic template, additional blocks of counter, TEMP_ANALOG_hv.nl.v, TEMP_ANALOG_lv.nl.v are produced in the src folder. For a temperature range of -20C to 100C, the generator determines that three header files are necessary to meet the specified requirements.

### Synthesis

The OpenROAD Flow commences with a flow configuration file, config.mk, as well as the chosen platform (such as sky130hd) and Verilog files that were generated in the previous steps.

Next, Yosys is used to conduct synthesis, which identifies the appropriate circuit implementation from the cells available in the platform.

### Floorplan
To generate the floorplan for the physical design, OpenROAD is utilized. It necessitates a description of the power delivery network in pdn.cfg.

The temperature sensor design comprises two voltage domains: VDD, which powers the majority of the circuit, and VIN, which powers the ring oscillator and serves as the output of the HEADER cells. The floorplan.tcl script is used to create these voltage domains.

Lastly, the script read_domain_instances.tcl is sourced, which assigns the relevant instances to the TEMP_ANALOG domain group. It obtains the required instances from the DOMAIN_INSTS_LIST variable, which is defined as tempsenseInst_domain_insts.txt in the config.mk file. This step ensures that the cells are placed in the correct voltage domain during the detailed placement phase.


The tempsenseInst_domain_insts.txt file contains all instances to be placed in the TEMP_ANALOG domain (VIN voltage tracks). These cells are the components of the ring oscillator, including the inverters whose quantity may vary according to the optimization results. Thus, this file actually gets generated during temp-sense-gen.py.

### Placement
Placement takes place after the floorplan is ready and has two phases: global placement and detailed placement. The output of this phase will have all instances placed in their corresponding voltage domain, ready for routing.

### Routing
The routing process consists of two phases: global routing and detailed routing. Just before global routing, OpenFASoC runs the pre_global_route.tcl script.

The script sources two additional files. The first file, add_ndr_rules.tcl, includes an NDR (Non-Default Rules) rule to the VIN net to optimize the connections between the voltage domains. The second file, create_custom_connections.tcl, establishes the link between the VIN net and the HEADER instances.

#### Final layout after routing:

![05](https://user-images.githubusercontent.com/118599201/222860726-f6e6de20-2c94-469a-9a5b-71025a642776.png)

### Post-layout verification

Once the design is generated, OpenFASoC proceeds to run DRC (Design Rule Check) and LVS (Layout vs Schematic) to ensure that the circuit is both manufacturable and aligns with the specified design. The flow/Makefile runs the targets magic_drc and netgen_lvs using make.

In DRC, Magic software utilizes the generated GDS file to verify if there are any violations of constraints. If there are any errors, a report is written and stored under temp-sense-gen/flow/reports/.

In LVS, Magic extracts the netlist of the generated GDS file and compares it with the original circuit netlist to ensure that the physical implementation was done correctly. Files generated from the layout extraction are stored under temp-sense-gen/flow/objects/.


# Week -4

# 4-bit asynchronous up counter

A 4-bit asynchronous up counter is a digital circuit that can count from 0 to 15. The counter consists of four flip-flops, each representing one bit of the counter, and a combinational circuit that performs the counting operation.

At the rising edge of the clock signal, the first flip-flop receives the input signal and stores it. The output of the first flip-flop is then fed as an input to the second flip-flop, which stores the previous state and increments its own output by one. This process continues until the fourth flip-flop increments its output by one, resulting in the counter reaching the maximum value of 15.

The asynchronous aspect of the counter means that each flip-flop operates independently, allowing the counter to increment without relying on a global clock signal. However, this can also lead to glitches in the output if the flip-flops do not transition at the same time. To prevent glitches, a synchronization circuit can be added to the counter.
 
 ## Circuit diagram of 4-Bit Asynchronous Up Counter
 
 ![image](https://user-images.githubusercontent.com/83575446/222718876-f0587a88-4bea-47db-81c7-81cf1c3d421f.png)

## Implementation of ring oscillator in Xschem



## Post layout characterization of ring oscillator using ALIGN






Magic Tool is used to post layout Spice file. SPICE file can be simulated in NGSPICE and compare with prelayout.

- Open terminal in work directory where our final gds stored.

set PDK ROOT for Magic using the command -
```
export PDK_ROOT=/path/to/your/pdks/
```

Then type `magic` in terminal which open magic.

- Then goto file and press read GDS and select our gds file
- Place the curser outside layout press `s` which select entire layout.
- Then goto tkcon and type `ext2spice`
- post layout spice file is created in work directory

