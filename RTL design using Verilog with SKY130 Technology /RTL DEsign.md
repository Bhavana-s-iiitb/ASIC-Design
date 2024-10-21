### Installation of Required tools
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
 ## To install iverilog:
 ` sudo apt-get install iverilog `

 ## To install gtkwave:
    ``` 
    sudo apt update
    sudo apt install gtkwave 
    ```

![Screenshot from 2024-10-21 19-31-10](https://github.com/user-attachments/assets/9e206e18-d0b1-46eb-bcae-824f434e2e46)

