
## Phase_4 Solution

Injecting code with exploit strings will not work in this case because the program uses the stack randomization principle. Meaning the location 
of the stack frame will be different at each run and even if the location was guessed the program will not allow executing code from the stack frame. 

&nbsp;

Therefore, the challenge of this phase is to build a return-oriented programming attack(ROP). The key of this strategy is to use gadgets.  Gadgets 
are defined in the pdf as bytes sequence within an existing program that consists of one or more instructions followed by ret. 

&nbsp;

The goal is to redo the attack in phase_2 using ROP. Let's recall that the goal of phase_2 was to call touch2 with the value of our cookie in %rdi. 

&nbsp;

In general the idea of the solution is as follows; when the program returns from getbuf it will jump to the code to execute popq %rax.
So the value at %rsp is pointing when executing popq %rax must be the value of the cookie. With the following ret instruction the program should 
jump to the code to move %rax to %rdi, and with the following ret it should go to touch2. 

 &nbsp; 
  
The easiest solution would be to pop the cookie directly to the %rdi and return to touch2. However, in my program there was no gadget that had 
the hex encoding 5f, actually the only hex encoding among ss 11.1 that existed in my program was 58. Therefore the only solution that I was able 
to come up with was to pop the cookie to %rax and then mov %rax to %rdi and return to touch2. Please see ss 11.2 for the solution and ss 11.3 for 
the visualization of the idea. 
 
 &nbsp;

![popq-hex-encoding](https://github.com/muratsankaya/AttackLab/assets/104160992/8c8c07bd-ee61-4dad-9188-3647c5b97087)
ss 11.1 popq-hex-encoding

 &nbsp;

![exploit-string-encoded-in-hex](https://github.com/muratsankaya/AttackLab/assets/104160992/1b7f7dd2-836b-4d28-b0d2-ce02edc324da)
ss 11.2 exploit-string-encoded-in-hex

 &nbsp; 
  
![visualization](https://github.com/muratsankaya/AttackLab/assets/104160992/61742a7a-2bf2-47fa-b3aa-67a08e1feae3)
ss 11.3 visualization
