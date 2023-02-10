# msvsd8bitcount
## Chapter 0 - Getting the tools

To adequately utilize the open source skywater130 pdk and understand the design flow, we first require to install all the tools, which are
- open_pdk
- magic
- ngspice
- xschem
- netgen
![toolchains](Resources/Lab1/toolchains.png)

*Note: open_pdk has to be installed last so it can correctly associate the xschem and magic directories.*
*Note: if the configure step fails during any process, its most likely due to missing additional packages, and they need to be installed (preferably from source) to complete the installation*

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
$  git clone https://github.com/StefanSchippers/xschem.git xschem_git
$	./configure
$  make
$  sudo make install
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

Now that we have all the necessary tools installed let's understand the design flow!
## Chapter 1 - Understanding the design flow

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
```Checking if magic works
![mag_test](Resources/Lab1/mag_test.png)<br /><br />
Checking if xschem works
![xschem_test](Resources/Lab1/xschem_test.png)<br /><br />
Checking if netgen works
![netgen_test](Resources/Lab1/netgen_test.png)<br /><br />
Checking if ngspice works
![spice_test](Resources/Lab1/spice_test.png)<br /><br />
### Creating inverter schematic using xschem
An initial schematic is made by placing components from the open_pdk library<br />
The required changes to the properties of the device can be made here and will automatically reflect in the layout
![xshem_inv](Resources/Lab1/xshem_inv.png)<br /><br />
Convert the schematic to a symbol
![xshem_sym](Resources/Lab1/xshem_sym.png)<br /><br />
Using the symbol, we can create an independent test bench to simulate the circuit
![xshem_tb](Resources/Lab1/xshem_tb.png)<br /><br />
### Creating and simulating testbench Schematic
The circuit can be simulated in ngspice. *make sure to disable .subckt in the simulation tab for the netlist generated for the sim*
![xschem_sim](Resources/Lab1/xschem_sim.png)
### Creating inverter layout in Magic and exporting its netlist
The original schematic can be used to export a netlist, which can be imported into magic to create the layout. <br /><br />
