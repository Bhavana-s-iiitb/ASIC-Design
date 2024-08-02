## ASIC Design Class
# Table of Contents
  - [Executing a simple c program on virtual machine using gcc ](#executing-a-simple-c-program-on-virtual-machine-using-gcc)
  - [ Lab 2 Executing the c program using riscv64 ](#lab-2-executing-the-c-program-using-riscv64)
  - [ Instruction types in RISC-V ](#instruction-types-in-risc-v)
  - [ Functional Simulation ](#functional-simulation)

## Executing a simple c program on virtual machine using gcc
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

## Lab 2 Executing the c program using riscv64
![A7](https://github.com/user-attachments/assets/19a188c5-08b5-42fa-82d1-896f8d207b1c)<br>
Output gives the machine level instructions of the program:<br>
![A8](https://github.com/user-attachments/assets/3b0fc518-3644-43a4-b81d-5a8804e3de15)

#### -o1 optimization
<img src = "https://github.com/user-attachments/assets/126de2b8-05e5-4d38-a8c5-76609764fb0b" width="400" height="400">
<br>

### Using -ofast instead of -o1 
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

# Instruction types in RISC-V

<img width="772" alt="Instruction format" src="https://github.com/user-attachments/assets/9d973526-ae08-4854-bee5-1c001c16389a">

### 1.R-Type:
The Register type instructions involve operations carried out on the register rather than memory locations. This format of instructions are used to perform arithmetic and logical operations.<br>
* OP-code or Operation code field is 7 bit in lenght and it specifies the type of instuction format used such as r-type, s-type, or j-type. <br>
* rd or Destination register field is 5 bits in lenght which indicates the register to which the result of the operation is stored.<br>
* rs and rt are the two source registers which are 5 bits each in lenght.<br>
* funct3 feild is 3 bit long and it indicates the type of operation performed such as addition, subratiocn or logical operation.<br>
* funct7 feild also specifies the type of operation ie., wheather multiplication or shift operation is being performed.<br>
Example: ADD r1, r2, r3 --> Adds the content in r3 and r2 and stores the sum in r1.
### 2. I-Type:
I-Type format is used for instructions that operate with immediate value.
* Immediate feild is the first 12 bits of the instuction which stores the address of the memory location.<br>
All other feilds are similar to that of R-Type format.<br>
Example: lw  r1, 10(r3) --> r1 = content in the memory location of (10+r3).
### 3. J-Type:
J-type instruction is used to perform an unconditional jump to a specified memory location. This type of instruction can be used while implementing loops or to transfer control within the program. It has two feilds for immediate values and one feild to speicfy register.
Example: jal rd, 10

### 4. U-Type:
U-Type format is used to load 20 bit immediate value into a register.<br>

Examples: lui r4, 20 ---> load upper immediate.

### 5. S-Type:
S-Type instructions store value from register to the specified memory location. The format consists of one source register, an immediate value and another register.<br>
Example sw r4, 20(r1)

### 6. B-Type:
B-Type instructions are used to perform conditional jumps.Here the condition is checked first and if the condition is true then the control jumps to the specified memory location.<br>
Example: BEQ r1,r2,25 --> if r1=r2 is true then, control jumps to the instruction stored in the immidiate valeu.

### Conversion of given assemble level code into machine level code:
<br>

 | Instructions | Instruction Type | 32 bit code | Hex code |
| 	:-----:	 | 	:-----:	 | 	:-----:	 | :-----: |
| ADD r6, r7, r8 | R-Type | 0000000010000011100000110011001 | 0x00F30333|
| SUB r8, r6, r7 | R-Type | 01000000011100110000010000110011 | 0x40C303B3 |
| AND r7, r6, r8 | R-Type | 00000000100000110000001110110011 | 0x00C32333 |
| OR r8, r7, r5 | R-Type | 000000000101001111100100001100111 | 0x00B32333 |
| XOR r8, r6, r4 | R-Type| 00000000010000110100010000110011 | 0x00034333 |
| SLT r10, r2, r4 | R-Type | 00000000010000010010010100110011 | 0x000282333
| ADDI r12, r3, 5 | I-Type | 00000000010100011000011000010011 | 0x00D30313 |
| SW r3, r1, 4 | S-Type | 00000000001100001010001000100011 | 0x00012023 |
| SRL r16, r11, r2| R-Type | 00000000001001011101100000110011 | 0x002B0A33 |
| BNE r0, r1, 20| B-Type | 0001010000100000001010101100011 | 0x01400063 |
| BEQ r0, r0, 15 | B-Type | 00011100000000000000000001100011 | 0x00000F63 |
| LW r13, r11, 2 | I-TYpe | 00000000001001011010011010000011 | 0x0026A023
| SLL r15, r11, r2 | R-Type | 00000000001001011001011110110011 | 0x002B5B33 |


## Functional Simulation
### Icarus Verilog:
It is an implementation of the Verilog hardware description language. The Icarus Verilog compiles the verilog program that can be run to perform the simulation.<br>

### gtkwave:
It is used to view the VCD(Value Change Dump) waveform.<br>
#### Output Waveforms
1. add r6,r1,r2 --> MEM[0] <= 32'h02208300;
![A4 2](https://github.com/user-attachments/assets/b23dfe0d-5448-4fc1-bfb4-e29a9869a793)
<br>
2. sub r7,r1,r2 --> MEM[1] <= 32'h02209380;
![A4 3](https://github.com/user-attachments/assets/d2ccc752-cfba-403a-8c88-4b99132349ea)
<br>
3. and r8,r1,r3 --> MEM[2] <= 32'h0230a400;
![A4 5](https://github.com/user-attachments/assets/78ecf99c-700e-410d-aa98-ea711032723e)
<br>
4. or r9,r2,r5 --> MEM[3] <= 32'h02513480;
![A4 6](https://github.com/user-attachments/assets/531f5116-46e6-4ff5-8e8a-c3d6d4be2324)
<br>
5. xor r10,r1,r4 --> MEM[4] <= 32'h0240c500;
![A4 7](https://github.com/user-attachments/assets/20b0263f-bfd1-462c-99c1-63145710b901)
<br>
6. slt r11,r2,r4 --> MEM[6] <= 32'h00520600; 
![A4 8](https://github.com/user-attachments/assets/fa85e473-5404-4b43-ab9c-6c9d964f5800)
<br> 
7. addi r12,r4,5 --> MEM[6] <= 32'h00520600;
![A4 9](https://github.com/user-ttachments/assets/fcaab012-617b-4d46-be6a-b4bc9db5dbb9)
<br>
8. sw r3,r1,2 --> MEM[7] <= 32'h00209181;
![A4 10](https://github.com/user-attachments/assets/11460434-9cda-423b-9d96-06dda4f02852)
<br>
9. lw r13,r1,2 --> MEM[8] <= 32'h00208681;
![A4 12](https://github.com/user-attachments/assets/b8a014f4-1e96-4245-8f7d-f51cf46cd272)
<br>
10. beq r0,r0,15 --> MEM[9] <= 32'h00f00002; 
![A4 13](https://github.com/user-attachments/assets/5cfe6353-0115-49a9-9d2f-17cb39d00108)
<br>
11. add r14,r2,r2 --> MEM[25] <= 32'h00210700;
![A4 14](https://github.com/user-attachments/assets/ef1f9b06-2262-4009-9caa-816ddfefafd8)

