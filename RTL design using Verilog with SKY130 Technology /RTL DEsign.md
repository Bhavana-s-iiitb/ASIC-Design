![image](https://github.com/user-attachments/assets/3a21c561-748b-47cf-b49f-2cf1dd52af53)# Installation of Required tools
Use the following commands to install yosys, iverilog and gtkwave

```
   $ git clone https://github.com/YosysHQ/yosys.git
    $ cd yosys
    $ sudo apt install make (If make is not installed) 
    $ sudo apt-get install build-essential clang bison flex \
        libreadline-dev gawk tcl-dev libffi-dev git \
        graphviz xdot pkg-config python3 libboost-system-dev \
        libboost-python-dev libboost-filesystem-dev zlib1g-dev
    $ make config-gcc
    $ make 
    $ sudo make install
```

![Screenshot from 2024-10-21 19-29-07](https://github.com/user-attachments/assets/8360cb64-327b-4c0d-a6fe-efb4f022a1f7)
 ### To install iverilog:
 ` sudo apt-get install iverilog `

 ### To install gtkwave:
    ``` 
    sudo apt update
    sudo apt install gtkwave 
    ```

![Screenshot from 2024-10-21 19-31-10](https://github.com/user-attachments/assets/9e206e18-d0b1-46eb-bcae-824f434e2e46)

# Day 1
### Simulator:
-Simulator is tool to verify the RTL design by using testbech. iverilog is one of the simulation tool.

-Design is the actual verilog code which has the intended functionality to meet with the required specifications.
-Testbench is the setup to apply stimulus.
iverilog based simualtion flow:
The iverilog simualator takes the design file and the testbench as input and generates a VCD(Value chae dump format) file. The simulaton output can be viewed using gtkwave analyser.
![image](https://github.com/user-attachments/assets/8fc95a48-dcf5-4ed8-8ce0-870e9b37f659)
## VSD Flow

```
mkdie VLSI
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop
```

To load a design file:
```
cd VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files
iverilog good_mux.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd
```
![image](https://github.com/user-attachments/assets/4444f832-84f0-493c-bf5c-3b1334f5656e)
To see the file structure :
` gvim tb_good_mux.v -o good_mux.v `
![image](https://github.com/user-attachments/assets/a7e20d02-42cc-441d-9c7a-742fa6bcc0a6)

## Yosys
Synthesizer is the tool for converting RTL design to netlist and YOSYS is the tool we are using as for synthesis.
Netlist is the representation of the design in the form of cells present in the design.

![image](https://github.com/user-attachments/assets/900a9cb6-6b9d-4653-aa40-4184c3f67907)

To verify the synthesis output: 
Give the netlist as input the iverilog simulator along with the same testbench and view the output VCD file using gtkwave analyser. This output should be same as the output obtained from RTL design file.
The set of primary inputs and outputs remain same for both RTL design and netlist simulation. Hence the same testbench is used in both cases.
![image](https://github.com/user-attachments/assets/7e5b85e5-f6d8-4350-aa99-2f1ebb639382)

## Logic Synthesis
### RTL Design 
It is the behavioural representaion of the requiered specification.
RTL to gate level transition is called synthesis.
the design is converted into gates and connections are made.
Netlist is the output of the synthesis.
### .lib:
It is the collection of logic modules.
It includes basic logic gates like AND, OR, NOT, etc.
It also includes different flavours of same gate, like 2 input AND gate, 3 input AND gate.
## Labs using Yosys

List of Commands in yosys:
<br>

| Task  | Command |
| :-----:	| :-----: | 
| To invoke Yosys | yosys |
| To read librery | read_liberty -lib /home/bhavana/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib |
| To read design | read_verilog good_mux.v |
| To synthesis | synth top good_mux.v |
| To generate netlist | abc -liberty ../path |
| To see realized logic | show |
| To write netlist | write_verilog -noattr good_mux_netlist.v <br> !gvim good_mux_netlist.v |

<br>

![image](https://github.com/user-attachments/assets/3fa82c63-7513-43fe-90dd-97a184d9cf14)

<br>

![image](https://github.com/user-attachments/assets/aa65991f-490d-44d4-8e6c-150e1de454ed)


Writing netlist:
<br> 
![image](https://github.com/user-attachments/assets/338a9ea1-d384-4318-b16f-24276ccd382d)

# Day 2

Open lib File
` gvim /home/bhavana/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib `

<br>

Details in the lib file:
<br>

![image](https://github.com/user-attachments/assets/2873a7ba-f6e3-4452-bb89-6d7e55e6f9a4)

<br>
Different features of each cell:

<br>

![image](https://github.com/user-attachments/assets/cffca201-60db-410a-8e63-61337cf7c3c5)

<br>

Comparing details of same cell but different size:

<br>

Smaller cell has smaller area but high delay compared to larger cell which are faster and consume more area.
![image](https://github.com/user-attachments/assets/c0ecaf2a-0bae-4b42-92c8-73935782bbb6)

<br>

## Heirarchical and Flat Synthesis

### Heirarchical Design

1. Read the multiple_modules.v file
2. ` synth -top multiple_modules `
3. ` show multiple_modules `
![image](https://github.com/user-attachments/assets/0ca6fb1f-8790-4362-8a80-ff47180bfafd)

![image](https://github.com/user-attachments/assets/0334dee4-cecb-4357-9644-f2910afc2cc6)

<br> 
Here it can be seen that heirarchies are reserved. The design containes two submodules.
![image](https://github.com/user-attachments/assets/47372e48-f239-442d-af6a-1f05ebc29387)

### Flat Synthesis
` flatten ` is the command to flat out the netlist.

### To synthesis a submodule in a multiple_modules file
```
read_verilog multiple_modules.v
synth -top sub_module_1
abc -liberty ../path_of_.lib
show
```
![image](https://github.com/user-attachments/assets/a83643e6-4fd2-4bf0-9d9e-6c6e676e113c)
<br>
Module leve synthesis is preferred when
- we need multiple instances of same module.
- Divide and Conquer - need for synthesising part by part.

## Flops
Flops are needed to avoid propogation of gliches.

### Ways of coding Flops:
1. Asynchronous Flops: There is no clock dependency. Asynchronous flip-flops can change their output state immediately in response to input changes. They do not have setup or hold time requirements related to a clock, leading to potential timing issues. They are often used in situations where immediate response to input changes is necessary, such as in certain control circuits.
<br>

![image](https://github.com/user-attachments/assets/d4bf4898-ac6d-4726-933b-3243a5ef8199)

<br>

![image](https://github.com/user-attachments/assets/9b842415-83ca-4daa-a846-244aad4d8613)

<br>
   
2. Synchronous Flops: Synchronous with clock signal. The output state only at specific times determined by a clock signal (e.g., on the rising or falling edge of the clock). Inputs must be stable for a certain period before and after the clock edge (setup and hold times).

<br>

![image](https://github.com/user-attachments/assets/81e6a1ec-e6c6-4cf3-94de-4a5d222e1ba7)

<br>

![image](https://github.com/user-attachments/assets/5d81e115-6416-4f04-906c-20a9a2b83b41)

<br>

## Synthesis of DFF using yosys
Add this command to specify the tool to pick the dff from the
` dfflibmap -liberty ../path-.lib `
<br>

![image](https://github.com/user-attachments/assets/547f48d3-5111-45e5-96ee-6d5442c97b4e)

<br>

![Screenshot from 2024-10-22 00-55-29](https://github.com/user-attachments/assets/9c2c28d4-3b1c-4974-86a7-950ca7aaa499)

<br>

## Optimization 

![image](https://github.com/user-attachments/assets/df657491-b34b-481b-b389-b66c4fc542d9)


![image](https://github.com/user-attachments/assets/3e391a42-cb69-4869-9655-ae9f208fc7a6)

Mult8
<br>

![image](https://github.com/user-attachments/assets/e01b7a6c-4f62-4162-b5a9-b555f0f862f6)

Netlist:
<br>

![image](https://github.com/user-attachments/assets/fd4cf71b-7ac5-4456-a1f5-d082f3bdfb5a)


# Day 3

## Combinational Logic Optimisation
Optimizing is squeezing the logic to get the most optimised design. Optimization results in area and power savings.
Techniques for combinational logic optimization:
- Constant Propogation - Direct Optimization
  <br>

  ![image](https://github.com/user-attachments/assets/fea7acb9-254e-491d-9e91-52a6072b7e62)

- Boolean Logic Optimization
  <br>

  ![image](https://github.com/user-attachments/assets/a434ad43-8b37-45f8-aa88-6059fc837bf9)

## Sequential Logic Optimization
Techniques:
- Basic: Sequential constant propogation
- Advanced: 1. State Optimization
           2. Retiming
           3. Sequential logic cloning (Floor Plan Aware Synthesis)
