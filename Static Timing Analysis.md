# Installation of Tools:
Follow the given commands to install OpenSTA
```
cd
sudo apt-get install cmake clang gcc tcl swig bison flex

git clone https://github.com/parallaxsw/OpenSTA.git
cd OpenSTA
cmake -DCUDD_DIR=/home/bhavana/VLSI/cudd-3.0.0
make
app/sta
```


![image](https://github.com/user-attachments/assets/3fc7e714-d0f4-4ee6-83db-6c242de2f09d)

## Timing Analysis

-Clock period = 10.3ns
-Setup uncertainty and clock transition will be 5% of clock
-Hold uncertainty and data transition will be 8% of clock.

```
./sta

read_liberty /home/bhavana/VLSI/OpenSTA/lab10/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog /home/bhavana/VLSI/OpenSTA/lab10/likith_riscv_netlist.v
link_design rvmyth

create_clock -name clk -period 9.45 [get_ports clk]
set_clock_uncertainty [expr 0.05 * 9.45] -setup [get_clocks clk]
set_clock_uncertainty [expr 0.08 * 9.45] -hold [get_clocks clk]
set_clock_transition [expr 0.05 * 9.45] [get_clocks clk]
set_input_transition [expr 0.08 * 9.45] [all_inputs]




![image](https://github.com/user-attachments/assets/27391283-b64f-4ee9-a2da-9ea300b4d1b9)

` report_checks -path_delay max`
<br>

![image](https://github.com/user-attachments/assets/dc81d503-a899-4dbd-8900-ac90b6b64b29)

<br>

`report_checks -path_delay min`

<br>

![image](https://github.com/user-attachments/assets/0b1a3455-7647-4e1b-aec7-0ea9b6b820e9)

