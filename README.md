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
# to type only the below text
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
![img2](https://github.com/user-attachments/assets/d9cd0cce-a996-425e-9606-09f28ab97c6a)

A new file named "runs" is created in picorv32a directory. 
This 'runs' file contains a directory named with the date of its creation. Here 31-08_20-19. 
![img3](https://github.com/user-attachments/assets/2c0b4107-0cc9-4382-893f-a2ac5bf57f4c)



#### Running the synthesis stage

``` run_synthesis```

![img4](https://github.com/user-attachments/assets/fe0f8adb-868e-47a4-ac5b-f6ccccc58ce9)


The synthesised netlist is now under the synthesis in the 'results' file of the 'runs' folder.

![img5](https://github.com/user-attachments/assets/fef9121c-0ab1-45c8-b31a-e5ea5ffff974)

The timing reports will be preset in the reports directory.
The last report generated in the YOSYS gives the actual statistic report.

![img6](https://github.com/user-attachments/assets/346cd28f-652d-4196-99fe-939a655e6dea)

### Finding the flip-flop ratio. 

![img7](https://github.com/user-attachments/assets/b17dd5e5-97de-474c-8364-2ee56eb74dc9)

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

![img8](https://github.com/user-attachments/assets/04e41554-f846-473e-aa9b-636909a2d612)

 For editing the text file:
 * use ``` i ``` to start editing
 * use ``` esc ``` to halt editing
 * use ``` :wq ``` to save the changes and exit the editable text file
 * use ``` q ``` to exit the non-editable text file

System Default values of FP_IO_VMETAL, FP_IO_HMETAL & FP_CORE_UTIL can be found in 
``` ../Desktop/work/tools/openlane_working_dir/openlane/configurations$ less floorplan.tcl ```
![img9](https://github.com/user-attachments/assets/d9c50305-5850-4e82-9490-1cc0e55a1803)

Modifications are shown in the below image
![img10](https://github.com/user-attachments/assets/6b98d387-e529-47c9-9955-ffd16f8545a9)

After making the changes to the variables we can run floorplan in the OpenLane flow
``` run_floorplan ```
This creates a .def file in the ../runs/31-08_20-19/results/floorplan 
![img11](https://github.com/user-attachments/assets/69711fb8-a29b-443a-98cf-2febce408894)

// To check whether the changes made in the config.tcl have taken precedence over the system default values or not we go to 
``` ...logs/floorlan ``` in the date named directory.



Now, viewing the floorplan in Magic layout tool.
![img12](https://github.com/user-attachments/assets/317c2ccf-5d69-4446-adcb-6928f63fcbc4)
![img13](https://github.com/user-attachments/assets/2160e1de-7e95-4fa7-85e2-52cde4a0918d)
![img14](https://github.com/user-attachments/assets/a081d071-9d2e-4b2a-b971-1549381eb009)
![img15](https://github.com/user-attachments/assets/26c37732-0f55-41cb-8203-c134aaa2002b)

#### Finding the die area.

![img19](https://github.com/user-attachments/assets/ca967d9f-3628-4e8e-a324-686f7f03ee84)


After floorplan, next stage is placement. We do ``` run_placement ``` in openlane flow. 
As a result of placement a .def is created in ``` ...results/placement ``` in the date named directory.

Magic layout of placement are shown below.

![img16](https://github.com/user-attachments/assets/ccb780ad-5036-4d20-8a2c-a3e06e2b42fb)
![img17](https://github.com/user-attachments/assets/61945ea9-f8f0-4e09-b908-122c3c6bf008)
![img18](https://github.com/user-attachments/assets/a7132eaa-5f5f-4faa-a121-7cb0ff43c6d4)

config.tcl in runs directory tells which all configurations are taken in the current flow.


## Day-3 Objective: PLUGGING-IN CUSTOM INVERTER CELL IN THE OPENLANE FLOW.

Command to clone the get repo in the openlane directory
``` git clone https://github.com/nickson-jose/vsdstdcelldesign.git ```

This will create a folder called vsdstdcelldesign in the directory
![img20](https://github.com/user-attachments/assets/dc71b5f0-d7ae-43de-8f65-ae777d55c61c)

copying .tech file to the vsdstdcelldesign folder, i.e. adding sky130A.tech to it
![img21](https://github.com/user-attachments/assets/3dbe0385-4b05-4f32-80df-20d74a147ef3)

Opening the inverter layout in Magic layout tool using
``` magic -T sky130A.tech sky130_inv.mag & ```
![img22](https://github.com/user-attachments/assets/5a650fa3-8eb4-4049-addb-ed9998bebfd4)

For extracting the inverter in .spice file, we pass following command one by one in Magic tkcon window:

1- ``` pwd ```

2- ``` extract all ```

3- ``` ext2spice cthresh 0 rthresh 0 ```

4- ``` ext2spice ```
![img23](https://github.com/user-attachments/assets/73b791af-5346-4cbb-96f7-3b1ad59a3b99)



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

Plotting the transient response 
``` plot y vs time a ```

![img24](https://github.com/user-attachments/assets/2bfa0a55-b1c3-49b7-8d74-4bbf017cb808)

 We get the below plot.
 
![img25](https://github.com/user-attachments/assets/335037a3-bb94-4e94-92f8-e1248ba491e8)

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

  #### Creating the .lef file

  We find the tracks info for the standard cell in the following location 
  ``` ..pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd$ less tracks.info ```
  
 ![img26](https://github.com/user-attachments/assets/fa054023-5d0f-4674-89d9-06d5d7e8db8f)

  Pressing ' g ' activates the grid in the Magic layout.
  Activated grid in the layout is shown in below image.

  ![img27](https://github.com/user-attachments/assets/fa7c6132-71f8-44f5-8529-8db45e8ff955)

  Converging grid with the tracks info
  
  -in the tkcon window:
  * ``` help grid ```
  * ``` grid 0.46um 0.34um 0.23um 0.17um ```
  * 
  ![img28](https://github.com/user-attachments/assets/d0462ed1-e898-4c94-9c2a-4bee4d7bbf4b)

Giving a new name to our custom cell and generating .lef file

-in the tkcon window: 
* ``` save sky130A_vsdinv.mag ```
* ``` lef write ```
* 
A new .lef file will be added in vsdstdcelldesign directory
![img29](https://github.com/user-attachments/assets/544bc355-6c9c-48c6-9bd3-46cb24393f3e)

Adding the .lef file to the design src file
![img30](https://github.com/user-attachments/assets/9e056e8a-5881-4515-8987-51a70c04476f)

Adding the 3 .lib files to the design src file
![img31](https://github.com/user-attachments/assets/199ddecb-0c10-4e87-bf92-3771e5df0d5e)

modifying design related config.tcl to the below given content
```
# Design
set ::env(DESIGN_NAME) "picorv32a"

set ::env(VERILOG_FILES) "./designs/picorv32a/src/picorv32a.v"
set ::env(SDC_FILE) "./designs/picorv32a/src/picorv32a.sdc"

set ::env(CLOCK_PERIOD) "12.000"
set ::env(CLOCK_PORT) "clk"


set ::env(CLOCK_NET) $::env(CLOCK_PORT)

set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]

set filename $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/$::env(PDK)_$::env(STD_CELL_LIBRARY)_config.tcl
if { [file exists $filename] == 1} {
        source $filename
}
```

To add the customised cell to the design flow we pass the below highlighted commands one at a time before we run synthesis.
![img32](https://github.com/user-attachments/assets/4605b40d-95b4-4134-9767-74eafbef66a8)
![img33](https://github.com/user-attachments/assets/722e397d-8887-4160-b010-753c236bfaab)

![img34](https://github.com/user-attachments/assets/c43dba1e-aad3-4609-8f4b-6e57e53be05d)



With the completion of synthesis, now we need to perform the floorplan by using the following commands:
```
init_floorplan

place_io

tap_decap_or
```

![img35](https://github.com/user-attachments/assets/303a7486-c693-4b4f-bd68-84d1ceb7779f)

![img36](https://github.com/user-attachments/assets/cf31cd4f-9464-47a2-943e-89b5626db078)


Then, ``` run_placement ```

![img37](https://github.com/user-attachments/assets/71256571-943d-4b70-b906-341a082b5c8e)

Placement layout
![img38](https://github.com/user-attachments/assets/832912ab-5992-4114-a15e-94c3e9bb3355)
![img39](https://github.com/user-attachments/assets/3cd88f4e-c035-49c1-9c44-419c944562c7)

Expanded view
![img40](https://github.com/user-attachments/assets/f8f9f670-583c-4f58-a75c-b3f946acbc19)

### Lab Introduction to Magic tool options and DRC rules

Downloading the lab files using the below given command in the home directory
``` wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz ```

Unzipping the downloaded zip file using the below command 
``` tar xfz drc_tests.tgz ```
![img82](https://github.com/user-attachments/assets/78f3be97-3f02-48ea-8727-022a58c26a5f)

.magicrc file below
![img83](https://github.com/user-attachments/assets/2d3a968a-9879-436f-b69a-8b8b572e6dda)

Opening Magic tool
``` magic -d XR & ``` this will open Magic with an empty window shown below
![img84](https://github.com/user-attachments/assets/2a3e23e8-0e50-4159-a944-159dc62a9679)
with this, go to File option in the menu and select Open. 
![img85](https://github.com/user-attachments/assets/ff5c44ce-f50f-4443-8f69-123963ad6ec3)
from here, select met3.mag and open
 *OR*
 in the terminal pass ``` magic -d XR met3 & ```
![img86](https://github.com/user-attachments/assets/f78fa945-8b8b-458a-9685-cc88de904254)

 select the area of any of the elements and using ``` drc why ``` in the tkcon window will tell which drc rule got violated for that design
![img87](https://github.com/user-attachments/assets/6027b35b-1c99-4a65-a094-f1ce675f743f)

For metal3 all rules are related to drawn layers, except for rule m3.4
 Now, select an area in the gui and guide the pointer on to the metal 3 layer and press P. The selected region will be filled with metal 3. 
 Now in console window type the command ``` cif see VIA2 ``` , The metal 3 filled area will be filled VIA2 mask.
 ![img88](https://github.com/user-attachments/assets/be59f4e0-952b-4d94-92d2-ba39d36e2960)

 
##### Incorrectly implemented poly.9 simple rule correction
The correct rule is shown below in the screenshot of https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#m3
![img1'](https://github.com/user-attachments/assets/94d81ccd-66e0-4a3d-ad50-996356f0a0cd)

Load the poly.mag file by passing ``` load poly.mag ``` in the console window
![img89](https://github.com/user-attachments/assets/b7754fde-4291-455f-8fa5-3ae82cb594f9)

Select the area between poly and poly resistor and pass ``` drc check ``` command
![img90](https://github.com/user-attachments/assets/f6be65f0-76b1-4b82-a8cb-4deb09650058)


Now, to open the sky130A.tech file from the drc_tests director.
Navigate to the keywords ' poly.9 ' in the file and make copy of the required lines and change *nsd and alldiff to allpolynonres.
Save the changes
![IMG91](https://github.com/user-attachments/assets/c4461c33-a6c6-4627-9e8d-c81ec1c5c016)

![IMG92](https://github.com/user-attachments/assets/af6597bd-51bb-4105-b724-24ad71ac2b2a)


Now, to check the correction, use ``` tech load sky130A.tech ``` in the tkcon window. And then ``` drc_check ``` ``` drc_why ```
![IMG93](https://github.com/user-attachments/assets/69a1ca46-6073-4bf2-bf98-8a97e0ad62a7)




##### Incorrectly implemented nwell.4 complex rule correction
Load nwell.mag using ``` load nwell.mag ``` in the console window. 
![IMG94](https://github.com/user-attachments/assets/f8608830-6e48-4ab1-94fc-9ba40ca53afd)

![IMG95](https://github.com/user-attachments/assets/40ac393d-7f66-4c17-b1d8-ca371a9ccd92)

To make below changes to the sky130A.tech file 
![IMG96](https://github.com/user-attachments/assets/6f731753-88df-4af3-b84a-36af50533853)

##### Incorrectly implemented difftap.2 simple rule correction
Opening difftap layout using ``` magic -d XR difftap & ``` in the terminal

![IMG97](https://github.com/user-attachments/assets/7d1a7960-b4f8-4aaa-aec8-57eb5057a283)

Checking drc
![IMG98](https://github.com/user-attachments/assets/a5233f6f-ed36-464f-8225-b0b2dc6fe652)

making change in below space..
![IMG99](https://github.com/user-attachments/assets/6b2f4adc-c875-4fee-abce-3694ac2396cc)

..to the below
![IMG100](https://github.com/user-attachments/assets/d7a81264-3f40-483b-837f-42f8b5052998)


![IMG101](https://github.com/user-attachments/assets/e6f4bfdd-5110-4a39-911c-24ebd27c0760)

##  Day-4 Objective: OPTIMISING SYNTHESIS FOR SLACK VIOLATION AND PERFORMING CTS.

created a file called ``` pre_sta.conf ``` in openlane directory using ``` vim ``` command.
![img41](https://github.com/user-attachments/assets/95cdc499-fbe9-48b4-9bc1-402400a33968)


Then created a file called ``` my_base.sdc ``` in ..design/picorv32/src using ``` vim ``` command.
![img42](https://github.com/user-attachments/assets/b2d384f6-0bfd-4f9b-bfad-4406f0294e39)

then 
``` .../openlane$ sta pre_sta.conf ```
![img43](https://github.com/user-attachments/assets/b29fe6c7-2bc4-4962-b74c-3fc82ded683d)
the slack violation value is same as what it was in synthesis step(img33)

Optimisng the design for slack reduction

Noticed that the net instance _10566_ has a fanout of 4 and is driven by or gate of size 3_4
![img44](https://github.com/user-attachments/assets/2ca63f63-ecd9-4b4e-ab64-4a762a14b9bb)

so replacinf this instance with _13165_ or gate of size 3_4
![img45](https://github.com/user-attachments/assets/8c4d91d4-7a7b-4269-8cf0-a5e903b30336)

with this we observe reduction in slack from -23.89 to -23.5184
![img46](https://github.com/user-attachments/assets/914334a5-9a7e-4355-980d-64e99c8bdab0)


Noticed that the or gate of driving strenth 2 driving OA gate has more delay
![img47](https://github.com/user-attachments/assets/602424c3-c7c3-46e9-9383-cf62339053a1)

so replacing this instance with _13132_ or gate of size 4_4
![img48](https://github.com/user-attachments/assets/3cbdabb6-6fc8-46e7-8669-26f4b904182f)

with this we observe reduction in slack from -23.5184 to -23.5119
![img49](https://github.com/user-attachments/assets/b8be89f9-7d8f-4777-8db5-940bc0145984)

So,now earlier the slack was -23.89 now it is -23.5119. That is, the slack has been reduced by 0.3781
// -- command to verify that the instance _13132_ has been replaced from or4_2 to or4_4 --
// ``` report_checks -from _26365_ -to _27762_ -through _13132 ```

Replacing the old netlist with the new one post the fixes done above

```
//making a copy of old netlist
.../results/synthesis$ cp picorv32a.synthesis.v picorv32a.synthesis_old.v
```
![img50](https://github.com/user-attachments/assets/920e1970-11b3-4256-8ef2-5cd57cb21c28)

NOW, overwriting the original netlist with the modified by passing the below command in OpenSTA flow itself 
```
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/25-03_18-52/results/synthesis/picorv32a.synthesis.v
```
Updation of netlist can be verified by the new time indicated prior to verilog file 
![img51](https://github.com/user-attachments/assets/7563d5dc-9942-40c4-8e5d-3c4fc26a28ab)

Verification that the instance _13132_ is replaced to sky130_fd_sc_hd__or4_4
![img52](https://github.com/user-attachments/assets/acba9888-c40d-4923-97f6-3d7a97766879)

NOW RUNNING FLOORPLAN ON THE UPDATED NETLIST IN THE **OPENLANE** FLOW
![img53](https://github.com/user-attachments/assets/79306a65-eb51-4743-8f0e-928d1a28a942)
RUNNING PLACEMENT 
![img54](https://github.com/user-attachments/assets/4c04eadd-c19c-43a9-8a10-0c90a8c776fb)

RUNNING CTS
``` run_cts ```
a new addition to ..results/synthesis directory 
![img55](https://github.com/user-attachments/assets/6d5ee517-3e6d-453e-b6a0-61346a1a2ae2)

clock buffer can be viewed in CTS layout in Magic
![img56](https://github.com/user-attachments/assets/3b12c52d-875f-45da-9cb0-8d18222418a0)


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
![img57](https://github.com/user-attachments/assets/c1288d55-6c38-480b-ba68-76664c8928aa)

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
![img58](https://github.com/user-attachments/assets/5271ea09-66ee-4c42-bc3c-5315ae5217fd)

![img59](https://github.com/user-attachments/assets/cce39fa6-e894-448c-90e5-12793be92119)

hold slack met
![img60](https://github.com/user-attachments/assets/6ea29742-67b5-401f-98d0-ae734eaafc69)

setup slack met 
![img61](https://github.com/user-attachments/assets/f17054f0-24f7-43c3-8eac-2d9ed76f0d38)

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
![img62](https://github.com/user-attachments/assets/af7e7930-9f8c-41ac-a8cf-d2860c51ead5)


effect on setup slack
![img63](https://github.com/user-attachments/assets/9a7c4b74-193b-49f3-8d7b-e0fa35b2f6f7)3

![img64](https://github.com/user-attachments/assets/c3fd2e53-854a-4a7e-84b1-d8c8867f989f)


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
![img65](https://github.com/user-attachments/assets/925db033-6c5e-4a45-b6cf-61872eae0567)
![img66](https://github.com/user-attachments/assets/94ea7ddb-3f3e-43dd-916c-c68ee2fffb65)

Magic Layout for generated PDN
![img67](https://github.com/user-attachments/assets/97de4c17-1f54-48d9-93d6-d15f3a7ffec5)
![img68](https://github.com/user-attachments/assets/7e253cfa-f9bc-4439-802f-4bdde68f09bc)

``` run_routing ```
![img69](https://github.com/user-attachments/assets/5db899df-791b-4a4d-91ba-05e05870901b)
![img70](https://github.com/user-attachments/assets/079ed469-c3c6-4eb9-887e-a16bb1974ffd)
![img71](https://github.com/user-attachments/assets/6d0c863a-607f-4487-8182-dfeeeb6be942)


Route layout in Magic
![img72](https://github.com/user-attachments/assets/59128348-6096-452a-9189-a026f3140be9)
![img73](https://github.com/user-attachments/assets/7956daef-d15c-43d0-9797-76382ed4c54b)
![img74](https://github.com/user-attachments/assets/6e2dc285-45e9-45a1-8271-32a12066f684)
![img75](https://github.com/user-attachments/assets/1a8ab965-2dbb-4eeb-8c87-7d6d7ba5627a)

Screenshot of fast route guide present in openlane/designs/picorv32a/runs/26-03_08-45/tmp/routing directory
![img76](https://github.com/user-attachments/assets/b0a3449b-e3e1-4180-b102-fbf20272e417)


####### Post-Route parasitic extraction using SPEF extractor.
![img77](https://github.com/user-attachments/assets/e85b21f4-8d39-4ca7-86b2-3aaffd3add60)

timing analysis
![img78](https://github.com/user-attachments/assets/8f0ecd32-e843-4e82-976d-d78f1045d7bb)
![img79](https://github.com/user-attachments/assets/023fde86-6c50-4869-8907-86d12d6442c5)

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
![img80](https://github.com/user-attachments/assets/fd9202ff-db10-4e97-8f82-376013ab5300)

setup slack
![img81](https://github.com/user-attachments/assets/da0cbd12-b8f0-40e3-9685-52eb4d34daa9)
