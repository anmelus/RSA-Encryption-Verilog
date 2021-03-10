# RSA-Encryption-Verilog

This verilog project models the RSA style of encryption that is widely used for data transmission. Most websites such as Amazon utilize RSA encryption to hide private and public keys from adversaries. 

"In a public-key cryptosystem, the encryption key is public and distinct from the decryption key, which is kept secret (private). An RSA user creates and publishes a public key based on two large prime numbers, along with an auxiliary value. The prime numbers are kept secret. Messages can be encrypted by anyone, via the public key, but can only be decoded by someone who knows the prime numbers." -Rivest, R.; Shamir, A.; Adleman, L. (February 1978). "A Method for Obtaining Digital Signatures and Public-Key Cryptosystems". Communications of the ACM. 21 (2): 120–126. CiteSeerX 10.1.1.607.2677. doi:10.1145/359340.359342. S2CID 2873616

Example RSA encryption/decryption is:
given message M = 9
encryption: ( private key 3 and public key 33 )
C = 93 mod 33 = 3
decryption: ( private key 7 and public key 33 ) M = 37 mod 33 = 9

# Technical description of each state

The high level module has 5 inputs 
Input private_key = 16 bits\
Input public_key = 16 bits.\
Input message_val = 16 bits.\
Input clk - 1 bit\
Input Start - 1  bit\
Input Rst - 1 bit\
Output Cal_done 1 bit\
Output Cal_val 16 bit
 
At reset the FSM resets all the registers.
Capture_state: When Start goes high, the Cal_val  and  Cal_done  are  set  to  zero.  The two keys and the message needs to copied to internal registers. No need to check Start signal in the following states.
Exponent_state1: feed the values to the myexp module from hw7 and raise the load signal. 
Exponenet_state2: set the load signal to low
Exponent_state3:	monitor myexp done signal, if done is low jump back to state2 and keep bouncing between state 2 and 3
Add more/less states as  needed.
Mod_state1: feed the values to the mymodfunc module and raise the load signal. 
Mod_state2: set the load signal to low
Mod_state3: monitor myexpo done signal, if done is low jump back to state2 and keep bouncing between state 2 and 3
Cal_done_state: set Cal_done to high and the output appears on Cal_val. Go back to Capture_state.

#### The testbench only models encryption, decryption is simply accomplished by changing which of the provided numbers are passed into each function.

# Synthesis Results:

![alt text](https://github.com/anmelus/RSA-Encryption-Verilog/blob/main/quartus1.png)

![alt_text](https://github.com/anmelus/RSA-Encryption-Verilog/blob/main/quartus2.png)

Logic Elements: 385
Registers: 325
Tri-State Buffers: 0
Memory bits: 0

# RELEVANT WARNINGS UPON SYNTHESIS
Warning (18236): Number of processors has not been specified which may cause overloading on shared machines.  Set the global assignment NUM_PARALLEL_PROCESSORS in your QSF to an appropriate value for best performance.
Warning (10463): Verilog HDL Declaration warning at proj2.v(17): "final" is SystemVerilog-2005 keyword
Warning (10230): Verilog HDL assignment warning at myexp.v(82): truncated value with size 32 to match size of target (16)
Warning (10230): Verilog HDL assignment warning at myexp.v(92): truncated value with size 32 to match size of target (16)  **not entirely sure why these warnings occur but it did not affect my synthesis overall, use caution when using very large values**
Warning (15714): Some pins have incomplete I/O assignments. Refer to the I/O Assignment Warnings report for details
Critical Warning (169085): No exact pin location assignment(s) for 68 pins of 68 total pins. For the list of pins please refer to the I/O Assignment Warnings table in the fitter report.             **No pin assignments were set**
Critical Warning (332148): Timing requirements not met     **No timing requirements were set**
