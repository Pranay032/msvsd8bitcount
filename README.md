# msvsd8bitcount
## WEEK-0 - Getting the tools

To adequately utilize the open source skywater130 pdk and understand the design flow, we first require to install all the tools, which are
- open_pdk
- magic
- ngspice
- xschem
- netgen


### Magic
Magic is an open-source VLSI layout tool.<br /><br />
Install steps:
```
$  git clone git://opencircuitdesign.com/magic
$  cd magic
$	 ./configure
$  make
$  sudo make install
```
More info can be found at [http://opencircuitdesign.com/magic/index.html](http://opencircuitdesign.com/magic/index.html)

### Netgen
Netgen is a tool for comparing netlists, a process known as LVS, which stands for "Layout vs. Schematic" <br /><br />
Install steps:
```
$  git clone git://opencircuitdesign.com/netgen
$  cd netgen
$	./configure
$  make
$  sudo make install
```
More info can be found at [http://opencircuitdesign.com/netgen/index.html](http://opencircuitdesign.com/netgen/index.html)

### Xschem
Xschem is a schematic capture program <br /><br />
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
More info can be found at [http://repo.hu/projects/xschem/index.html](http://repo.hu/projects/xschem/index.html)

### Ngspice
ngspice is the open-source spice simulator for electric and electronic circuits.<br /><br />
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
More info can be found at [https://ngspice.sourceforge.io/index.html](https://ngspice.sourceforge.io/index.html)

Please note that to view the simulation graphs of ngspice, xterm is required and can be installed using.
```
$ sudo apt-get update
$ sudo apt-get install xterm
```

### open_pdk

Open_PDKs is distributed with files that support the Google/SkyWater sky130 open process description [https://github.com/google/skywater-pdk](https://github.com/google/skywater-pdk). Open_PDKs will set up an environment for using the SkyWater sky130 process with open-source EDA tools and tool flows such as magic, qflow, openlane, netgen, klayout, etc.<br /><br />
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

### Creating and simulating testbench Schematic
The circuit can be simulated in ngspice. *make sure to disable .subckt in the simulation tab for the netlist generated for the sim*

### Creating inverter layout in Magic and exporting its netlist
The original schematic can be used to export a netlist, which can be imported into magic to create the layout.
