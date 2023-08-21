# RISCV
## Day - 1 : Introduction to RISC-V ISA and GNU compiler toolchain

### Tool Installation
Install the dependencies using the following command :
```
sudo apt-get install libboost-regex-dev
```

**Steps to install the toolchain**
```
git clone https://github.com/kunalg123/riscv_workshop_collaterals.git
cd riscv_workshop_collaterals
chmod +x run.sh
./run.sh
```



```
cd ~/riscv_toolchain/iverilog/
git checkout --track -b v10-branch origin/v10-branch
git pull 
chmod 777 autoconf.sh 
./autoconf.sh 
./configure 
make
sudo make install 
```

Once the toolchain is installed it is necessary to create a PATH variable in bashrc file. To create the path variable follow the steps given below :

```
gedit .bashrc


#Type at last line
export PATH="/home/name/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH" 

# close the bashrc and type in terminal
source .bashrc
```
### Introduction to RISC-V ISA
RISC-V ISA is a base integer ISA and must be present in any implemenatation along with some optional extension. The RISC-V has been designed to support extensive customization and specialization which can be extended with one or more optional instruction-set extensions, but the base integer instructions cannot be redefine. The different instructions included in RISC-V are listed below.

Pseudo instructions - For e.g- mv,li,ret etc
Base integer instruction (RV64I, RV32I)-For e.g-lui,addi etc
Multiply extension (RV64M) -For e.g- mulw,divw etc
Single and double floating point instruction (RV64F, RV64D) -For e.g- flw,fadd etc
Application binary instruction
Memory allocation and stack pointer
The detail of the RISC-V instructions set manual can be found here.

Each base integer set is characterized by the width of the register (XLEN) and size of the user address space. The most important advantage of RISC-V is that it is an open standard instruction which is easily available for academics and commercial purposes free of cost.








### Illustration of the RISC-V gnu toolchain

#### O1 mode 
Consider the simple C program given below which calculates the sum of the number form 1 to n. 

```
#include<stdio.h>
int main()
{
    int i ,sum=0,n=5;
    for (i=1;i<=n;i++)
        sum+=i;
    printf("The sum of numbers from 1 to %d is %d\n",n,sum);
    return 0;
}
```

