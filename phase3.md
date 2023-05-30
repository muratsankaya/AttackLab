## Phase_3 Solution

The idea for phase_3 is very similar to phase_2. Just like in phase_2 the goal is to insert an instruction which this time instead of 
passing the value of cookie to %rdi the goal is to pass an address of a char pointer, which points to the beginning of the string 
representation of the cookie.  

&nbsp;

First step is to write the x86-64 set of instructions that passes the address of the char pointer to %rdi, and then decode it using the 
tools provided in the tar file.(see ss 10.1)

 &nbsp;

![insturction-d](https://github.com/muratsankaya/AttackLab/assets/104160992/c1c24f4b-06fa-449e-9e39-658334a66e60)
ss 10.1 decoded-x86-64-instruction

&nbsp;

As I stated the solution is very similar to the one in phase_2 . Again the code for the instruction can be in any place within stack space 
allocated for <getbuf>. This time I decided to put it at the bottom of the stack frame. Then when the program returns getbuf, %rip points to 
the set of inserted instructions(see ss 10.1). Following the retq via the dynamics of the stack frame and procedures the program calls touch3 
with the address of the string reps of the cookie as the single argument. 

 &nbsp; 
  
An important fact here is that the string must be located below the area which was used for <getbuf>. This is necessarily because touch3 will be 
called after <getbuf> is popped from the stack. Hence any function called within touch3 can overwrite the data stored there. One solution is to store 
the data at an address that is above the address of getbuf(addresses increase towards the bottom of the stack) before calling touch3. This 
could break the execution of the program, but it is not important here because touch3 exits the program. 

 &nbsp;

![exploit-string-in-hex](https://github.com/muratsankaya/AttackLab/assets/104160992/ce242765-81e1-4f05-9c24-f8a5fe077332)
ss 10.2 exploit-string-in-hex
