# ASIC Design Class
## Table of Contents
  - [ Executing a simple c program on virtual machine using gcc ](#1-executing-a-simple-c-program-on-virtual-machine-using-gcc)
  - [ Executing the c program using riscv64 ](#2-executing-the-c-program-using-riscv64)
  - [ Instruction types in RISC-V ](#3-instruction-types-in-risc-v)
  - [ Functional Simulation ](#4-functional-simulation)
  - [ Application program compilation](#5-compilation-of-an-application-program-with-gcc-and-risc-v-gcc)
  - [ Converting TL-Verilog into Verilog and Simulation](#7-converting-tl-verilog-to-verilog-and-simulation)
  - [ VSDBabySoC](#8-vsdbabysoc)

<details>
  <summary> Executing a simple c program on virtual machine using gcc</summary>

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

</details>

<details>
  <summary> Executing the c program using riscv64</summary>

### Command to run the program using RISCV compiler:<br>
``riscv64-unknown-elf-gcc -ofast -mabi-rv64i -o sum1ton.o sum1ton.c``<br>
`spike pk sum1ton.o`<br>
This gives the sum of numbers from 1 to n.
<br><br>
![VSDL3 1](https://github.com/user-attachments/assets/8f30ce99-3b21-44dc-af75-e1afcc1a73f5)
<br>

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




### Command to debug the program

`spike -d pk sum1ton.o`
<br>
A debbuger is opened.
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
### Command to check the content of registers:<br>
`reg 0 <register_name>`<br><br>
For example, the value of register a0 before the execution was 0x0000000000000001 and content in a0 after excetion of `lui a0,0x21` is 0x0000000000021000, which means "load upper immediate" instruction is completed.<br><br>
<img src = "https://github.com/user-attachments/assets/04965af0-ac85-408e-b029-be3b47b32bd1" width = "700" height ="400">

</details>

<details>
  <summary>Instruction types in RISC-V</summary>


<img width="772" alt="Instruction format" src="https://github.com/user-attachments/assets/9d973526-ae08-4854-bee5-1c001c16389a">

### 3.1 R-Type:
The Register type instructions involve operations carried out on the register rather than memory locations. This format of instructions are used to perform arithmetic and logical operations.<br>
* OP-code or Operation code field is 7 bit in lenght and it specifies the type of instuction format used such as r-type, s-type, or j-type. <br>
* rd or Destination register field is 5 bits in lenght which indicates the register to which the result of the operation is stored.<br>
* rs and rt are the two source registers which are 5 bits each in lenght.<br>
* funct3 feild is 3 bit long and it indicates the type of operation performed such as addition, subratiocn or logical operation.<br>
* funct7 feild also specifies the type of operation ie., wheather multiplication or shift operation is being performed.<br>
Example: ADD r1, r2, r3 --> Adds the content in r3 and r2 and stores the sum in r1.
### 3.2 I-Type:
I-Type format is used for instructions that operate with immediate value.
* Immediate feild is the first 12 bits of the instuction which stores the address of the memory location.<br>
All other feilds are similar to that of R-Type format.<br>
Example: lw  r1, 10(r3) --> r1 = content in the memory location of (10+r3).
### 3.3 J-Type:
J-type instruction is used to perform an unconditional jump to a specified memory location. This type of instruction can be used while implementing loops or to transfer control within the program. It has two feilds for immediate values and one feild to speicfy register.
Example: jal rd, 10

### 3.4 U-Type:
U-Type format is used to load 20 bit immediate value into a register.<br>

Examples: lui r4, 20 ---> load upper immediate.

### 3.5 S-Type:
S-Type instructions store value from register to the specified memory location. The format consists of one source register, an immediate value and another register.<br>
Example sw r4, 20(r1)

### 3.6 B-Type:
B-Type instructions are used to perform conditional jumps.Here the condition is checked first and if the condition is true then the control jumps to the specified memory location.<br>
Example: BEQ r1,r2,25 --> if r1=r2 is true then, control jumps to the instruction stored in the immidiate valeu.

### Conversion of given assemble level code into machine level code:
<br>

 | Instructions | Instruction Type | 32 bit code | Hex code |
| 	:-----:	 | 	:-----:	 | 	:-----:	 | :-----: |
| ADD r6, r7, r8 | R-Type | 00000000100000111000001100110011 | 0x00F30333|
| SUB r8, r6, r7 | R-Type | 01000000011100110000010000110011 | 0x40C303B3 |
| AND r7, r6, r8 | R-Type | 00000000100000110111001110110011 | 0x008373B3 |
| OR r8, r7, r5 | R-Type | 00000000010100111110010000110011 | 0x0053E433 |
| XOR r8, r6, r4 | R-Type| 00000000010000110100010000110011 | 0x00434433 |
| SLT r10, r2, r4 | R-Type | 00000000010000010010010100110011 | 0x00412533 |
| ADDI r12, r3, 5 | I-Type | 00000000010100011000011000010011 | 0x00518303 |
| SW r3, r1, 4 | S-Type | 00000000001100001010001000100011 | 0x0030A223 |
| SRL r16, r11, r2| R-Type | 00000000001001011101100000110011 | 0x0025D833 |
| BNE r0, r1, 20| B-Type | 0000010000100000001010001100011 | 0x01400063 |
| BEQ r0, r0, 15 | B-Type | 00011100000000000000000001100011 | 0x00000F63 |
| LW r13, r11, 2 | I-TYpe | 00000000001001011010011010000011 | 0x0025A683 |
| SLL r15, r11, r2 | R-Type | 00000000001001011001011110110011 | 0x002597B3 |

### Difference between RISC-V ISA and Hardcore ISA
<img width="905" alt="image" src="https://github.com/user-attachments/assets/ec736852-e82a-4f01-8d63-88df6567c737">
<br> 

 | Instructions | RISC-V ISA | Hardcore ISA |
| :-----:	| :-----: | :-----:	|
| ADD r6, r7, r8 | 0x00F30333| 0x00E83020 |
| SUB r8, r6, r7 | 0x40C303B3 | 0x00C74022 |
| AND r7, r6, r8 | 0x008373B3 | 0x00C83824 |
| OR r8, r7, r5 | 0x0053E433 | 0x00E54025 |
| XOR r8, r6, r4 | 0x00434433 | 0x00C44026 |
| SLT r10, r2, r4 | 0x00412533 | 0x0044502A |
| ADDI r12, r3, 5 | 0x00518303 | 0x206C0005 |
| SW r3, r1, 4 | 0x0030A223 | 0xAC230004 |
| SRL r16, r11, r2 | 0x0025D833 | 0x1628006 |
| BNE r0, r1, 20 | 0x01400063 | 0x14010014 |
| BEQ r0, r0, 15 | 0x00000F63 | 0x2000000F |
| LW r13, r11, 2 | 0x0025A683 | 0x46B60002 |
| SLL r15, r11, r2 | 0x002597B3 | 0x1627804 |

</details>


<details>
  <summary>Functional Simulation</summary>

### Icarus Verilog:
It is an implementation of the Verilog hardware description language. The Icarus Verilog compiles the verilog program that can be run to perform the simulation.<br>

### gtkwave:
It is used to view the VCD(Value Change Dump) waveform.<br>
#### Output Waveforms
1. add r6,r1,r2 --> MEM[0] <= 32'h02208300;
![A4 2](https://github.com/user-attachments/assets/b23dfe0d-5448-4fc1-bfb4-e29a9869a793)

2. sub r7,r1,r2 --> MEM[1] <= 32'h02209380;
![A4 3](https://github.com/user-attachments/assets/d2ccc752-cfba-403a-8c88-4b99132349ea)

3. and r8,r1,r3 --> MEM[2] <= 32'h0230a400;
![A4 5](https://github.com/user-attachments/assets/78ecf99c-700e-410d-aa98-ea711032723e)

4. or r9,r2,r5 --> MEM[3] <= 32'h02513480;
![A4 6](https://github.com/user-attachments/assets/531f5116-46e6-4ff5-8e8a-c3d6d4be2324)

5. xor r10,r1,r4 --> MEM[4] <= 32'h0240c500;
![A4 7](https://github.com/user-attachments/assets/20b0263f-bfd1-462c-99c1-63145710b901)

6. slt r11,r2,r4 --> MEM[6] <= 32'h00520600; 
![A4 8](https://github.com/user-attachments/assets/fa85e473-5404-4b43-ab9c-6c9d964f5800)
 7. addi r12,r4,5 --> MEM[6] <= 32'h00520600;
![A4 9](https://github.com/user-ttachments/assets/fcaab012-617b-4d46-be6a-b4bc9db5dbb9)
8. sw r3,r1,2 --> MEM[7] <= 32'h00209181;
![A4 10](https://github.com/user-attachments/assets/11460434-9cda-423b-9d96-06dda4f02852)
9. lw r13,r1,2 --> MEM[8] <= 32'h00208681;
![A4 12](https://github.com/user-attachments/assets/b8a014f4-1e96-4245-8f7d-f51cf46cd272)
10. beq r0,r0,15 --> MEM[9] <= 32'h00f00002; 
![A4 13](https://github.com/user-attachments/assets/5cfe6353-0115-49a9-9d2f-17cb39d00108)

11. add r14,r2,r2 --> MEM[25] <= 32'h00210700;
![A4 14](https://github.com/user-attachments/assets/ef1f9b06-2262-4009-9caa-816ddfefafd8)

</details>

<details>
  <summary>Compilation of an application program with gcc and risc-v gcc</summary>

### Application: Currency Converter
c program:

```
#include <stdio.h>
 
int main()
{
  float amount;
  float rupee, dollar, pound, euro;
  int choice;
 
  printf("Following are the Choices:");
  printf("\nEnter 1: Ruppe");
  printf("\nEnter 2: Dollar");
  printf("\nEnter 3: Pound");
  printf("\nEnter 4: Euro");
  choice = 1;
  amount = 274;
  switch (choice)
  {
    case 1: // Ruppe Conversion
        dollar = amount / 83;
        printf("%.2f Rupee =  %.2f dollar", amount, dollar);
 
        pound = amount / 107;
        printf("\n%.2f Rupee =  %.2f pound", amount, pound);
 
        euro = amount / 92.5 ;
        printf("\n%.2f Rupee =  %.2f euro", amount, euro);
        break;
 
    case 2: // Dollar Conversion
        rupee = amount * 83;
        printf("\n%.2f Dollar =  %.2f rupee", amount, rupee);
 
        pound = amount *0.78;
        printf("\n%.2f Dollar =  %.2f pound", amount, pound);
 
        euro = amount *0.87;
        printf("\n%.2f Dollar =  %.2f euro", amount, euro);
        break;
 
    case 3: // Pound Conversion
        rupee = amount * 107;
        printf("\n%.2f Pound =  %.2f rupee", amount, rupee);
 
        dollar = amount *1.26;
        printf("\n%.2f Pound =  %.2f dollar", amount, dollar);
 
        euro = amount *92.5;
        printf("\n%.2f Pound =  %.2f euro", amount, euro);
        break;
 
    case 4: // Euro Conversion
        rupee = amount * 83;
        printf("\n%.2f Euro =  %.2f rupee", amount, rupee);
 
        dollar = amount *1.14;
        printf("\n%.2f Euro =  %.2f dollar", amount, dollar);
 
        pound = amount *0.90;
        printf("\n.2%f Euro =  %.2f pound", amount, pound);
        break;
 
     //Default case
    default:
        printf("\nInvalid Input");
  }
 
  return 0;
}
```

### Compilation using gcc and risc-v gcc:
![14 3](https://github.com/user-attachments/assets/e95150b2-8caf-4662-a088-885078c9b784)

</details>

<details>
  <summary>Converting TL-Verilog to verilog and simulation</summary>

### Installation of packages
```
sudo apt install make python python3 python3-pip git iverilog gtkwave
sudo apt-get install python3-venv
python3 -m venv .venv
source ~/.venv/bin/activate
pip3 install pyyaml click sandpiper-saas
```
### Clone the repository
```
git clone https://github.com/manili/VSDBabySoC.git
cd VSDBabySoC
```
### Convert .tlv file into .v file
```
sandpiper-saas -i home/vsduser/VSDBabySoC/src/module/bhavana_rvmyth.tlv -o bhavana_rvmyth.v --bestsv --noline -p verilog --outdir home/vsduser/VSDBabySoC/src/module/
```
![WhatsApp Image 2024-08-27 at 12 09 46 AM (2)](https://github.com/user-attachments/assets/52b87057-580f-489a-b11d-c3e7d479c2f4)
<br>

``` make pre_synth_sim ```

<br>

![WhatsApp Image 2024-08-27 at 12 09 46 AM (3)](https://github.com/user-attachments/assets/d25ff81a-598a-4a0d-8936-223fc4261c6b)


### Compilation and Simulation
```
iverilog -o output/pre_synth_sim.out -DPRE_SYNTH_SIM home/vsduser/VSDBabySoC/src/module/testbench.v -I src/include -I home/vsduser/VSDBabySoCsrc/module

cd output
./pre_synth_sim.out
 ```

<br>

![WhatsApp Image 2024-08-27 at 12 09 47 AM](https://github.com/user-attachments/assets/b9d3bcdd-2f6b-4dfd-852b-7b599df9b810)

### To open waveform using gtkwave
``` gtkwave pre_synth_sim.out ```

<br>

![WhatsApp Image 2024-08-27 at 12 09 48 AM](https://github.com/user-attachments/assets/2319c9cd-5b55-400d-9890-1ad3f4f71a0d)

<br>
Output in binary:

![WhatsApp Image 2024-08-27 at 7 00 23 AM](https://github.com/user-attachments/assets/b9efa767-cf1e-41c9-ab0c-881d0b2f6290)

<br>

Comparision of waveforms with makerchip results:
<br>

<img width="960" alt="Screenshot 2024-08-22 115754" src="https://github.com/user-attachments/assets/3bd803ab-63c7-4b24-b04c-5f25b72f7dc5">
<img width="960" alt="Screenshot 2024-08-21 194140" src="https://github.com/user-attachments/assets/8f3dd11b-8ccd-4e2d-93bf-33255009ca92">
<img width="877" alt="Screenshot 2024-08-21 192246" src="https://github.com/user-attachments/assets/19a67575-aa76-495c-871b-700bf32aac48">

</details>

<details>
  <summary>VSDBabySoC</summary>

VSDBabySoC is a small RISCV-based System on a Chip (SoC). It contains one RVMYTH microprocessor, a PLL generator and a DAC to communicate with analog devices.
### PLL
Phase Locked Loop is an anolog circuit used for frequency synthesis. The function of the PLL is to compare the distributed clock to the incoming reference clock, and vary the phase and frequency of its output until the reference and feedback clocks are phase and frequency matched.

### DAC
Digital to Analog Converter converts input digital signal into an analog signal. DACs are used in many applications, and can be integrated into system on a chip (SOC) designs. A DAC converts a binary input number into an analog output. A binary number on the input causes a corresponding voltage on the output. 

### Commands:

Clone this git:
` git clone https://github.com/Subhasis-Sahu/BabySoC_Simulation.git `

And run these commands
```
cd BabySoC_Simulation
iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
./pre_synth_sim.out
```
![image](https://github.com/user-attachments/assets/14d4303e-6ad5-4c26-858d-c42d251d6a9b)

To open gtkwave:
```
gtkwave pre_synth_sim.vcd
```
![VirtualBox_bhavana_02_09_2024_18_04_46_1](https://github.com/user-attachments/assets/0d4bab6d-df7e-4b4c-bef5-d56d053f0136)

![VirtualBox_bhavana_03_09_2024_02_57_57_1](https://github.com/user-attachments/assets/45f71ade-24f6-4577-8098-1f6b430f2dd6)

</details>
