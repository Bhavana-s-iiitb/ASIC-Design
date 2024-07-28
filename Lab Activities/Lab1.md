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
<img src = "https://github.com/user-attachments/assets/126de2b8-05e5-4d38-a8c5-76609764fb0b" width="400" height="400">
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

# Instruction types in RISC-V
![image](https://github.com/user-attachments/assets/1b34ccc8-a87f-435b-9096-cc76b8b48c2b)


### 1.R-Type:
The Register type instructions involve operations carried out on the register rather than memory locations. This format of instructions are used to perform arithmetic and logical operations.<br>
* OP-code or Operation code field is 7 bit in lenght and it specifies the type of instuction format used such as r-type, s-type, or j-type. <br>
* rd or Destination register field is 5 bits in lenght which indicates the register to which the result of the operation is stored.<br>
* rs and rt are the two source registers which are 5 bits each in lenght.<br>
* funct3 feild is 3 bit long and it indicates the type of operation performed such as addition, subratiocn or logical operation.<br>
* funct7 feild also specifies the type of operation ie., wheather multiplication or shift operation is being performed.<br>
### 2. I-Type:
I-Type format is used for instructions that operate with immediate value.
* Immediate feild is the first 12 bits of the instuction which stores the address of the memory location.<br>
All other feilds are similar to that of R-Type format.<br>
### 3. J-Type:
J-type instruction is used to perform an unconditional jump to a specified memory location. This type of instruction can be used while implementing loops or to transfer control within the program. It has two feilds for immediete values and one feild to speicfy register.

### 4. U-Type:
U-Type format is used to load 20 bit immedeiate value into a register.<br>

Examples:

### 5. S-Type:
S-Type instructions store value from register to the specified memory location.

### 6. B-Type:
B-Type instructions are used to perform conditional jumps.

Conversion of given Assemble level code into machine level code:
<br>

 | Instructions | Instruction Type | 32 bit code | Hex code |
| 	:-----:	 | 	:-----:	 | 	:-----:	 | :-----: |
| ADD r6, r7, r8 | R-Type | 0000000010000011100000110011001 | 0x00F30333|
| SUB r8, r6, r7 | R-Type | 01000000011100110000010000110011 | 0x40C303B3 |
| AND r7, r6, r8 | R-Type | 00000000100000110000001110110011 | 0x00C32333 |
| OR r8, r7, r5 | R-Type | 000000000101001111100100001100111 | 0x00B32333 |
| XOR r8, r6, r4 | R-Type| 00000000010000110100010000110011 | 0x00034333 |
| SLT r10, r2, r4 | R-Type | 00000000010000010010010100110011 | 0x000282333
| ADDI r12, r3, 5 | I-Type |
| SW r3, r1, 4 | I-Type
| SRL r16, r11, r2| R-Type |
| BNE r0, r1, 20| B-Type |
| BEQ r0, r0, 15 | B-Type |
| LW r13, r11, 2 | I-TYpe |
| SLL r15, r11, r2 | R-Type |

<br>
"Identify various RISC-V instruction type (R, I, S, B, U, J) and exact 32-bit instruction code in the instruction type format for below RISC-V instructions
 ADD r6, r7, r8
 SUB r8, r6, r7
 AND r7, r6, r8
 OR r8, r7, r5
 XOR r8, r6, r4
 SLT r10, r2, r4
 ADDI r12, r3, 5
 SW r3, r1, 4
 SRL r16, r11, r2
 BNE r0, r1, 20
 BEQ r0, r0, 15
 LW r13, r11, 2
 SLL r15, r11, r2
  
 Upload the 32-bit pattern on Github"
 <br>

 | 	header1	 | 	header2	 | 	header3	 | 
| 	:-----:	 | 	:-----:	 | 	:-----:	 | 
| 	Value1	| 	Value2	| 	Value3	 | 
| 	Value1	| 	Value2	| 	Value3	 | 
| 	Value1	| 	Value2	| 	Value3	 | 
| 	Value1	| 	Value2	| 	Value3	 | 
| 	Value1	| 	Value2	| 	Value3	 | 
