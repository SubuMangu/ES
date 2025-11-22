# ES Quiz
## Ch 2
### Control and Status Signals in ARM7
1. **Processor mode signals**
- `nM[4:0]`: Identify the mode of the processor. The output bits are the inverses of the internal status  bits indicating processor operation mode(User,FIQ,IRQ,Supervisor,UNDEF,Abort,System).
2. **Clock Signals**
- `MCLK`: Master clock. In phase 1 MCLK is low and in phase 2 it is high.
- `nWAIT`: To wait for integer number of MCLK cycle by setting to low.
- `MCLK` and `nWAIT` are joined by AND operator. So `nWAIT` value can only be changed when `MCLK` is low.
3. **Memory interface signals**
-    