# Executing program using RISCV compiler
## Command:<br>
``riscv64-unknown-elf-gcc -ofast -mabi-rv64i -o sum1ton.o sum1ton.c``<br>
`spike pk sum1ton.o`<br>
This gives the sum of numbers from 1 to n.<br>

![VSDL3 1](https://github.com/user-attachments/assets/3308d826-14f2-4d20-8bee-3fbf2f576bc3)
<br>
## Command to debug the program<br>
`spike -d pk sum1ton.o`
<br>
<br>
To run the assembly code line by line, following command is used:<br>
`until pc 0 100b0`<br><br>
Now the pc points to the first instruction that is stored in the memory aadress 100b0.<br>
Press "Enter" to execute the next line.<br><br>
Object dump command can be used to display the assembly instructions and corresponding memory address. <br>
``

## Command to check the content of registers:<br>
`reg 0 <register_name>`<br><br>
For example, the value of register a0 before the execution was 0x0000000000000001 and content in a0 after excetion of `lui a0,0x21` is 0x0000000000021000, which means "load upper immediate" instruction is completed.<br><br>
![VSDL3 2](https://github.com/user-attachments/assets/04965af0-ac85-408e-b029-be3b47b32bd1)
