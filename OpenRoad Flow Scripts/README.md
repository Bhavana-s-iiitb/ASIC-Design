# OpenRoad Flow Scripts

OpenROAD Flow is a full RTL-to-GDS flow built entirely on open-source tools. 
OpenROAD Flow Scripts we have the following public platforms:

- sky130hd
- sky130hs
- nangate45
- asap7
### RTL to GDS Flow:

 RTL to GDS flow is a methodology for designing an integrated circuit (IC) in the semiconductor industry. 

<img width="479" alt="image" src= https://github.com/user-attachments/assets/f2715266-6607-44ac-9fc7-8d8ba0c4b7f6>

<br>
1. Logic Synthesis:  In logic synthesis, a high-level description of the design (RTL Code) is converted into an optimized gate-level representation of a given standard cell library and certain design constraints.<br>
2. Place and Route (PnR): Gate level netlist after DFT Insertion and SDC file is taken as input for the PnR and based on standard cells library, PnR starts. The goal of PnR stage is to place all the standard cells, Macros and I/O pads with minimal area, with minimal delay and Route them together in such a way that there is no DRC (Design Rule Check) error. The final output of this stage is the layout of design in the form of GDSII file which is defacto standard of layout file in the industry. <br>

# OpenRoad Tool Installations
```
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
sudo ./setup.sh
```
<img width="479" alt="image" src=https://github.com/user-attachments/assets/b9447b95-e852-4b36-b058-66f6747e1d95>

To build
` ./build_openroad.sh --local `

### Verification of Installation

```
source ./env.sh
yosys -help
openroad -help
cd flow
make
```

<img width="479" alt="image" src=https://github.com/user-attachments/assets/cef5f364-7fcc-4e19-a3b6-fb9319713fd5>

<img width="479" alt="image" src=https://github.com/user-attachments/assets/08776748-eacb-4a09-8a54-7bcf7f56cec7>

# OpenRoad Flow  with vsdbabysoc file
## 1. Synthesis of vsdbabysoc
```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk synth

```
<img width="700" alt="image" src=https://github.com/user-attachments/assets/f702ee9f-fb34-4c62-9e14-4e514e43e373>
## Log Files

<img width="700" alt="image" src=https://github.com/user-attachments/assets/3ac0dee4-5c08-4bb5-a293-fb0d2535f2b5>

<img width="700" alt="image" src=https://github.com/user-attachments/assets/64a953b5-3d1f-4305-90c5-80a5b9d41555>

<img width="700" alt="image" src=https://github.com/user-attachments/assets/4783a64d-d04d-489b-9a8e-cfe0d381bd02>


## 2. Floorplan

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk floorplan

```

<img width="479" alt="image" src=https://github.com/user-attachments/assets/8d15785f-76f0-4011-8aa0-25431c91eadc>

<img width="479" alt="image" src=https://github.com/user-attachments/assets/e5fcfa1c-777d-4ea4-8de2-87a5347a54b2>


```
make gui_floorplan
```
DAC
<br>

<img width="600" alt="image" src=https://github.com/user-attachments/assets/2409697a-e6f2-4ac1-b5e9-3a0c929ad205>

PLL
<br>


<img width="600" alt="image" src=https://github.com/user-attachments/assets/a1e798d5-a2c3-4e73-86a1-2318ad9effbd>

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_floorplan
```

<img width="600" alt="image" src=https://github.com/user-attachments/assets/5d6d0555-d27f-493c-b34b-28b1d720312f>

<img width="600" alt="image" src=https://github.com/user-attachments/assets/f7fb39a4-b7f3-47ed-a2f4-8b76afba8a1f>

### floorplan Final report
<img width="600" alt="image" src=https://github.com/user-attachments/assets/6ff40c10-7b1c-4c66-a556-ebacb5fa6744>


## 3. Placement
```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk place

```

<img width="600" alt="image" src=https://github.com/user-attachments/assets/01d6719e-2294-4efe-b5db-f809879613ec>


<img width="600" alt="image" src=https://github.com/user-attachments/assets/eb729b62-a8a7-4adf-acd3-71da7493ab88>
<img width="600" alt="image" src=https://github.com/user-attachments/assets/8043c9a8-a0db-4ebd-b4dd-c67d44ce5104>


### gui_place:
```
make gui_place
```

<img width="600" alt="image" src=https://github.com/user-attachments/assets/6e67529b-ae30-4ccf-9851-9fd6aa9368cd>
<img width="600" alt="image" src=https://github.com/user-attachments/assets/21aaaacc-c49f-4e5c-bd1a-466f09789d91>

### Log Files

<img width="600" alt="image" src=https://github.com/user-attachments/assets/ecfc6f50-9604-4c6e-ad48-c64b0c771718>
### Placement Report
<img width="600" alt="image" src=https://github.com/user-attachments/assets/9c2702cb-c0e1-47b5-987d-4249579a88b6>

## 4. CTS
```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk cts

```
<img width="700" alt="image" src=https://github.com/user-attachments/assets/f8cb76f7-cb04-4243-990e-63ff7160a394>
### gui_cts
<img width="700" alt="image" srchttps://github.com/user-attachments/assets/f4651c51-7f72-4218-b57c-b528a2659239>

### cts_sdc
<img width="700" alt="image" srchttps://github.com/user-attachments/assets/40b28ef3-492d-478d-82c5-58e57ddff834>
### cts final report
<img width="700" alt="image" srchttps://github.com/user-attachments/assets/b4569206-23a1-46f9-ac34-7ff02864ff48>


## 5. Routing
![image](https://github.com/user-attachments/assets/5c528a9f-3ed8-47b3-9fd8-696ba5c0d27d)
![image](https://github.com/user-attachments/assets/e7528008-ebb7-426f-bae1-91d60190aab3)


