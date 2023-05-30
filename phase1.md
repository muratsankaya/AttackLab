
##Phase_1 Solution


Solving phase 1 is quite simple and the information provided in the pdf is very instructive. The idea is to get the program execute <touch1> after completing the 
execution for <getbuf>. One creative solution for this is to overwrite the current return address of <getbuf> with the address of <touch1>. So the first step is to 
get the runtime address of the <touch1> (see ss 8.1) using gdb to break the execution during runtime.  


![address_of_touch1](https://github.com/muratsankaya/AttackLab/assets/104160992/843eac97-e0ca-4df5-89c5-d8ae62f8ad33)
ss 8.1 address-of-touch1


The key concept to understand here is the structure of the stack frame within procedure calls. In general, at the top of the procedure there is the argument build area 
in the case the current procedure calls another procedure which requires more than 6 arguments. Which in that case argument 7 till argument n are stored in the argument 
build area(argument 7 being on top of the frame). Below the argument build area there is space for local variables. In particular this space is used for local contiguous 
structures such as arrays and for local variables whose memory address is used somewhere in the program. Finally on the bottom of the frame there exists space for 
callee-saved registers. Right above the callee-saved registers lays the return address to the caller procedure. 

To truly understand the stack frame of the <getbuf> . We would have to see the disassembled version of <getbuf> (see ss 8.2). Luckily <getbuf> is a quite simple function, 
and the only data it stores in its stackframe is the char array of the size of BUFFER_SIZE , which in target24 it happened to be 24 bytes. 

![getbuf-d](https://github.com/muratsankaya/AttackLab/assets/104160992/90d3d181-3af6-45e9-ba4c-f66aa8968b7f)
ss 8.2 getbuf-in-x86-64

After the previous observations the solution is straightforward. The procedure <Gets> either reads a string  from the terminal or the input file, it returns an error message 
if the string is too short. However if the string is too large there is no-built-in way of checking for the error, and the part of the string that is more than 24 bytes 
continues to get copied to the stackframe. Now recall that right before the beginning of the callee-procedure lays the return address. Hence our goal is to change the current 
return address to the address of touch1. 

![image](https://github.com/muratsankaya/AttackLab/assets/104160992/3a094abf-93b8-466f-9ed2-7d668baabb12)
ss 8.3 exploit-string-in-hex

![visualization](https://github.com/muratsankaya/AttackLab/assets/104160992/4789d8a3-01e7-4983-b7b7-84fac3487836)
ss 8.4 visualization
