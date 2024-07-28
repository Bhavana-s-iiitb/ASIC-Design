## Lab 1: Executing a simple c program on virtual machine using gcc
### Step 1:Creating a .c file using leafpad editor 
leafpad is the editor which can be installed using the command<br> 
``"sudo snap install leafpad"``<br>

A c program file named "sum1ton" is created<br>
`leafpad sum1ton.c`<br>
<img src ="https://github.com/user-attachments/assets/7fa519cf-8f92-44b0-a433-049a67c2e15f" width="300" height="300">

### Step 2: Write the c program to count the numbers from 1 to n
![A3](https://github.com/user-attachments/assets/382a07e9-5736-46f4-b28f-6b8c6f5f9aae)

### Step 3: Compile and run the c program using gcc
``"gcc sum1ton.c"<br>``
``"./a.out"``<br>
The sum of numbers from 1 to 20 is displayed as shown<br>
![A5](https://github.com/user-attachments/assets/b6c230f3-de57-4586-ad71-ec8df1f71fc7)

## Lab 2: Executing the c program using riscv64
![A7](https://github.com/user-attachments/assets/19a188c5-08b5-42fa-82d1-896f8d207b1c)<br>
Output gives the machine level instructions of the program:<br>
![A8](https://github.com/user-attachments/assets/3b0fc518-3644-43a4-b81d-5a8804e3de15)

#### -o1 optimization
![A9](https://github.com/user-attachments/assets/126de2b8-05e5-4d38-a8c5-76609764fb0b)
<br>
#### Using -ofast instead of -o1 
![A10](https://github.com/user-attachments/assets/5317125d-f3c6-40c3-81e1-fd7e4070fe04)
#### Less number of instructions compared to -o1 indicating more optimization
![A11](https://github.com/user-attachments/assets/716a8280-d868-4ff1-81ad-c600bdce297e)


# Lab 2 Executing program using RISCV compiler
## Command to run the program using RISCV compiler:<br>
``riscv64-unknown-elf-gcc -ofast -mabi-rv64i -o sum1ton.o sum1ton.c``<br>
`spike pk sum1ton.o`<br>
This gives the sum of numbers from 1 to n.
<br><br>
![VSDL3 1](https://github.com/user-attachments/assets/8f30ce99-3b21-44dc-af75-e1afcc1a73f5)
<br>
## Command to debug the program

``spike -d pk sum1ton.o``
<br>
A debbuger is opened.<br>
<br>
To run the assembly code line by line, following command is used:<br>
`until pc 0 100b0`<br><br>
Now the PC points to the first instruction that is stored in the memory aadress 100b0.<br>
Press "Enter" to execute the next line.<br><br>
Object dump command can be used to display the assembly instructions and corresponding memory address. <br>
`riscv64-unknown-elf-objdump -d sum1ton.0 |less`
<br> This gives the content of the assembler.<br><br>
<img src = "https://github.com/user-attachments/assets/3b0fc518-3644-43a4-b81d-5a8804e3de15" width="650" height="300">
<br>
## Command to check the content of registers:<br>
`reg 0 <register_name>`<br><br>
For example, the value of register a0 before the execution was 0x0000000000000001 and content in a0 after excetion of `lui a0,0x21` is 0x0000000000021000, which means "load upper immediate" instruction is completed.<br><br>
<img src = "https://github.com/user-attachments/assets/04965af0-ac85-408e-b029-be3b47b32bd1" width = "700" height ="400">


