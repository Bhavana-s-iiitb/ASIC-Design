# 1 Installation of Tools:
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

- Clock period = 10.3ns
- Setup uncertainty and clock transition will be 5% of clock
- Hold uncertainty and data transition will be 8% of clock.

```
./sta

read_liberty /home/bhavana/VLSI/OpenSTA/lab10/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog /home/bhavana/VLSI/OpenSTA/lab10/bhavana_riscv_netlist.v
link_design rvmyth

create_clock -name clk -period 10.3 [get_ports clk]
set_clock_uncertainty [expr 0.05 * 10.3] -setup [get_clocks clk]
set_clock_uncertainty [expr 0.08 * 10.3] -hold [get_clocks clk]
set_clock_transition [expr 0.05 * 10.3] [get_clocks clk]
set_input_transition [expr 0.08 * 10.3] [all_inputs]

```


![image](https://github.com/user-attachments/assets/27391283-b64f-4ee9-a2da-9ea300b4d1b9)

` report_checks -path_delay max`
<br>

![image](https://github.com/user-attachments/assets/dc81d503-a899-4dbd-8900-ac90b6b64b29)

<br>

`report_checks -path_delay min`

<br>

![image](https://github.com/user-attachments/assets/0b1a3455-7647-4e1b-aec7-0ea9b6b820e9)

# 2 PVT Corner Analysis

### vsdbabysoc_synth.sdc: For specifying constraints.

- Clock period = 10.3ns
- Setup uncertainty and clock transition is 5% of clock
- Hold uncertainty and data transition is 8% of clock

![image](https://github.com/user-attachments/assets/782690fa-3b57-42ef-bcc3-b72282a18221)

<br>

### Tcl script

```
set list_of_lib_files(1) "sky130_fd_sc_hd__tt_025C_1v80.lib"
set list_of_lib_files(2) "sky130_fd_sc_hd__tt_100C_1v80.lib"
set list_of_lib_files(3) "sky130_fd_sc_hd__ff_100C_1v65.lib"
set list_of_lib_files(4) "sky130_fd_sc_hd__ff_100C_1v95.lib"
set list_of_lib_files(5) "sky130_fd_sc_hd__ff_n40C_1v56.lib"
set list_of_lib_files(6) "sky130_fd_sc_hd__ff_n40C_1v65.lib"
set list_of_lib_files(7) "sky130_fd_sc_hd__ff_n40C_1v76.lib"
set list_of_lib_files(8) "sky130_fd_sc_hd__ff_n40C_1v95.lib"
set list_of_lib_files(9) "sky130_fd_sc_hd__ss_100C_1v40.lib"
set list_of_lib_files(10) "sky130_fd_sc_hd__ss_100C_1v60.lib"
set list_of_lib_files(11) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
set list_of_lib_files(14) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
set list_of_lib_files(15) "sky130_fd_sc_hd__ss_n40C_1v60.lib"
set list_of_lib_files(16) "sky130_fd_sc_hd__ss_n40C_1v76.lib"

for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {
read_liberty ./timing/$list_of_lib_files($i)
read_liberty -min avsdpll.lib
read_liberty -max avsdpll.lib
read_liberty -min avsddac.lib
read_liberty -max avsddac.lib
read_verilog  vsdbabysoc_netlist.v
link_design vsdbabysoc
read_sdc vsdbabysoc_synth.sdc
check_setup -verbose
report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} > ./sta_output/min_max_$list_of_lib_files($i).txt

exec echo "$list_of_lib_files($i)" >> ./sta_output/sta_worst_max_slack.txt
report_worst_slack -max -digits {4} >> ./sta_output/sta_worst_max_slack.txt

exec echo "$list_of_lib_files($i)" >> ./sta_output/sta_worst_min_slack.txt
report_worst_slack -min -digits {4} >> ./sta_output/sta_worst_min_slack.txt

exec echo "$list_of_lib_files($i)" >> ./sta_output/sta_tns.txt
report_tns -digits {4} >> ./sta_output/sta_tns.txt

exec echo "$list_of_lib_files($i)" >> ./sta_output/sta_wns.txt
report_wns -digits {4} >> ./sta_output/sta_wns.txt
}

```
<br>


<img width="650" alt="image" src="https://github.com/user-attachments/assets/63ca57c2-8894-4b61-b384-cf65528c02b7">

<img width="650" alt="image" src="https://github.com/user-attachments/assets/54ae33c4-0e60-44cd-9846-740ad6ed90a2">

<br>

After executing these commands in sta prompt, reports will be dumped in sta_output directory. The setup slack and worst negative slack graph analysis is shown below.
<br>


### Table showing values of TNS and WNS
<img width="215" alt="image" src="https://github.com/user-attachments/assets/1bb669bf-ec90-4eae-b9b2-8f9bffd19a14">

<br>

### Graph of TNS and WNS

<img width="600" alt="image" src="https://github.com/user-attachments/assets/8581bbe1-45be-4195-bb81-cb3b9c28b4ad">

<img width="600" alt="image" src="https://github.com/user-attachments/assets/60d2da3d-0c54-468a-847b-f82d69a8eab4">
