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
![img1](https://github.com/user-attachments/assets/796579a9-10a1-49aa-9342-fbacb622acdb)



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

max value = 3.3V
min value = 0V

  **Rise Transition** - Time taken for the output waveform to go to 20% of its max value from 80% of its max value.

  @ 20% 0f 3.3V : x0 = 2.18001e-09, y0 = 0.660167
  @ 80% 0f 3.3V : x0 = 2.24569e-09, y0 = 2.63983
  Rise time = 0.06568 ns

  **Fall Transition** - Time taken for the output waveform to go to 80% of its max value from 20% of its max value.

  @ 80% 0f 3.3V : x0 = 4.05262e-09, y0 = 2.64
  @ 20% 0f 3.3V : x0 = 4.09507e-09, y0 = 0.659574
  Fall time = 0.04245 ns

  **Cell Rise Delay** - Time difference between input and output at 50% of max value when the output is rising.

  output @ 50% of 3.3V : x0 = 2.21008e-09, y0 = 1.59948
  input @ 50% of 3.3V : x0 = 2.15202e-09, y0 = 1.59948
  Cell Rise time = 0.05806 ns

  **Cell Fall Delay** - Time difference between input and output at 50% of max value when the output is falling.

  output @ 50% of 3.3V : x0 = 4.07822e-09, y0 = 1.60063
  input @ 50% of 3.3V : x0 = 4.04851e-09, y0 = 1.60063
  Cell Rise time = 0.02971 ns

  ###### Creating the .lef file

  We find the tracks info for the standard cell in the following location 
  ``` ..pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd$ less tracks.info ```
  
  img26

  Activated grid in the layout in shown in below image
  -pressing ' g ' activates the grid in the Magic layout

  img27

  Converging grid with the tracks info
  
  -in the tkcon window:
  * ``` help grid ```
  * ``` grid 0.46um 0.34um 0.23um 0.17um ```
  img28

Giving a new name to our custom cell and generating .lef file

-in the tkcon window: 
* ``` save sky130A_vsdinv.mag ```
* ``` lef write ```
A new .lef file will be added in vsdstdcelldesign directory
img29

Adding the .lef file to the design src file
img30

Adding the 3 .lib files to the design src file
img31 

modifying design related config.tcl to the below given content
```

```

 img33

 img34


 Running floorplan

 As we completed with synthesis stage, now we need to perform floorplan by using the following commands

init_floorplan

place_io

tap_decap_or

img35

img36


Running placement 

img37
img38


Placement layout
img39

expanded
img40

### Lab Introduction to Magic tool options and DRC rules

Downloading the lab files using the below given command in the home directory
``` wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz ```

Unzipping the downloaded zip file using the below command 
``` tar xfz drc_tests.tgz ```
img82

.magicrc file below
img83

Opening Magic tool
``` magic -d XR & ``` this will open Magic with an empty window shown below
img84
with this, go to File option in the menu and select Open. 
img85
from here, select met3.mag and open
 *OR*
 in the terminal pass ``` magic -d XR met3 & ```
 img86

 select the area of any of the elements and using ``` drc why ``` in the tkcon window will tell which drc rule got violated for that design
 img87

For metal3 all rules are related to drawn layers, except for rule m3.4
 Now, select an area in the gui and guide the pointer on to the metal 3 layer and press P. The selected region will be filled with metal 3. 
 Now in console window type the command ``` cif see VIA2 ``` , The metal 3 filled area will be filled VIA2 mask.

 
Incorrectly implemented poly.9 simple rule correction
img1`







##  Day-4 Objective: OPTIMISING SYNTHESIS FOR SLACK VIOLATION AND PERFORMING CTS.

created a file called ``` pre_sta.conf ``` in openlane directory using ``` vim ``` command.
img41


Then created a file called ``` my_base.sdc ``` in ..design/picorv32/src using ``` vim ``` command.
img42

then 
``` .../openlane$ sta pre_sta.conf ```
img43
the slack violation value is same as what it was in synthesis step(img33)

Optimisng the design for slack reduction

Noticed that the net instance _10566_ has a fanout of 4 and is driven by or gate of size 3_4
img44

so replacinf this instance with _13165_ or gate of size 3_4
img45

with this we observe reduction in slack from -23.89 to -23.5184
img46


Noticed that the or gate of driving strenth 2 driving OA gate has more delay
img47

so replacing this instance with _13132_ or gate of size 4_4
img48 

with this we observe reduction in slack from -23.5184 to -23.5119
img49

So,now earlier the slack was -23.89 now it is -23.5119. That is, the slack has been reduced by 0.3781
// -- command to verify that the instance _13132_ has been replaced from or4_2 to or4_4 --
// ``` report_checks -from _26365_ -to _27762_ -through _13132 ```

Replacing the old netlist with the new one post the fixes done above

```
//making a copy of old netlist
.../results/synthesis$ cp picorv32a.synthesis.v picorv32a.synthesis_old.v
```
img50

NOW, overwriting the original netlist with the modified by passing the below command in OpenSTA flow itself 
```
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/25-03_18-52/results/synthesis/picorv32a.synthesis.v
```
Updation of netlist can be verified by the new time indicated prior to verilog file 
img51

Verification that the instance _13132_ is replaced to sky130_fd_sc_hd__or4_4
img52

NOW RUNNING FLOORPLAN ON THE UPDATED NETLIST IN THE **OPENLANE** FLOW
img53
RUNNING PLACEMENT 
img54

RUNNING CTS
``` run_cts ```
a new addition to ..results/synthesis directory 
img55

clock buffer can be viewed in CTS layout in Magic
img56


Timing analysis post-CTS( USING OpenROAD)
```
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/24-03_10-03/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts.db
```
creation of database can be verified in openlane directory
img57

```
# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# In case of error while reading liberty
read_liberty /openLANE_flow/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Check syntax of 'report_checks' command
help report_checks

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```
img58

img59

hold slack met
img60

setup slack met 
img61


###### Checking how timing gets affected if I modify the buffer list

```
# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Removing 'sky130_fd_sc_hd__clkbuf_1' from the list
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Checking current value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Setting def as placement def
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/placement/picorv32a.placement.def

# Run CTS again
run_cts

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/24-03_10-03/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts1.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Report hold skew
report_clock_skew -hold

# Report setup skew
report_clock_skew -setup

# Exit to OpenLANE flow
exit

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Inserting 'sky130_fd_sc_hd__clkbuf_1' to first index of list
set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)
```
effect on hold slack
img62

effect on setup slack
img63

img64


## Day 5- 

Need to run flow till CTS from start.

```
cd Desktop/work/tools/openlane_working_dir/openlane

docker

# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Following commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

# Now we are ready to run placement
run_placement

# Incase getting error
unset ::env(LIB_CTS)

# With placement done we are now ready to run CTS
run_cts

# Now that CTS is done we can do power distribution network
run_power_grid_generation

//gen_pdn
```
img65
img66

Magic Layout for generated PDN
img67
img68

``` run_routing ```
img69
img70
img71


Route layout in Magic
img72 to 75

Screenshot of fast route guide present in openlane/designs/picorv32a/runs/26-03_08-45/tmp/routing directory
img76


####### Post-Route parasitic extraction using SPEF extractor.
img77

timing analysis
img78 
img79

###### Post-Route OpenSTA timing analysis with the extracted parasitics of the route.
```
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/02-09_18-54/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/02-09_18-54/results/routing/picorv32a.def

# Creating an OpenROAD database to work with
write_db pico_route.db

# Loading the created database in OpenROAD
read_db pico_route.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/02-09_18-54/results/synthesis/picorv32a.synthesis_preroute.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Read SPEF
read_spef /openLANE_flow/designs/picorv32a/runs/02-09_18-54/results/routing/picorv32a.spef

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```
hold slack
img80

setup slack
img81
