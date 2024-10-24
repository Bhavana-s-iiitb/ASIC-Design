# Functional Simulation and Gate Level Simulation of rvmyth.v
To generate netlist of rvmyth.v:
invke yosys
``` 
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog rvmyth.v
read_verilog clk_gate.v
synth -top rvmyth
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog -noattr rvmyth.v
!gvim rvmyth.v
```
This generates the netlist for the design file rvmyth.v
<br>

![Screenshot from 2024-10-24 07-39-22](https://github.com/user-attachments/assets/414451b8-11bc-457f-b959-0b28ab0fb92f)

![Screenshot from 2024-10-24 07-39-39](https://github.com/user-attachments/assets/72ded39f-d918-4442-b340-8f46462751e5)

<br> The generated netlist file:
<br>

![Screenshot from 2024-10-24 07-43-05](https://github.com/user-attachments/assets/168ccd71-3078-4218-b103-a3019958f022)

Simulation of generated netlist:

```
iverilog ../../my_lib/verilog_model/primitives.v ../../my_lib/verilog_model/sky130_fd_sc_hd.v rvmyth.v testbench.v vsdbabysoc.v avsddac.v avsdpll.v clk_gate.v
ls
./a.out
gtkwave dump.vcd
```

The Simulation output:
<br>

![image](https://github.com/user-attachments/assets/23e55328-eef9-408f-b05d-8b048d08f8c2)
