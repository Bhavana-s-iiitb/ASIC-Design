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


![image](https://github.com/user-attachments/assets/3fa82c63-7513-43fe-90dd-97a184d9cf14)

![image](https://github.com/user-attachments/assets/aa65991f-490d-44d4-8e6c-150e1de454ed)

Writing netlist:
![image](https://github.com/user-attachments/assets/338a9ea1-d384-4318-b16f-24276ccd382d)
