# ALU
An Arithmetic Logic Unit (ALU) designed with a combination on structural and behavioral programming styles.

Takes two 16-bit inputs, performs an operation, and returns a 32-bit ouput.

The ALU is capable of performing the operations below. For more information, please refer to docs/High-Voltage.Split.PART4.Operations.


Command	  Opcode	  Description
No-Op	      0000	  No action on this clock tick
Add	        0001	  Add A and B, store in output register
Subtract	  0010	  Subtract A and B, store in output register
Multiply	  0011	  Multiply A and B, store in output register
Divide	    0100	  Dive A and B, store in output register. 
Modulus	    0101	  Modulo A and B, store in output register
AND	        0110	  Add A and B, store in output register
OR	        0111	  Add A and B, store in output register
NOT	        1000	  NOT A, store in output register
XOR	        1001	  Add A and B, store in output register
NAND	      1010	  Add A and B, store in output register
NOR	        1011	  Add A and B, store in output register
XNOR	      1100	  Add A and B, store in output register
Output->A	  1101	  Set A register to previous output value to perform operations with the output
Reset	      1110	  Reset all registers to 0


Sample run. For more runs, please refer to docs/High-Voltage.Split.PART4.output.

A = 5
B = 0

++================++================++======++===++===++===================================++
||       A        ||       B        ||Opcode||Clk||Err||                Output             ||
  0000000000000101  0000000000000000  0000    0    0    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
  0000000000000101  0000000000000000  0000    1    0    0000000000000000xxxxxxxxxxxxxxxx
  0000000000000101  0000000000000000  0000    0    0    0000000000000000xxxxxxxxxxxxxxxx
  0000000000000101  0000000000000000  0001    1    0    00000000000000000000000000000101
  0000000000000101  0000000000000000  0001    0    0    00000000000000000000000000000101
  0000000000000101  0000000000000000  0010    1    0    00000000000000000000000000000101
  0000000000000101  0000000000000000  0010    0    0    00000000000000000000000000000101
++================++================++======++===++===++===================================++
  0000000000000101  0000000000000000  0011    1    0    00000000000000000000000000000000
++================++================++======++===++===++===================================++
  0000000000000101  0000000000000000  0011    0    0    00000000000000000000000000000000
  0000000000000101  0000000000000000  0100    1    1    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
  0000000000000000  0000000000000000  1110    0    0    00000000000000000000000000000000
  0000000000000000  0000000000000000  0101    1    0    0000000000000000xxxxxxxxxxxxxxxx
  0000000000000000  0000000000000000  0101    0    0    0000000000000000xxxxxxxxxxxxxxxx
  0000000000000000  0000000000000000  0110    1    0    00000000000000000000000000000000
  0000000000000000  0000000000000000  0110    0    0    00000000000000000000000000000000
++================++================++======++===++===++===================================++
  
Comments: No-op functions as intended. Error occurs when dividing by 0 and system subsequently is reset.
			Similar situation will occur when overflow occurs in the ADDER_SUBTRACTOR.
