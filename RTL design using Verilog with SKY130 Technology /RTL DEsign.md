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
### VSD Flow

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
