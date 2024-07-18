# asic-design-class
This project contains the details of labs conducted in the ASIC design class.
## Lab 1: Executing a simple c program on virtual machine using gcc
### Step 1:Creating a .c file using leafpad editor 
leafpad is the editor which can be installed using the command<br> "sudo snap install leafpad".<br>

A c program file named "sum1ton" is created<br>
![A1](https://github.com/user-attachments/assets/7fa519cf-8f92-44b0-a433-049a67c2e15f)

### Step 2: Write the c program to count the numbers from 1 to n
![A3](https://github.com/user-attachments/assets/382a07e9-5736-46f4-b28f-6b8c6f5f9aae)

### Step 3: Compile and run the c program using gcc
"gcc sum1ton.c"<br>
"./a.out"<br>
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




