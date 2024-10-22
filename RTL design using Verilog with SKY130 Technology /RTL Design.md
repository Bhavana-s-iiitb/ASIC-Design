# Installation of Required tools
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
## Simulator:
- Simulator is tool to verify the RTL design by using testbech. iverilog is one of the simulation tool.

- Design is the actual verilog code which has the intended functionality to meet with the required specifications.
- Testbench is the setup to apply stimulus.
iverilog based simualtion flow:
The iverilog simualator takes the design file and the testbench as input and generates a VCD(Value chae dump format) file. The simulaton output can be viewed using gtkwave analyser.
![image](https://github.com/user-attachments/assets/8fc95a48-dcf5-4ed8-8ce0-870e9b37f659)
## VSD Flow

```
mkdir VLSI
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop
```

To simulate a design file:
```
cd VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files
iverilog good_mux.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd
```
The iverilog simulator takes the RTL design file and test bench as input and gives the vcd files as output.
<br>

![image](https://github.com/user-attachments/assets/4444f832-84f0-493c-bf5c-3b1334f5656e)

<br>
To see the file structure :
` gvim tb_good_mux.v -o good_mux.v `
<br>
 
![image](https://github.com/user-attachments/assets/a7e20d02-42cc-441d-9c7a-742fa6bcc0a6)

## Yosys
Synthesizer is the tool for converting RTL design to netlist and YOSYS is the tool we are using as for synthesis.
Netlist is the representation of the design in the form of cells present in the design.

![image](https://github.com/user-attachments/assets/900a9cb6-6b9d-4653-aa40-4184c3f67907)

To verify the synthesized output: 
Give the netlist as input the iverilog simulator along with the same testbench and view the output VCD file using gtkwave analyser. This output should be same as the output obtained from RTL design file.
The set of primary inputs and outputs remain same for both RTL design and netlist simulation. Hence the same testbench is used in both cases.
![image](https://github.com/user-attachments/assets/7e5b85e5-f6d8-4350-aa99-2f1ebb639382)

## Logic Synthesis
### RTL Design 
- It is the behavioural representaion of the requiered specification.
- RTL to gate level transition is called synthesis.
- The design is converted into gates and connections are made.
- Netlist is the output of the synthesis.
### .lib:
- It is the collection of logic modules.
- It includes basic logic gates like AND, OR, NOT, etc.
- It also includes different flavours of same gate, like 2 input AND gate, 3 input AND gate.
## Labs using Yosys

List of Commands in yosys:
<br>

| Task  | Command |
| :-----:	| :-----: | 
| To invoke Yosys | yosys |
| To read librery | read_liberty -lib /home/bhavana/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib |
| To read design | read_verilog good_mux.v |
| To synthesis | synth top good_mux |
| To generate netlist | abc -liberty ../path |
| To see realized logic | show |
| To write netlist | write_verilog -noattr good_mux_netlist.v <br> !gvim good_mux_netlist.v |

Example 1: good_mux.v
<br>
```
read_liberty -lib /home/bhavana/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog ../verilog_files/good_mux.v
synth top good_mux
abc -liberty ../path_to_.lib
show
```
After going through the above steps, the following results can be obtained.
<br>

![image](https://github.com/user-attachments/assets/3fa82c63-7513-43fe-90dd-97a184d9cf14)

<br>

![image](https://github.com/user-attachments/assets/aa65991f-490d-44d4-8e6c-150e1de454ed)


Writing netlist:
` write_verilog -noattr good_mux_netlist.v <br> !gvim good_mux_netlist.v `

After this command a new file named "good_mux_netlist.v" is generated which has the netlist of the design.
<br> 
![image](https://github.com/user-attachments/assets/338a9ea1-d384-4318-b16f-24276ccd382d)

<br>

# Day 2

Open lib File
<br> 
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
<br>

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
Module level synthesis is preferred when
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
Add this command to instruct the tool what library to pick.
` dfflibmap -liberty ../path-.lib `
<br>

![image](https://github.com/user-attachments/assets/547f48d3-5111-45e5-96ee-6d5442c97b4e)

<br>

![Screenshot from 2024-10-22 00-55-29](https://github.com/user-attachments/assets/9c2c28d4-3b1c-4974-86a7-950ca7aaa499)

<br>

## Optimization of Logic

![image](https://github.com/user-attachments/assets/df657491-b34b-481b-b389-b66c4fc542d9)


![image](https://github.com/user-attachments/assets/3e391a42-cb69-4869-9655-ae9f208fc7a6)

### Optimization of Mult8 design
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
  This optimization focuses on identifying and propagating constant values through the logic to simplify the circuit and improve performance. 
- Advanced: 1. State Optimization: Optimization of unused states.
           2. Retiming
           3. Sequential logic cloning (Floor Plan Aware Synthesis): 
Sequential logic optimization involves improving the efficiency, speed, and resource usage of sequential circuits (like flip-flops, registers, and state machines)

## Optimization Labs
### opt_check: And realization

![image](https://github.com/user-attachments/assets/f8394eac-0269-4f34-aca6-da1716f899f0)

### opt_chech2: OR realization 

![image](https://github.com/user-attachments/assets/b45e7621-8d79-4668-b256-ef80d1285d80)

### opt_check3: 3 input AND gate

![image](https://github.com/user-attachments/assets/9fba4f61-b576-40f2-a3b4-f04eb55df8a7)

### opt_check4: 2 input XNOR gate

![image](https://github.com/user-attachments/assets/5453a759-ee59-49ae-b8a9-0ad290c8db1b)

### multiple_module_opt: 

![image](https://github.com/user-attachments/assets/d9e7dc7d-a57a-4999-9812-17b4365577b1)

### multiple_module_opt2

![image](https://github.com/user-attachments/assets/22640e00-2270-4b00-8195-474baf25605f)

## Sequential Optimization lab

### dff_const1

![image](https://github.com/user-attachments/assets/7550cef5-3d1f-4e9f-9e67-f79c29613538)


![image](https://github.com/user-attachments/assets/f6793be7-9053-425b-9838-fe042a0702b9)

### dff_const2

![image](https://github.com/user-attachments/assets/c0ef930e-ca64-455a-b0ae-9a2e202457b4)


![image](https://github.com/user-attachments/assets/291f57ea-3a27-4446-befd-fd794046bed5)

### dff_const3

![image](https://github.com/user-attachments/assets/a96af469-4b16-4feb-be12-3d9f321fa09f)


![image](https://github.com/user-attachments/assets/975fab58-b64c-458b-9f58-54eb80559d75)

### dff_const4

![image](https://github.com/user-attachments/assets/3f97c0f8-6c28-4c7f-83d7-04ff68d7e5f5)

![image](https://github.com/user-attachments/assets/cd1b7967-f8e5-4229-b270-e298a58daaed)


![image](https://github.com/user-attachments/assets/282f33f8-e340-4bd2-bafa-470d0586c2b9)


![image](https://github.com/user-attachments/assets/44e1ba8d-608c-4572-a8f5-04a84ac5d317)

### dff_const5

![image](https://github.com/user-attachments/assets/c4feb30a-8cbf-4caa-af8a-7ebfe35f3c50)

![image](https://github.com/user-attachments/assets/ddb01c4d-dd8d-4f5e-8187-43158a7f9051)


![image](https://github.com/user-attachments/assets/92dad3ae-79c8-46e4-b4aa-35320d1a3ed7)

## Sequential optimizations for unused outputs
Logic which is not used may not be present in the design. This part of the logic which is not used can be ignored.

### counter_opt
There is only one D flip flop inferred instead of 3 after optimization. The unused bits are completely optimized as the output is not affected by these two inputs.

![image](https://github.com/user-attachments/assets/04792016-e6ef-4a40-b39f-f206f9f3e7a8)


After editing the code:
<br>

![image](https://github.com/user-attachments/assets/ed7985fc-790e-4a88-b4b1-38a554ebdc5a)

<br>

![image](https://github.com/user-attachments/assets/02bf58ee-ef6a-49b6-acf6-e150b1bc0548)


# Day 4
## Gate level Simulation
Running the test bench with netlist as design under test.
Netlist is logically same as RTL code and the inputs and outputs are same. Therefore same test bench will align with the design.

### Why GLS
- Verify the logical correctness of design after synthesis
- Ensuring the timing of the design is met. For this GLS needs to be run with delay annotation.

### GLS using iverilog
![image](https://github.com/user-attachments/assets/b1bb864f-2034-40cd-abe1-a3f74573dcd1)

## Synthesis Simulation Mismatch
1. Missing sensitivity List: The simulator works on "activity of inputs". Output changes when the input changes. this is called sensitivity list. If the sensitivity list doesnot contain the required inputs, then there is a mismatch between synthesis and simulation and the desired output may not be obtained.

2. Blocking Vs Non_Blocking Assignments:
   Inside the always block, " = " is the Blocking assignment and " <= " is non_blocking assignment. Blocking statement excecutes the statements in the order it is written whereas the non_blocking statement executes all the RHS when the always block is entered and assigns to LHS which is the parallel evaluation.

   <br>
   Inside an always block of a sequential logic, non_blocking assignment should be used.
   

4. Non Standard Verilog Coding

## GLS Labs
### ternary_operator_mux

![image](https://github.com/user-attachments/assets/543ca221-e2f9-43f7-b633-f3ea159b4433)

![image](https://github.com/user-attachments/assets/09bbd6dc-fd17-426f-ab5c-4a124c5d69ce)


![image](https://github.com/user-attachments/assets/0b5a8fe3-9df2-45f4-86c5-e0b2ea2e677f)

### Missing sensitivity Mis-Match Example

#### Example of bad_mux.v:
<br>

![image](https://github.com/user-attachments/assets/849dd326-e142-4e11-bbad-587b250e651e)

Simulation results:
<br>

 ![image](https://github.com/user-attachments/assets/8462fb60-4af8-4844-9b73-df27b2f09660)


Synthesis results:
<br>

![image](https://github.com/user-attachments/assets/86a2d416-1e5d-4dfa-b559-a18e01a2dd6a)

<br>
It can be seen that there is a miss-matc between results from simulation and synthesis. This is due to logical error in sensitivity list.

Netlist of bad_mux:
<br>

![image](https://github.com/user-attachments/assets/7ab6cf84-bc9f-46df-9d7e-9cdc9fb334b1)


### Blocking mis match example

![image](https://github.com/user-attachments/assets/94bf93a9-76e9-42f5-b62c-a141ebf3ebec)

![image](https://github.com/user-attachments/assets/96ef1cad-3c82-4d8d-97ba-a9f1ca0bea0f)

