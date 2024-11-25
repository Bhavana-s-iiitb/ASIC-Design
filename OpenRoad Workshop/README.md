# Tool Installations
```
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
sudo ./setup.sh
```
![image](https://github.com/user-attachments/assets/b9447b95-e852-4b36-b058-66f6747e1d95)

To build
` ./build_openroad.sh --local `

## Verification of Installation

```
source ./env.sh
yosys -help
openroad -help
cd flow
make
```

![image](https://github.com/user-attachments/assets/cef5f364-7fcc-4e19-a3b6-fb9319713fd5)

![image](https://github.com/user-attachments/assets/08776748-eacb-4a09-8a54-7bcf7f56cec7)


# Synthesis of vsdbabysoc
```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk synth

```
![image](https://github.com/user-attachments/assets/f702ee9f-fb34-4c62-9e14-4e514e43e373)


# Floorplan

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk floorplan

```
![image](https://github.com/user-attachments/assets/9a0314a2-9c3a-4dc5-af3c-e4669f2c8d6c)

![image](https://github.com/user-attachments/assets/0d40d06f-b058-43d2-b8ca-1cd5b4e0f3ef)

```
make gui_floorplan
```
![image](https://github.com/user-attachments/assets/7ee5ff76-576a-4287-97d6-d54e12a3a2c0)


```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_floorplan
```

![image](https://github.com/user-attachments/assets/5d6d0555-d27f-493c-b34b-28b1d720312f)

![image](https://github.com/user-attachments/assets/f7fb39a4-b7f3-47ed-a2f4-8b76afba8a1f)

# Placement

![image](https://github.com/user-attachments/assets/a894733b-d3ed-4aa6-b3af-1583a55e54ed)

![image](https://github.com/user-attachments/assets/989d040c-2f1e-4733-bd6a-013129720d97)



