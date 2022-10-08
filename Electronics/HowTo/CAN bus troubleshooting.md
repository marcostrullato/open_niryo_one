# CAN bus troubleshooting

If you see "can bus scan failed" at the Nyrio One Studio log, you can use this manual to try to solve the problem.

This error you can get because of:

1. The bad wiring CANH and CANL between stepper pcb and Raspberry Pi hat.
2. Bad soldering of MCP2515 (most common error)
3. Bad soldering of SN65HVD230D
4. Bad soldering of MCU
5. MCP2515 or SN65HVD230D is dead
6. MCP2515 or SN65HVD230D haven't got a power supply

At first check wiring rules and connection between stepper PCB and Raspberry Pi hat.

## Wiring rules

1. CANH and CANL wires must be twisted.
2. CAN bus needs to be terminated with 120 Ohm resiter on two sides at muster and last slave. You can use JP1 on the stepper PCB for it. Just put a jumper on it. 

## CAN bus stepper PCB

To use this section, please open schematic drawing of your version stepper pcb. The most common problem is bad soldering of MCP2515. The QFN case is not easy for soldering.

0. Check all voltages on the board. Also check power supply on U6 and U7.
1. Connect USB to the stepper PCB.
2. Turn on the terminal and restart the stepper pcb (you can use terminal in Arduino IDE)
3. If you see a message about the bad initialization of MCP2515, you have problems with SPI. Soldering this pins U7 and U1. If the error persists, try to change MCP2515.
4. If you have the message "MCP2515 CAN started", you have problems with CAN side.
5. If you have an oscilloscope, you can scan CANH and CANL. Raspberry pi should scan CAN bus on start up. Also you can scan CAN_TX and CAN_RX and find the problem.
6. If you haven't got an oscilloscope, just solder very carefully U7 and U6. Also you can try to change U6.

## CAN bus Raspberry Pi

1. Test CAN initialization on the Raspberry Pi side. You can see it in the logs at Niryo One Studio. 
2. The recommendations are the same. If you have problems with initilization - soldering SPI pins, if not - soldering CAN pins.

## Soldering recommendations

1. QFN is small case and has small pins and pitch, so use small tip for your soldering iron and don't put too much solder.
2. This case has ground pad on the bottom. If you put too much solder on it and press down the chip, it can short sircit to outher pins. Use hot air to remove chip in this situation.
3. My first package of MCP2515 has oxide on pins. It's the sign of bad storage. Pins look like yellow. The soldering becomes very dificult. You should heat by soldering iron it to burn oxide. 
