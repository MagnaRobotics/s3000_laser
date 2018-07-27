# s3000_laser

<h2>Description</h2>
Driver for ROS to read the scan data from the device SICK S3000. This driver is based on the original driver developed for Player/Stage by Brian Gerkey, Kasper Stoy, Richard Vaughan, & Andrew Howard.

This driver controls the SICK S3000 safety laser scanner interpreting its data output. The driver is very basic and assumes the S3000 has already been configured to continuously output its measured data on the RS422 data lines.

<h2>Supported Hardware</h2>
This package should work with SICK S3000 safety laser scanners. 

![Image of Sick s3000](http://wiki.ros.org/s3000_laser?action=AttachFile&do=get&target=SICK_S3000.jpg)

<h2>Nodes
S3000 laser driver.

<h3>Published Topics:</h3>
s3000_laser/scan (sensor_msgs/LaserScan)
    Scan data from the laser, containing either range data or reflector detections. 

<h3>Parameters:</h3>
~port (string, default: "/dev/ttyUSB0")
    Serial port. 

~frame_id (string, default: "/laser")
    Relative frame for the publication of the laser measures. 

~baud_rate (int, default: 500000)
    Serial port baudrate (options: 9600, 19200, 38400, 115200, 500000). 

~serial_parity (string, default: "none")
    Serial port parity (options: "even", "odd", "none"). 

~serial_datasize (int, default: 8)
    Character size mask for serial link (options: 5,6,7,8). 

~range_min (float, default: 0m)
    Min valid range (m) for S3000 data produced. 

~range_max (float, default: 40m)
    Max valid range (m) for S3000 data. 

**Configuration for Magna Project**
Using serial to USB hubs with __continuous__ data output configured from 2x SICK S3000 lasers,
check and configure ports for each laser using

1. `ls -l /dev/ | grep USB` - should output `ttyUSBn` where `n` is port number
2. `sudo chmod o+rw /dev/ttyUSBn` : `n` is port number
3. `echo -ne '\033[2J' > /dev/ttyUSBn`
4. `echo -ne '\033[2J' > /dev/ttyUSBn` - this should echo data on the terminal. If you see some output, the port works.
5. Example command for laser scanner on right [node_name doesn't matter, but needs to be specified to avoid node naming conflict]

`roslaunch s3000_laser s3000_laser.launch port:=/dev/ttyUSB2 frame_id:=/s3000_link_right node_name:=s3000_right topic_name:=/scan_right`

**roslaunch for each laser scanner attached**

Now you should be able to visualize `LaserScan` topics in RViz when using the respective Magna launchfile from its respective package.
