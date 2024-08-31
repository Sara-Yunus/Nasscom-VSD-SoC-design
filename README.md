# Nasscom Certification Program in Digital VLSI SoC design and planning 
### (21st Aug'24 - 3rd Sept'24)

## Day-1 Objective: TO CALCULATE THE FLOP RATIO

Invoking the OpenLane tool.
```bash
# Change the directory to a required location using the following command in the terminal.
cd Desktop/work/tools/openlane_working_dir/openlane
```

Opening the OpenLane tool
```
# type only the below text
docker
```
Setting up OpenLane in interactive mode
```
# type the below given
./flow.tcl -interactive
# it'll turn the prompt to a percentage symbol
```
img1



Importing all the packgaes to work with
```
# to get ready to execute the command
% package require openlane 0.9
```
Preparing Design files
```
# for working on the design of Picorv32 Processor
% prep -design picorv32a
```
img2

A new file named "runs" is created in picorv32a directory. 
This 'runs' file contains a directory named with the date of its creation. Here 31-08_20-19. 
img3



#### Running the synthesis stage

``` run_synthesis```

img4


The synthesised netlist is now under the synthesis in the 'results' file of the 'runs' folder.

img5

The timing reports will be preset in the reports directory.
The last report generated in the YOSYS gives you the actual statistic report.

img6

##### Finding the flip-flop ratio. 

img7

```math
Flip  flop  Ratio = {No. of D  flip  flops}/{Total  no.  of  cells} = {1613}/{14876} = 0.108429
```
```math
Percentage of D flip-flops = 0.108429 * 100 = 10.8429%
```

## Day-2 Objective: FINDING DIE AREA

Modifying the design-related config.tcl 

* using ``` less ``` to view the text file
* using ``` vim ``` to edit the text file

  img8

 For editing the text file:
 * use ``` i ``` to start editing
 * use ``` esc ``` to halt editing
 * use ``` :wq ``` to save the changes and exit the editable text file
 * use ``` q ``` to exit the non-editable text file

System Default values of FP_IO_VMETAL, FP_IO_HMETAL & FP_CORE_UTIL can be found in 
``` ../Desktop/work/tools/openlane_working_dir/openlane/configurations$ less floorplan.tcl ```
img9

Modifications are shown in the below image
img10

After making the changes to the variables we can run floorplan in the OpenLane flow
``` run_floorplan ```
This creates a .def file in the ../runs/31-08_20-19/results/floorplan 
img11

// To check whether the changes made in the config.tcl has taken precedence over the system default values or not we go to 



Now, viewing the floorplan in Magic layout tool.
img12
img13
img14
img15


``` run_placement ```
img16
img17
img18

##### Finding the die area.

img19




## Day-3 Objective: PLUGGING-IN CUSTOM INVERTER CELL IN THE OPENLANE FLOW.

Command to clone the got repo in the openlane directory
``` gIt clone ```

This will create a folder called vsdstdcelldesign in the directory
img20

copying .tech file to the vsdstdcelldesign folder add sky130A.tech to it
img21

Opening the inverter layout in Magic layout tool
img22

Extracting the inverter in .spice file
1- ``` pwd ```
2- ``` extract all ```
3- ``` ext2spice cthresh 0 rthresh 0 ```
4- ``` ext2spice ```
img23



Modifying the sky130_inv.spice with the below given SPICE DECK
```
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=0.01u
.include ./libs/pshort.lib
.include ./libs/nshort.lib

//.subckt sky130_inv A Y VPWR VGND
M1001 Y A VGND VGND nshort_model.0 ad=1435 pd=152 as=1365 ps=148 w=35 l=23
M1000 Y A VPWR VPWR pshort_model.0 ad=1443 pd=152 as=1517 ps=156 w=37 l=23
VDD VPWR 0 3.3V
VSS VGND 0 0V
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)
C0 A Y 0.05fF
C1 Y VPWR 0.11fF
C2 A VPWR 0.07fF
C3 Y 0 2fF
C4 VPWR 0 0.59fF
//C5 VPWR VGND 0.781f
//.ends
.tran 1n 20n
.control
run
.endc
.end
```

Running the SPICE simulation in Ngspice tool
``` ngspice sky130_inv.spice ```

plotting the transient response 
``` plot y vs time a ```
img24
 
img25

##### Characterising the Inverter cell

Finding the values of 
* rise transition
* fall transition
* cell rise propagation delay
* cell fall propagation delay
