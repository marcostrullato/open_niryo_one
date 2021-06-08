# HowTo setup steppers torque

To change the stepper torque change max_effort.

sudo nano ~/catkin_ws/src/niryo_one_bringup/config/v2/stepper_params.yaml

Restart Niryo after changing. Be careful: max_effort increasing makes stepper driver hotter. Use fan and radiator for cooling down.
