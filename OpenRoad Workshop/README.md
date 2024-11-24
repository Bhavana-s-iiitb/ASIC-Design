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
