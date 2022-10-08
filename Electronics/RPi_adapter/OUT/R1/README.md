# Raspberry Pi Hat Revision 1

## Errors:

1. FAN3 signal and DIR signal (Dynamixel control signal) are the same signals. To solve this problem, you should cut FAN3 wire on Bottom layer and solder extra wire on Top layer (see pics). Fan 3 you can connect to Fan 2 or 3V3.
![alt text](https://github.com/marcostrullato/open_niryo_one/blob/develop/Electronics/RPi_adapter/OUT/R1/Error1_cut%20wire.png)
![alt text](https://github.com/marcostrullato/open_niryo_one/blob/develop/Electronics/RPi_adapter/OUT/R1/Error1_extra%20wire.png)
2. Q6..Q8 are IRLML2402. Q1..Q5 and Q9 are IRLML6402.