![Screenshot from 2023-08-19 22-26-15](https://github.com/Priyanshiiitb/RISCV/assets/140998626/ede8172f-9fb5-4490-9426-2db462c03331)








### Data Representation
![Screenshot from 2023-08-19 22-31-00](https://github.com/Priyanshiiitb/RISCV/assets/140998626/c9756e3b-275f-4eee-8653-154c47411dd0)


In RISC-V and computer architecture in general, several terms relate to data representation and storage. Let's explore them:

1. **Byte:** - A byte is the fundamental unit of data storage and representation in computers. It consists of 8 bits and can represent a single character or value.

2. **Word:** - A word typically refers to the natural data size that a processor operates with. In RISC-V, the term "word" can vary based on the architecture. For example, in RV32 (32-bit architecture), a word is 4 bytes (32 bits), while in RV64 (64-bit architecture), a word is 8 bytes (64 bits).

3. **Double Word:** - A double word is twice the size of a word. In RISC-V, for example, in RV32, a double word is 8 bytes (64 bits), and in RV64, a double word is 16 bytes (128 bits).

4. **Least Significant Bit (LSB):** -  The least significant bit is the lowest-order bit in a binary representation. 

5. **Most Significant Bit (MSB):** -  The most significant bit is the highest-order bit in a binary representation. It has the greatest influence on the overall value of a number. The MSB is the bit that represents the largest power of two.


6. **Endianess:** - Endianess refers to how multi-byte data is stored in memory. In a big-endian system, the most significant byte is stored at the lowest memory address, while in a little-endian system, the least significant byte is stored at the lowest memory address. RISC-V supports both big-endian and little-endian modes.

7. **Byte addressing** -  is a memory addressing scheme used in computer systems to identify and access individual bytes of data within the computer's memory. In byte addressing, each individual byte in the memory has a unique address, allowing direct access to and manipulation of single bytes of data. In RISC-V, like in many other computer architectures, memory is byte-addressable.

Understanding these terms is crucial when working with data representation, memory allocation, and programming in computer systems, including the RISC-V architecture.


### Representation of Signed and Unsigned Numbers
#### Unsigned Numbers
Unsigned numbers don’t have any sign, these can contain only magnitude of the number. So, representation of unsigned binary numbers are all positive numbers only.
Since there is no sign bit in this unsigned binary number, so N bit binary number represent its magnitude only. Zero (0) is also unsigned number. Every number in unsigned number representation has only one unique binary equivalent form, so this is unambiguous representation technique. The range of unsigned binary number is from  **0 to ((2^n)-1)**.

#### Signed Numbers
Generally 2's complement representation is used for the signed numbers. 2’s complement of a number is obtained by inverting each bit of given number plus 1 to least significant bit (LSB). So, positive numbers are represented in binary form and negative numbers are represented in 2’s complement form. There is extra bit for sign representation. If value of sign bit is 0, then number is positive and you can directly represent it in simple binary form, but if value of sign bit 1, then number is negative and 2’s complement of given binary number should be taken. In this representation, zero (0) has only one (unique) representation which is always positive. The range of 2’s complement form is from  **(-2^(n-1))  to ((2^(n-1))-1)**.

### Illustration of Signed and Unsigned Numbers in RISC-V
#### Unsigned Numbers

Consider the C code given below which demostrates the maximum unsigned number the RV64I can store. 

```
#include<stdio.h>
#include<math.h>

int main()
{
    unsigned long long int max = (unsigned long long int)(pow(2,64)-1); //Line 1
    // unsigned long long int max = (unsigned long long int)(pow(2,127)-1);// Line 2
    // unsigned long long int max = (unsigned long long int)(pow(2,64)*-1);// Line 3
    // unsigned long long int max = (unsigned long long int)(pow(-2,64)-1);// Line 4
    // unsigned long long int max = (unsigned long long int)(pow(-2,63)-1);// Line 5
    // unsigned long long int max = (unsigned long long int)(pow(2,10)-1);// Line 6
    printf("Highest number represented by unsigned long long  int is %llu \n",max);
    return 0;
}
```

- Line 1 will execute and give the result of (2^64)-1.</br>
- Line 2 will execute and give the result of (2^64)-1 instead of (2^127)-1 since the maximum unsigned value that can be stored in the 64 bit register is (2^64)-1.</br>
- Line 3 will execute and give the result of 0 instead of -(2^64) since the minimum unsigned value that can be stored in 64 bit register is 0.</br>
- Line 4 will execute and give the result of 0 instead of (2^64)-1.</br>
- Line 5 will execute and give the result of 0 instead of -(2^64) since the minimum unsigned value that can be stored in 64 bit register is 0.</br>
- Line 6 will execute and give the result of 1024 since the value of max is less that (2^64)-1.

To compile and execute the C code in RISC-V gnu toolchain follow the steps given below:

```

riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o unsigned.o unsigned.c 
spike  pk unsigned.o 
```

### Output of the execution
![Screenshot from 2023-08-19 21-57-36](https://github.com/Priyanshiiitb/RISCV/assets/140998626/0b40237f-e5fb-44a9-a822-c0034721c795)


#### Signed Numbers

Consider the C code given below which demostrates the maximum and minimum signed number the RV64I can store. 


```
#include<stdio.h>
#include<math.h>

int main()
{
    long long int max = (long long int)(pow(2,63)-1);
    long long int min = (long long int)(pow(-2,63));
    printf("Highest number represented by long long  int is %lld \n",max);
    printf("Smallest number represented by long long  int is %lld \n",min);
    return 0;
}
```
To compile and execute the C code in RISC-V gnu toolchain follow the steps given below:

```

riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o signedHighest.o unsignedHighest.c 
spike  pk signedHighest.o
```

## Day - 2 : Introduction to ABI and Basic Verification Flow

### Introduction to Application Binary Interface
* The application program can directly access the registers of the RISC V architecture using something known as system calls. The ABI (also known as system call interface enables the application to access the hardware resources via registers.
  
* In RISC V architecture, the width of the register is defined as XLEN. For RV64 and RV32, the widths are 64 bits and 32 bits, respectively.
  
* RISC V belongs to the little endian memory addressing system, which means that the least significant byte of a word is stored in the smallest memory address.

An Application Binary Interface is a set of rules enforced by the operating system on a specific architecture. So, Linker converts relocatable machine code to absolute machine code via ABI interface specific to the architecture of machine.
Just like how application program interface (API) is used by application programs to access the standard libraries, an application binary interface or system  call interface is utilised to access hardware resources . The ISA is inherently divided into two parts: *User & System ISA* and *User ISA*  the latter is available to the user directly by system calls. 
  
Now, how does the ABI access the hardware resources? 
- It uses different registers(32 in number) which are each of width `XLEN = 32 bit` for RV32 (~`XLEN = 64 for RV64`) . On a higher level of abstraction these registers are accessed by their respective ABI names.
  
  For base integer instructions there are broadly 3 types of of such registers:
  - I-type : For instructions having immediate values as operands.
  - R-type : For instructions having only registers as operands.
  - S-type : For instructions used for storing operations.

So, it is system call interface used by the application program to access the registers specific to architecture. Overhere the architecture is RISC-V, so to access 32 registers of RISC-V below is the table which shows the calling convention (ABI name) given to registers for the application programmer to use.


### Load,Add And Store Instructions
```
ld x8,16(x23)
add x8,x29,x8
sd x8,8(x23)
```
![Screenshot from 2023-08-20 11-33-11](https://github.com/Priyanshiiitb/RISCV/assets/140998626/dedf4c4c-2b88-466d-8594-cc98213b8808)
![Screenshot from 2023-08-20 11-33-19](https://github.com/Priyanshiiitb/RISCV/assets/140998626/09b4b31a-c3a0-4b98-ac18-5b68f2e3c637)
![Screenshot from 2023-08-20 11-33-27](https://github.com/Priyanshiiitb/RISCV/assets/140998626/c7ba9cbd-caf5-46e3-8034-df0e795b7aba)


### Example of ABI
Consider the C code given below which calculates the sum from 1 to 9 :
```
#include<stdio.h>

extern int load(int x, int y);

int main()
{
    int result = 0;
    int count = 9;
    result = load(0x0,count+1);
    printf("Sum of numbers from 1 to %d is %d\n",count,result);
    
}
```

Consider the assembly code (ASM) given below :
```
.section .text 
.global load
.type load, @function

load:
    add a4, a0, zero
    add a2, a0, a1
    add a3, a0, zero
loop : add a4, a3, a4
       addi a3, a3, 1
       blt a3, a2, loop
       add a0, a4, zero
       ret
```


How to Perform
```
cd ~/RISCV-ISA/riscv_isa_labs/day_2/lab1/
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o custom1_to9.o custom1_to_9.c load.S
riscv64-unknown-elf-objdump -d custom1_to9.o | less
spike pk custom1_to9.o
```

### Outputs of the Lab
![Screenshot from 2023-08-20 10-42-26](https://github.com/Priyanshiiitb/RISCV/assets/140998626/49291d34-dd03-4318-9e33-3e59d86253f9)
![Screenshot from 2023-08-20 10-44-17](https://github.com/Priyanshiiitb/RISCV/assets/140998626/d21171e1-25c8-46dc-8c4e-24e8a05c677d)



### Lab to run c program on RISC-V CPU
```
cd ~/riscv_workshop_collaterals/labs/
chmod 777 rv32im.sh
./rv32im.sh  # Contains necessary commands to convert C to hex
```
**Output, Script(rv32im.sh) and firmare.hex**

!![Screenshot from 2023-08-20 11-03-01](https://github.com/Priyanshiiitb/RISCV/assets/140998626/8f20bdb8-1020-4e4d-bb61-eab299e357c8)

## Day - 3 : Digital Logic with TL-Verilog and Makerchip
### Logic Gates


### Multiplexer Using Ternary Operator
Consider the verilog code for multiplexer gicen below :
```
assign f = s ? x1 : x0;
```
This code uses ternary operator that will realize a simple 2:1 multiplexer hardware in which the output f follows x1 if s is 1 otherwise it will follow x0. The harware and logic gate representation l is shown below :
![Screenshot from 2023-08-20 23-18-22](https://github.com/Priyanshiiitb/RISCV/assets/140998626/527a7dea-48e3-4a21-850f-839fde5106a7)



The higher bit multiplexers can also be realized using the coditional operator. Consider the 4:1 multiplexer code given below :

```
assign f = sel[0] ? a : (sel[1] ? b : (sel[2] ? c : d));
```
This code creates a priority for the inputs with input a getting the highest and input d getting the least. Instead of realizing as a single 4:1 multiplexer it will create a series of 2:1 multiplexers. In this case the sel is a one hot vector i.e, only one of the bit in the sel will be high at a time. The hardware realization is shown below :

![Screenshot from 2023-08-20 23-18-32](https://github.com/Priyanshiiitb/RISCV/assets/140998626/7c982a86-7d36-44b3-9946-9f9dc14466b8)



### Basic Combinational Circuits in Makerchip

#### Pythagorean Example Demo


![Screenshot from 2023-08-20 17-11-43](https://github.com/Priyanshiiitb/RISCV/assets/140998626/d115c275-ee6d-46d5-a534-a6589e78f978)







#### AND gate

The TL-Verilog code is shown below :
```
   $out = $in1 && $in2;
```
![Screenshot from 2023-08-20 17-18-36](https://github.com/Priyanshiiitb/RISCV/assets/140998626/64271452-8b2a-46a2-b35d-6e8ca5fd4f1e)




#### XOR gate

The TL-Verilog code is shown below :
```
   $out = $in1 ^ $in2;
```
![Screenshot from 2023-08-20 21-49-12](https://github.com/Priyanshiiitb/RISCV/assets/140998626/b0d8c5b5-d551-46e6-b30e-d8bc39049652)


#### Vector Addition

The TL-Verilog code is shown below :
```
   $out[4:0] = $in1[3:0] + $in2[3:0];
```
![Screenshot from 2023-08-20 21-51-53](https://github.com/Priyanshiiitb/RISCV/assets/140998626/b79eacf0-2db5-4e66-a216-9b11dbd218e6)


#### 2:1 Multiplexer

The TL-Verilog code is shown below :
```
   $out = $sel ? $in1 : $in0;
```
![Screenshot from 2023-08-20 21-56-44](https://github.com/Priyanshiiitb/RISCV/assets/140998626/75962fd3-0c1a-4a23-9db9-5a8f82836e4e)


#### 2:1 Vector Multiplexer

The TL-Verilog code is shown below :
```
   $out[7:0] = $sel ? $in1[7:0] : $in0[7:0];
```
![Screenshot from 2023-08-20 21-58-11](https://github.com/Priyanshiiitb/RISCV/assets/140998626/2ecea26a-acc5-4325-9dc8-195635b105e5)


#### Calculator

The TL-Verilog code is shown below :
```
   $reset = *reset;
   
   
   $op[1:0] = $random[1:0];
$val1[31:0] = $rand[3:0];
$val2[31:0] = $rand[3:0];

$div[31:0] = $val1/$val2;
$add[31:0] = $val1+$val2;
$mul[31:0] = $val1*$val2;
$sub[31:0] = $val1-$val2;

$out[31:0] = $op[1] ? ($op[0] ? $div : $add):($op[0] ? $mul : $sub) ;
```



![Screenshot from 2023-08-20 22-38-42](https://github.com/Priyanshiiitb/RISCV/assets/140998626/d36a232a-7f05-48a4-bfbe-ba5d1941ac30)


### Sequential Circuits
A sequential circuit is a type of digital circuit that employs memory elements to store information and produce outputs based not only on the current input values but also on the circuit's previous state. Unlike combinational circuits, which generate outputs solely based on the present input values, sequential circuits incorporate feedback loops and memory elements like flip-flops or registers to maintain and utilize their internal state.


### Basic Sequential Circuits in Makerchip

#### Fibonacci Series

The TL-Verilog code for fibonacci series is shown below :
```
   $reset = *reset;
   $num[31:0] = $reset ? 1 : (>>1$num + >>2$num);
```

___
![Screenshot from 2023-08-20 23-25-08](https://github.com/Priyanshiiitb/RISCV/assets/140998626/048b46f8-82e3-4e5f-a6f7-270ef095cb55)

The block diagram of the fibonacci series generator is shown below :
![Screenshot from 2023-08-20 22-51-50](https://github.com/Priyanshiiitb/RISCV/assets/140998626/72c8eaf2-71c1-439d-8ad6-04ab8cf24016)

#### Free running counter

The TL-Verilog code for free running counter is shown below :
```
   $reset = *reset;
   $cnt[31:0] = $reset ? 0 : (>>1$cnt + 1);
```
The block diagram of the free running counter is shown below :
![Screenshot from 2023-08-20 23-26-55](https://github.com/Priyanshiiitb/RISCV/assets/140998626/b5e6b50a-081b-41d5-9f4b-243672c761a3)



#### Sequential Calculator
The TL-verilog code for sequential calculator is shown below :
```
   $reset = *reset;
   
   $cnt2[2:0] = $reset ? 0 : (>>1$cnt2 + 1);
   $cnt3[1:0] = $reset ? 0 : (>>1$cnt3 + 1);
   
   $op[1:0] = $cnt3;
   
   $val1[31:0] = >>1$out;
   $val2[31:0] = $cnt2;
   $sum[31:0] = $val1+$val2;
   $diff[31:0] = $val1-$val2;
   $prod[31:0] = $val1*$val2;
   $div[31:0] = $val1/$val2;
   
   $out[31:0] = $reset ? 32'h0 : ($op[1] ? ($op[0] ? $div : $prod):($op[0] ? $diff : $sum));
```
![Screenshot from 2023-08-20 23-28-59](https://github.com/Priyanshiiitb/RISCV/assets/140998626/8246fc2d-a43a-4ecf-92d0-4141e0e5abf2)




![Screenshot from 2023-08-20 23-14-05](https://github.com/Priyanshiiitb/RISCV/assets/140998626/ec55db5e-4375-4faf-ab45-91c6170cee69)

### Pipelining
Pipelining or timing abstract is an important feature in TL verilog as it can be implemented very easily with fewer codes as compared to system verilog which reduces bugs to a great extent. An example of the pipeling for pythogoras theorem using both TL verilog and system verilog in this repo . In TL verilog pipeling can be implemented by defining the pipeline as |calc and the different pipeline stages should be properly align and are indicated by @1, @2 and so on.


### Basic Pipelined Circuits

#### Pipelined Pythagorean
The TL-Verilog code is given below:
```
\m5_TLV_version 1d: tl-x.org
\m5
   
   // =================================================
   // Welcome!  New to Makerchip? Try the "Learn" menu.
   // =================================================
   
   //use(m5-1.0)   /// uncomment to use M5 macro library.
\SV
   // Macro providing required top-level module definition, random
   // stimulus support, and Verilator config.
   m5_makerchip_module   // (Expanded in Nav-TLV pane.)
   
   `include "sqrt32.v"
\TLV
   $reset = *reset;
   $aa = $rand1[3:0];
   $bb = $rand2[3:0];
   |calc
      @1
         $aa_sq[31:0] = $aa * $aa;
      @2
         $bb_sq[31:0] = $bb * $bb;
      @3
         $cc_sq[31:0] = $aa_sq + $bb_sq;
      @4
         $out[31:0] = sqrt($cc_sq);
   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
\SV
   endmodule
```
 ___
 **@** - Represents the pipelined stage number.
 
 

![Screenshot from 2023-08-21 10-34-52](https://github.com/Priyanshiiitb/RISCV/assets/140998626/b4ddaf43-c4d9-4dbf-a34f-cf3db07cac9e)


#### Error Detection Demo
The TL-Verilog code is given below :
```
|comp
      @1
         $err1 = $bad_input || $illegeal_op;
      @3
         $err2 = $err1 || $over_flow;
      @6
         $err3 = $err2 || $div_by_zer0;

```

![Screenshot from 2023-08-21 10-55-17](https://github.com/Priyanshiiitb/RISCV/assets/140998626/524ada40-6216-4054-811a-b10b3da801bf)


#### Counter and Calculator in Pipeline
The block diagram of the counter with calculator in pipeline is shown below :


The TL-Verilog code is given below :
```
   $reset = *reset;
   $op[1:0] = $random[1:0];
   $val2[31:0] = $rand2[3:0];
   
   |calc
      @1
         $val1[31:0] = >>1$out;
         $sum[31:0] = $val1+$val2;
         $diff[31:0] = $val1-$val2;
         $prod[31:0] = $val1*$val2;
         $div[31:0] = $val1/$val2;
         $out[31:0] = $reset ? 32'h0 : ($op[1] ? ($op[0] ? $div : $prod):($op[0] ? $diff : $sum));
         
         $cnt[31:0] = $reset ? 0 : (>>1$cnt + 1); 

```



#### 2 Cycle Calculator
The block diagram of the 2 cycle calculator is shown below:
![Screenshot from 2023-08-21 12-02-05](https://github.com/Priyanshiiitb/RISCV/assets/140998626/31d2a406-83c4-4b85-a8d3-2bcc03b90066)


The TL-verilog code is shown below :

``` 
$reset = *reset;
   
   |calc
      @1
         $val1[31:0] = >>1$out;
	 $val2[31:0] = $rand2[3:0];

         $sum[31:0] = $val1+$val2;
         $diff[31:0] = $val1-$val2;
         $prod[31:0] = $val1*$val2;
         $quot[31:0] = $val1/$val2;

         $out[31:0] = $reset ? 0 : ($op[1] ? ($op[0] ? $div : $prod):($op[0] ? $diff : $sum));
         
         $cnt[31:0] = $reset ? 0 : >>1$cnt + 1; 

```

![Screenshot from 2023-08-21 10-58-43](https://github.com/Priyanshiiitb/RISCV/assets/140998626/0201c5b3-f45f-44c5-88be-5e2be8a50e14)


### Validity
In Transaction-Level Verilog (TL-Verilog), validity is a concept used to track the state and timing of transactions within a design description. In TL-Verilog, transactions are used to represent higher-level actions or events that occur in a design. A transaction typically consists of a set of signals that represent the data and control information associated with that action. Validity, refers to whether a transaction is considered "valid" or "invalid" based on the state of its associated signals.
Validity is another feature in TL verilog which is asserted if a particular transactions in a pipeline is valid or true. A new scope, called “when” scope is introduced for this and it is denoted as ?$valid. This new scope has many advantages - easier design, cleaner debug, better error checking and automated clock gating. Validity provides :

* Easier debug
* Cleaner design
* Better error checking
* Automated Clock gating


### Illustration of Validity

#### Distance Accumulator
The block diagram of the distance accumulator is shown below :



The TL-Verilog code is given below:
``` 
    calc
      @1
         $reset = *reset;
      ?$valid
         @1
            $aa_sq[31:0] = $aa[3:0] * $aa;
            $bb_sq[31:0] = $bb[3:0] * $bb;
         @2
            $cc_sq[31:0] = $aa_sq + $bb_sq;
         @3
            $out[31:0] = sqrt($cc_sq);
      @4
         $tot_dist[31:0] = $reset ? '0 : ($valid ? (>>1$tot_dist + $out) : $RETAIN);
```

Once the valid signal is asserted the previous value of result will be added with the current value and it result will get updated otherwise the previous value is retained.

![Screenshot from 2023-08-21 12-04-53](https://github.com/Priyanshiiitb/RISCV/assets/140998626/da1f3a34-8cd1-4c69-965a-dd7cdbc24034)


#### 2 Cycle Calculator with Validity
The block diagram of 2 Cycle calculator with validity is shown below :


The TL-Verilog code is given below :
```
   $reset = *reset;
   |calc
      @1
         $valid = $reset ? 0 : >>1$valid+1;
         $valid_or_reset = $valid || $reset;
      ?$valid_or_reset
         @1
            $val1[31:0] = >>2$out;
            $sum[31:0] = $val1+$val2;
            $diff[31:0] = $val1-$val2;
            $prod[31:0] = $val1*$val2;
            $div[31:0] = $val1/$val2;
            $valid = $reset ? 0 : (>>1$valid + 1);
         @2
            $out[31:0] = $reset  ? 32'h0 : ($op[1] ? ($op[0] ? $div : $prod):($op[0] ? $diff : $sum));
```



#### Calculator with Single Value Memory
The block diagram of calculator with single value memory is shown below :



The TL-Verilog code is given below:
```
   |calc
      @0
         $reset = *reset;
         
      @1
         $val1 [31:0] = >>2$out;
         $val2 [31:0] = $rand2[3:0];
         
         $valid = $reset ? 1'b0 : >>1$valid + 1'b1 ;
         $valid_or_reset = $valid || $reset;
         
      ?$vaild_or_reset
         @1   
            $sum [31:0] = $val1 + $val2;
            $diff[31:0] = $val1 - $val2;
            $prod[31:0] = $val1 * $val2;
            $div[31:0] = $val1 / $val2;
            
         @2   
            $mem[31:0] = $reset ? 32'b0 :
                         ($op[2:0] == 3'b101) ? $val1 : >>2$mem ;
            
            $out [31:0] = $reset ? 32'b0 :
                          ($op[2:0] == 3'b000) ? $sum :
                          ($op[2:0] == 3'b001) ? $diff :
                          ($op[2:0] == 3'b010) ? $prod :
                          ($op[2:0] == 3'b011) ? $quot :
                          ($op[2:0] == 3'b100) ? >>2$mem : >>2$out ;

```









## Acknowledgement
- Kunal Ghosh, VSD Corp. Pvt. Ltd.
- Kanish R,Colleague,IIIT B
- Alwin Shaju,colleague,IIITB


  
## Reference 
- https://www.vsdiat.com
- https://en.wikipedia.org/wiki/Toolchain
- https://en.wikipedia.org/wiki/GNU_toolchain
- https://github.com/riscv/riscv-gnu-toolchain
- https://github.com/KanishR1
- https://github.com/riscv-software-src/homebrew-riscv/tree/main
- https://github.com/stevehoover
