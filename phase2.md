

## Phase_2 Solution 

The goal of this phase is to call the function <touch2> with the unique cookie for target24 as the single argument. The idea is to 
set %rip to the address of the inserted assembly instruction that moves the value of the cookie to %rdi, and then return/jump to touch2. 

&nbsp;  
  
The main challenge of this phase is to clearly understand the dynamics of the procedures. Writing assembly code is the easy part which 
consists of two lines of x86-64 code that is decoded into sequences of hexadecimals with the programs included in the initial tar file. 

&nbsp;  
  
Continuing from the <getbuf> procedure which is detailed in the phase_1 solution, when getbuf is called the exploit string must input data 
in a way that when the program tries to return from getbuf should jump to the beginning of of the instructions that set %rdi a to the value of the cookies. Then jump to the beginning of touch2. 

&nbsp;
  
The decoded mov instruction consists of 8 bytes, which can be positioned at anyplace within size allocated for the buffer. For the sake 
of fun I placed the code in the middle of the buffer for target24. Then the location to the beginning of the instruction must be computed 
and positioned right above the beginning getbuf’s stack frame. After the program tries to return and jumps to the beginning of the inserted 
instructions, it also pops %rsp, consequently %rsp starts pointing 8 bytes above from where it was pointing when executing the retq instruction 
for getbuf. Hence that is the location where the address of touch2 must be located. (Note: after retq %rip is set to the address which %rsp is 
pointing to) 

&nbsp;
  
![exploit-string-in-hex](https://github.com/muratsankaya/AttackLab/assets/104160992/02361ca7-404b-4de8-a8eb-4011e80dfaa8)
ss 9.1 exploit-string-in-hex

&nbsp;
 
![visualization](https://github.com/muratsankaya/AttackLab/assets/104160992/05eae96a-28cf-4f8d-afb4-58f910d0f757)
ss 9.2 visualization
