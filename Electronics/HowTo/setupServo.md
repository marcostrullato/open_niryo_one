# HowTo setup servo drivers

### Source

Command list: https://github.com/NiryoRobotics/niryo_one_ros/tree/eb413c7a967c94d3e6eb20513fab2c4acd200c69/niryo_one_debug

### Instruction

DXL have baudrate = 57600 bps and ID = 1, as default value. All Nyrio drivers has difrent ID and 1M bps, as baundrate. So you need to connect servo drivers one by one setup it.

ID list:

- 2 # -> id of Axis 4
- 3 # -> id of Axis 5
- 6 # -> id of Axis 6
- 11 # id of Gripper 1
- 12 # id of Gripper 2
- 13 # id of Gripper 3
- 31 # if of Vacuum Pump 1


At first stop Nyrio ROS stack and scan dxl serial line by debug instruments. 

sudo systemctl stop niryo_one_ros.service
cd ~/catkin_ws/devel/lib/niryo_one_debug
./dxl_debug_tools --scan --baudrate 57600

The answer is:

Using baudrate: 57600, port: /dev/serial0
Dxl ID: 0
Dxl Bus successfully setup
--> SCAN Dxl bus
Detected Dxl motor IDs:
- 1

Setup the new baudnrate:

./dxl_debug_tools --baudrate 57600 --id 1 --set-register 8 3 1

Setup the new id of servo motor (id=3):

./dxl_debug_tools --id 1 --set-register 7 3 1

### Errors code

niryo_one_ros/dynamixel_sdk/include/dynamixel_sdk/packet_handler.h

#define COMM_SUCCESS        0       // tx or rx packet communication success
#define COMM_PORT_BUSY      -1000   // Port is busy (in use)
#define COMM_TX_FAIL        -1001   // Failed transmit instruction packet
#define COMM_RX_FAIL        -1002   // Failed get status packet
#define COMM_TX_ERROR       -2000   // Incorrect instruction packet
#define COMM_RX_WAITING     -3000   // Now recieving status packet
#define COMM_RX_TIMEOUT     -3001   // There is no status packet
#define COMM_RX_CORRUPT     -3002   // Inco

### Trouble shooting 

1. Check all wires connections
2. The XL430 will blink red at start. It can blink without GND.
3. Check voltage supplay.
