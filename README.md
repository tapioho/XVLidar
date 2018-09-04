# XVLidar
A python class for using the XV LiDAR controller from www.getsurreal.com in a more object oriented way.

* [XV Lidar Sensor Mount Package](https://www.getsurreal.com/product/xv-lidar-sensor-mount-package/)
* [Controller source code](https://github.com/getSurreal/XV_Lidar_Controller)
* [XV Lidar Controller Visual Test](https://www.getsurreal.com/xv-lidar-controller-first-release/xv-lidar-controller-visual-test/)

## Required software
Required software for running the LiDAR
* [Arduino IDE](https://www.arduino.cc/en/Main/Software) (Using `sudo apt-get install arduino` on Linux is **not** fully compatible with Teensyduino)
* [Teensyduino](https://www.pjrc.com/teensy/teensyduino.html) (Software add-on to run Arduino sketches on the Teensy (v1.27 – v1.28))
* [XVLidar.py](https://github.com/TUTvision/Monsterbog/blob/master/Lidar/XVLidar.py) (A python class for easier use, can be found in the current directory)
  * numpy (Python library for arrays)
  * pyserial (Python library used for communicating with the controller through USB serial port)
  
## Setting up the lidar
1. Download and install [Arduino IDE](https://www.arduino.cc/en/Main/Software)
2. Download and install [Teensyduino](https://www.pjrc.com/teensy/td_download.html) by following the instructions
3. Launch Arduino IDE and connect lidar to a USB port. The lidar should start spinning
4. From Arduino IDE, set the board to Teensy 2.0 and the serial port to the port that appears after you plugin the Teensy
5. Open Tools->Serial Monitor, at the bottom window of the Serial Monitor change the settings to “NewLine”
6. The Serial monitor should now be displaying lots of non-readable data. Send `MotorOff` from the serial monitor, the motor should stop spinning.

## Using the XVLidar-class
Start by importing the XVLidar class
```python
from XVLidar import *
```
Create a XVLidar instance. Here `port` is the name of the serial port that appeared after plugging in the Teensy. You could also pass a second argument `baud` to the class initializer, by defaul this is set to 115200.
```python
lidar = XVLidar(port)
```
Now you are ready to use all the class methods.
```python
lidar.reset()           # Resets the LiDAR configuration, this is executed when you create a new class instance
lidar.getPort()         # Returns name of the port the lidar is connected to
lidar.getData()         # Returns the current LiDAR data in a 360x2 numpy array
lidar.getSpeed()        # Returns the current rotation speed (RPM)
lidar.getErrors()       # Returns the number of data package errors so far
lidar.motorOn()         # Turns on the motor
lidar.motorOff()        # Turns off the motor
lidar.setRPM(speed)     # Sets the rotation speed (RPM), the value has to be between 180 and 349
lidar.sendCommand(cmd)  # Sends a command through the USB port to the LiDAR
lidar.scan(duration)    # Scan the surrounding for the given duration and save the data to the LiDAR data array
```
## Lidar data
The lidar data is stored in a 360x2 numpy array. The row index indicates the angle (degrees) of the measurement, while the row itself consists of two values: distance (mm) and quality (higher values for better quality)
