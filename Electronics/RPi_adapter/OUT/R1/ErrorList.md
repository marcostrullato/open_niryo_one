# Raspberry Pi Hat Revision 1

## Errors:

1. FAN3 signal and DIR signal (Dynamixel control signal) are the same signals. To solve this problem, you should cut FAN3 wire on Bottom layer and solder extra wire on Top layer (see pics). Fan 3 you can connect to Fan 2 or 3V3.
2. Q6..Q8 are IRLML2402. Q1..Q5 and Q9 are IRLML6402.