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
![image1](https://github.com/user-attachments/assets/5f79b866-36e8-4bb3-87ea-58cc3b4acbc1)


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
This 'runs' file contains a directory of named with date of its creation. Here 25-08_06-10. 
![image2](https://github.com/user-attachments/assets/6dac452f-9995-4050-a50a-cf4283169222)

As of now, everything except the 'tmp' folder will be empty in this. Merged.lef is stored here.

Running the synthesis step

``` run_synthesis```

![image3](https://github.com/user-attachments/assets/4e50441f-8981-41bd-bae5-2c4cdeeeec01)

![image5](https://github.com/user-attachments/assets/cac85962-33d8-45f6-ab6c-de77dfc29457)

![image6](https://github.com/user-attachments/assets/8ed67ee8-d773-40e1-9053-f4d6bdd95d30)


1st objective of this workshop is to find the flip-flop ratio. 

![image4](https://github.com/user-attachments/assets/1acbbc44-a108-4e1e-a955-8c04a26cd611)

```math
Flip  flop  Ratio = {No. of D  flip  flops}/{Total  no.  of  cells} = {1613}/{14876} = 0.108429
```
```math
Percentage of D flip-flops = 0.108429 * 100 = 10.8429%
```
