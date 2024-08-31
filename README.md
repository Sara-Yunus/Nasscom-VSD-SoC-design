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
