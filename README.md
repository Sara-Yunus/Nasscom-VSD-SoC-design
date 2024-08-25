# Nasscom Certification Program in Digital VLSI SoC design and planning 
### (21st Aug'24 - 3rd Sept'24)

## Day 1:  Inception of open-source EDA, OpenLANE and Sky130 PDK
### Section 1 - How to talk to computers
#### Lecture 1 - Introduction to QFN-48 Package, chip, pads, core, die and IPs
#### Lecture 2 - Introduction to RISC-V 
#### Lecture 3 - From Software Applications to Hardware

### Section 2 - SoC design and OpenLANE
#### Lecture 1 - Introduction to all components of open-source digital ASIC design
#### Lecture 2 - Simplified RTL2GDS flow
#### Lecture 3 - Introduction to OpenLANE and Strive chipsets
#### Lecture 4 - Introduction to OpenLANE detailed ASIC design flow

### Section 3 - Get familiar with open-source EDA tools
#### Lecture 1 - OpenLANE Directory structure in detail
Invoking the OpenLane tool.
```bash
# Change the directory to a required location using the following command in the terminal.
cd Desktop/work/tools/openlane_working_dir/openlane
```

#### Lecture 2 - Design Preparation Step
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
`image1'

Importing all the packgaes to work with
```
# to get ready to execute the command
% package require openlane 0.9
```
Preparing Design files
```
# for working on the design of Picorv32 Processor
% prep -design picorv32
```
A new file named "runs" is created in picorv32a directory. 

`image2'
#### Lecture 3 - Review files after design prep and run synthesis
This 'runs' file contains a directory of named with date of its creation. Here 25-08_06-10. As of now, everything except the 'tmp' folder will be empty in this. Merged.lef is stored here.

`The config.tcl file present in the folder 25-08_06-10 shows/contains all the default parameters taken by the run. Using this we can check whether the modifications done is correctly reflected in the execution of the design.` 

`The cmds.log takes the record of all the commands used. This gets populated as additional steps are done.`

Running the synthesis step

``` run_synthesis```

`image3'
#### Lecture 4 - OpenLANE Project Git Link Description
A github repository is suggested to go through which I cannot find on the Github.

A YouTube video is also suggested, which is 
https://www.youtube.com/live/Vhyv0eq_mLU?si=vghy-LKS7MWhH_xY
#### Lecture 5 - Steps to Characterize Synthesis Results

1st objective of this workshop is to find the flip-flop ratio. 

`image4'

```math
Flip  flop  Ratio = {No. of D  flip  flops}/{Total  no.  of  cells} = {1613}/{14876} = 0.108429
```
```math
Percentage of D flip-flops = 0.108429 * 100 = 10.8429%
```
The synthesised netlist is now in the 'results' file of the 'runs' folder.
`image5'

The last report to be generated in Yosys gives you the actual synthesis statistic report.
`image6'

` we can exit the screen after using less command with 'q' key.`



#### Lecture 2 - Design Preparation Step
#### Lecture 3 - Review files after design prep and run synthesis
#### Lecture 4 - OpenLANE Project Git Link Description
#### Lecture 5 - Steps to Characterize Synthesis Results

## Day 2: Good floorplan vs bad floorplan and introduction to library cells
### Section 1 - Chip Floor planning considerations
#### Lecture 1 - Utilization factor and aspect ratio
#### Lecture 2 - Concept of pre-placed cells
#### Lecture 3 - De-coupling capacitors
#### Lecture 4 - Power planning
#### Lecture 5 - Pin placement and logical cell placement blockage
#### Lecture 6 - Steps to run floorplan using OpenLANE
#### Lecture 7 - Review floorplan files and steps to view floorplan
#### Lecture 8 - Review floorplan layout in Magic

### Section 2 - Library Binding and Placement
#### Lecture 1 - Netlist binding and initial place design
#### Lecture 2 - Optimize placement using estimated wire length and capacitance
#### Lecture 3 - Final placement optimization
#### Lecture 4 - Need for libraries and characterization
#### Lecture 5 - Congestion-aware placement using RePlAce

### Section 3 - Cell design and characterization flows
#### Lecture 1 - Inputs for cell design flow
#### Lecture 2 - Circuit design step
#### Lecture 3 - Layout design step
#### Lecture 4 - Typical characterization flow

### Section 4 - General timing characterization parameters
#### Lecture 1 - Timing threshold definitions
#### Lecture 2 - Propagation delay and transition time

