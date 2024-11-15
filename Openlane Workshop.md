#### 1. Run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.

Commands to invoke the OpenLANE flow and perform synthesis

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```
<br>

![image](https://github.com/user-attachments/assets/3c3c0091-6bc5-485a-b71d-e945619f5840)

<img width="479" alt="image" src="https://github.com/user-attachments/assets/4fb4fea4-9970-48a7-aaa6-9511455a7cea">

<br>
Calculation of Flop Ratio and DFF % from synthesis statistics report file

```math
Flop\ Ratio = \frac{1613}{14876} = 0.108429685
```
```math
Percentage\ of\ DFF's = 0.108429685 * 100 = 10.84296854\ \%
```
<img width="479" alt="image" src="https://github.com/user-attachments/assets/fd43373a-5667-4f8f-8646-db18ae69fb50">

<img width="479" alt="image" src="https://github.com/user-attachments/assets/fe797c9e-0b3c-4bd8-9f72-f436e73342ee">

## Section 2 - Good floorplan vs bad floorplan and introduction to library cells 

### Theory

### Implementation

Section 2 tasks:- 
1. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.
2. Calculate the die area in microns from the values in floorplan def.
3. Load generated floorplan def in magic tool and explore the floorplan.
4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.
5. Load generated placement def in magic tool and explore the placement.

```math
Area\ of\ die\ in\ microns = Die\ width\ in\ microns * Die\ height\ in\ microns
```

#### 2. Calculate the die area in microns from the values in floorplan def.

Screenshot of contents of floorplan def

According to floorplan def
```math
1000\ Unit\ Distance = 1\ Micron
```
```math
Die\ width\ in\ unit\ distance = 660685 - 0 = 660685
```
```math
Die\ height\ in\ unit\ distance = 671405 - 0 = 671405
```
```math
Distance\ in\ microns = \frac{Value\ in\ Unit\ Distance}{1000}
```
```math
Die\ width\ in\ microns = \frac{660685}{1000} = 660.685\ Microns
```
```math
Die\ height\ in\ microns = \frac{671405}{1000} = 671.405\ Microns
```
```math
Area\ of\ die\ in\ microns = 660.685 * 671.405 = 443587.212425\ Square\ Microns
```
<img width="479" alt="image" src="https://github.com/user-attachments/assets/331ae73a-02e4-432c-8fc0-e1153dd74f97">

<img width="479" alt="image" src="https://github.com/user-attachments/assets/020b2149-b800-4aa8-9337-2b5f5f412106">


<br>

#### 3. Load generated floorplan def in magic tool and explore the floorplan.

Commands to load floorplan def in magic in another terminal

```bash
# Change directory to path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
<img width="479" alt="image" src="https://github.com/user-attachments/assets/e011cd0f-695f-4908-9cbf-0287a9c5c697">


<img width="479" alt="image" src="https://github.com/user-attachments/assets/bf2b4e36-a86c-4f0f-98d4-34178d74bab2">

<img width="479" alt="image" src="https://github.com/user-attachments/assets/a9620b3d-19db-4ac0-8126-f250b35c658e">




<img width="479" alt="image" src="https://github.com/user-attachments/assets/89962473-76b7-4f02-8896-4e414cf626c5">



#### 4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.

Command to run placement

```tcl
# Congestion aware placement by default
run_placement
```

Screenshots of placement run

<img width="479" alt="image" src="https://github.com/user-attachments/assets/c6e698c3-1e69-407c-a3f5-5227b9c16356">


<img width="479" alt="image" src="https://github.com/user-attachments/assets/9dbe0a2d-0c08-46db-94f9-a3b211e2acde">


<img width="479" alt="image" src="https://github.com/user-attachments/assets/1c21cd5e-41a6-4be3-9d49-3e5cbd183e69">

#### 5. Load generated placement def in magic tool and explore the placement.

Commands to load placement def in magic in another terminal

```bash
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

Screenshots of floorplan def in magic




<img width="479" alt="image" src="https://github.com/user-attachments/assets/ff851ebb-e756-4f6a-9c2e-3591d2a98a15">

<img width="479" alt="image" src="https://github.com/user-attachments/assets/e03f5de5-1b79-4b85-8e57-f408a2b05871">

<img width="479" alt="image" src="https://github.com/user-attachments/assets/5fa66d6b-4ba9-42c2-a8c4-220681a5ddf3">


<img width="479" alt="image" src="https://github.com/user-attachments/assets/a57f1b0f-44c3-4438-9361-1b9de984b467">

<img width="479" alt="image" src="https://github.com/user-attachments/assets/5515e9bd-266d-45b2-a821-c9b2f9896742">

<img width="479" alt="image" src="https://github.com/user-attachments/assets/a3bc3555-6d9d-46e8-b885-0c4b3bc5886b">

<img width="479" alt="image" src="https://github.com/user-attachments/assets/08743da9-0e97-4838-bfdc-bac4ce1b392d">

<img width="479" alt="image" src="https://github.com/user-attachments/assets/b5f77485-0379-4e4f-91a7-28d50af940d0">

<img width="479" alt="image" src="https://github.com/user-attachments/assets/649c42dd-a32d-4ef3-b4a2-e639e1554f53"

<img width="479" alt="image" src="https://github.com/user-attachments/assets/22e70ed3-28cd-4f0e-acc3-feede10a3b27">

<img width="479" alt="image" src="https://github.com/user-attachments/assets/a550d33c-eb73-409e-b00f-490f216b0e65">

<img width="479" alt="image" src="https://github.com/user-attachments/assets/1c1b63e1-d7ad-4d89-911a-aa239a076946">

<img width="479" alt="image" src="https://github.com/user-attachments/assets/512ad54d-95a7-4e59-b69d-2a255178cece">

### Incorrect Poly9:
<br>


<img width="479" alt="image" src="https://github.com/user-attachments/assets/7750f9eb-6897-40e5-b1f5-2cf88e20c36f">

Correction of DRC
<img width="479" alt="image" src="https://github.com/user-attachments/assets/07a877f4-c724-4c8b-bf8d-4c151989dce8">


<img width="479" alt="image" src="https://github.com/user-attachments/assets/e4f4530a-d857-4a92-9a9a-f7e08b008cc9">

<img width="479" alt="image" src="https://github.com/user-attachments/assets/fd29dd6e-9de5-4c84-9874-4cceab4404bf">

# Day 4

![image](https://github.com/user-attachments/assets/546092b8-6236-4dcd-ba7a-d10a059d733e)

![image](https://github.com/user-attachments/assets/69cbd5bd-84cb-4961-a177-9b452d4cbe0f)


![image](https://github.com/user-attachments/assets/e09f54f2-61f1-4af3-9a11-78af7b227bbf)

![image](https://github.com/user-attachments/assets/84222307-4fc8-4898-9afa-80aede522bfa)

![image](https://github.com/user-attachments/assets/a610be37-73d0-45fc-8d44-0b5d2fdb7ffc)

Saving Inverter with custom name:

![image](https://github.com/user-attachments/assets/bea835ed-7943-44d5-8b3f-f274967d01f5)

Writing lef File:

![image](https://github.com/user-attachments/assets/1e2dc9df-6bda-4651-ab49-0dbef5ff0546)

4. Copy newly generated lef file

5. ![image](https://github.com/user-attachments/assets/0b583c79-bc1c-405a-9ec8-2e7455152602)

6. Edit the config.tcl

   ![image](https://github.com/user-attachments/assets/77bbdcf8-9941-45ea-b9e1-68ce5c718860)

7. ![image](https://github.com/user-attachments/assets/fa6fcfce-88cf-4fd4-9b3b-1f836243e0a8)
   ![image](https://github.com/user-attachments/assets/452d3008-6e20-449a-9afb-77d4c789341f)

   Noting down the chip area and slack value:
   ![image](https://github.com/user-attachments/assets/bb6d886b-834f-4347-8e14-b303f58e543d)
   ![image](https://github.com/user-attachments/assets/a1c0784e-18d3-4ee5-9ec6-b4d73744f991)


Changing Values
![image](https://github.com/user-attachments/assets/80033c86-99ed-42f1-9c98-01eddf7003e9)

Comparing the previous values:
![image](https://github.com/user-attachments/assets/44e4caac-1b4b-403e-9f4f-07ac6e3eeb63)

![image](https://github.com/user-attachments/assets/ab664f59-6ef5-423e-bc80-ea6b27e80cf1)
Screenshot of merged.lef in tmp:
![image](https://github.com/user-attachments/assets/7bdb5fa3-dfbc-480b-b0b1-736132e00b7d)

Facing unexpected unexplainable error while running floorplan
![image](https://github.com/user-attachments/assets/afc93d64-11b2-4ff3-812d-ef5ed65e8b75)

Running new set of commands:
![image](https://github.com/user-attachments/assets/ad66d05d-97e7-4555-819d-224a13c18eb3)

![image](https://github.com/user-attachments/assets/2b828493-7eba-40c7-8133-769e24943a9c)

![image](https://github.com/user-attachments/assets/3138dbaf-2819-4649-b985-5143a211b340)


Screenshot of placement.def in magic
![image](https://github.com/user-attachments/assets/954a466d-b60a-4a20-b90e-dc869b551fdb)

Finding the custom inverter

![image](https://github.com/user-attachments/assets/48ebc4ea-d996-4e3a-92f8-bfcbbaf1a534)

![image](https://github.com/user-attachments/assets/db61c72e-f4f9-45ae-8a78-0314ac832840)

Timing analysis using Open STA:

![image](https://github.com/user-attachments/assets/975be719-186c-4eaa-b05f-c9f1bbf2e886)


![image](https://github.com/user-attachments/assets/b9da5207-5862-40ea-b74c-efe79705cd18)


![image](https://github.com/user-attachments/assets/5787a554-b9bc-4287-98aa-fdf09c3a41a8)

Newly created sta.conf:

![image](https://github.com/user-attachments/assets/e8543877-127c-438c-aa7a-b0dd00e9850b)



![image](https://github.com/user-attachments/assets/f1e8fc93-84af-42ba-9d05-e6b23445abfc)

Newly created pre_sta.conf file and my_base.sdc file:

![image](https://github.com/user-attachments/assets/72401444-d355-40c4-a895-82dd07767143)


![image](https://github.com/user-attachments/assets/d313fd0e-14af-4f07-9b8e-1866f34dce6f)

After running pre_sta.conf

![image](https://github.com/user-attachments/assets/a78c5709-784d-43df-9d14-70512c9cce83)

Commands to include new lef and perform synthesis
<br>
Screenshot after final run_synthesis:

![image](https://github.com/user-attachments/assets/eabb488b-094f-47f0-aea6-4c44cfb94d5f)

Running sta in new window:

![image](https://github.com/user-attachments/assets/5be8282e-fd69-4000-9b02-74b978dca004)


![image](https://github.com/user-attachments/assets/647ea7f0-3fb9-4150-a67d-5a993f4a23a2)

<br>
10. Make timing ECO fixes to remove all violations.
<br>
OR gate of drive strength 2 is driving 4 fanouts
<br>

![image](https://github.com/user-attachments/assets/714dfe64-4cc0-45c9-bfef-071b4f3adab3)


![image](https://github.com/user-attachments/assets/51223511-c5af-4c7c-ad8d-bd8c98b22ab3)

<br>
OR gate of drive strength 2 is driving 4 fanouts
<br>


![image](https://github.com/user-attachments/assets/5e23973f-0067-448d-97e1-cca73318f90e)

