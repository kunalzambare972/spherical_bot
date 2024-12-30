# Spherical-Bot

This repository encapsulates the information related to board dimensions, electronics, sensors and its testing scripts, to controller node for the development of Spherical Bot 2.0

For the software parts and test scrpits of the robot go to this repo : (https://github.com/kunalzambare972/sp-bot) 

## Table of Contents

* [Hardware](https://github.com/kunalzambare972/spherical_bot?tab=readme-ov-file#hardware)
  * [Hardware BOM](#hardware-bom)
  * [PCB Layout and BOM](#pcb-bom-and-layout)
* [Software](https://github.com/kunalzambare972/spherical_bot?tab=readme-ov-file#software)
  * [SD Card Setup](#sd-card-setup)
  * [Software Pre-Requisites](#software-pre-requisites)
  * [IMU Test](#imu-test)
  * [Picamera Test](#picamera-test)
  * [Robot Motion](#robot-motion)
* [Control](https://github.com/kunalzambare972/spherical_bot?tab=readme-ov-file#control)
  * [Pitch Control](#pitch-control)
  * [Roll Control](#roll-control)


# Hardware

## Hardware BOM


<p align="center"> <img src="https://github.com/kunalzambare972/spherical_bot/blob/main/imgs/hardware.PNG" height="700" width="500" alt="Final BOM">
<br/>


## PCB BOM and Layout

<p align="center"> <img src="https://github.com/kunalzambare972/spherical_bot/blob/main/imgs/bom_final.PNG" height="700" width="500" alt="Final BOM">
<br/>

<p align="center"><img src="https://github.com/kunalzambare972/spherical_bot/blob/main/imgs/pcb_measure.PNG" height="300" width="300" alt="PCB Dim" hspace="10">
<img src="https://github.com/kunalzambare972/spherical_bot/blob/main/imgs/pcb_dim.PNG" height="300" width="300" alt="Overall Board Dimension">


# Software

## SD Card Setup

1. Download the Raspberry Pi Imager<br/>
   The easiest way to set up an SD card is by using the Raspberry Pi Imager tool.<br/>
   i. Download Raspberry Pi Imager from the official website of raspberry pi.<br/>

2. Install Raspberry Pi Imager<br/>
   i. Windows/Mac: Run the downloaded installer and follow the on-screen instructions.<br/>
   ii. Linux: For Ubuntu, you can install it directly via the terminal:
   ```
   sudo apt install rpi-imager
   ```
3. Insert the microSD card into your computer using an SD card reader.
4. Launch Raspberry Pi Imager on your computer.
5. Choose the Device in Raspberry Pi Imager which is Raspberry Pi Zero W.
6. Choose the OS in Raspberry Pi Imager which is Raspberry Pi OS (32-bit)- Debian Bookworm.
7. Select the SD Card which is connected to the computer.
8. Advanced Settings<br/>
   i. Access the advanced settings by pressing "Ctrl+Shift+X" before clicking Next/Write.<br/>
   ii. Set the hostname to "spbot"<br/>
   iii. Set the username as "sp"<br/>
   iv. Set your password.<br/>
   v. Configure Wi-Fi: Enter your Wi-Fi SSID and password if you want the Raspberry Pi to connect to Wi-Fi automatically.<br/>
   vi. Enable the SSH in the Services Section of Advanced Settings.<br/>
   vii. Click on "SAVE"<br/>
9. Click on Next/ Write and the Raspberry Pi Imager would start to write the SD card with Raspberry Pi OS(Linux).
10. Once done eject the SD card from the computer and insert it into Raspberry Pi for first boot.
    
## Software Pre-Requisites
1. Firstly we need to clone the software package github repositroy in raspberry pi file system. Since the repository is private you need to generate a personal access token for cloning the directory<br/>
  To generate a personal access token :-<br/>
   i. Go to your Github Settings >> Developer Settings >> Personal Access Tokens.<br/>
   ii. Then click on classic token.<br/>
   iii. Select the expiration date and select "repo" in scopes for full control of private repository.<br/>
   iv. Generate and copy the token. Keep it safe; you won't be able to see it again.<br/>

2. Now open the terminal and run for cloning "sp-bot" repository
```
git clone https://github.com/kunalzambare972/sp-bot.git

```
3. Authenticate<br/>
   i.When prompted for a username, enter your GitHub username.<br/>
   ii.When prompted for a username, enter your GitHub username.<br/>
   
4. Enter the Repository Directory

```
cd sp-bot
```
   
5. Install all the required dependencies  
```
pip install -r requirements.txt
```
The above the steps would help to setup the raspberrypi sdcard for the operation of the robot

## IMU Test

The IMU being used is the Bosch 9-axis BNO-055 sensor -

<p align="center"> <img src="https://github.com/kunalzambare972/spherical_bot/blob/main/imgs/bno_055_imu.jpg" height="300" width="300" alt="BNO_IMU">
<br/>

### Plotting the IMU result

<p align="center"><img src="https://github.com/kunalzambare972/spherical_bot/blob/main/imgs/imu.PNG" height="300" width="300" alt="IMU_command_line" hspace="10">
<img src="https://github.com/kunalzambare972/spherical_bot/blob/main/imgs/rpy_updated.PNG" height="300" width="300" alt="IMU_graph">

### To test the IMU output - 

1. Navigate to sensor_tests/IMU_test directory
2. Run the test_imu.py


## PiCamera Test

The Camera being used is the Waveshare OV5647 220FOV Camera -

<p align="center"> <img src="https://github.com/kunalzambare972/spherical_bot/blob/main/imgs/ov5647.PNG" height="300" width="300" alt="Camera">
<br/>

### Viewing the output from camera

<p align="center"><img src="https://github.com/kunalzambare972/spherical_bot/blob/main/imgs/picamera_view.jpg" height="300" width="300" alt="Cam_output" hspace="10">
<img src="https://github.com/kunalzambare972/spherical_bot/blob/main/imgs/picam_opencv.PNG" height="300" width="300" alt="Cam_fw_output">

### To test the camera output - 

1. Note down the system's IP
2. Navigate to sensor_tests/Picamera_test directory
3. Run the test_picam2.py
4. Head to the following page - http://{IP_ADDRESS or spbot.local}:5000/


## Robot Motion

The following clip shows the robot's forward movement - 

https://github.com/user-attachments/assets/1400048a-bb1d-4988-941b-7a97be991c32


The following clip shows the robot's forward and backward movement -

https://github.com/user-attachments/assets/daac4932-1899-4ce8-a5ff-24615ddae6e5


# Control

## Pitch Control

### Methodology
The pitch control of the Spherical Bot is managed using a Proportional Controller (P-controller). The control strategy is implemented to maintain the desired pitch angle by adjusting the servo motors and the N20 motor based on the feedback from the IMU sensor.

### Mathematical formula

The Proportional Control equation used is:

$$
P_{\text{error}} = \text{Set Point} - \text{Actual Pitch}
$$

$$
\text{Motor PWM} = \max(0, \min(200, K_p \times P_{\text{error}} + \text{User PWM}))
$$

where:
-  $$  P_{\text{error}} $$ is the pitch error.
- $$ \( \text{Set Point} \) is the desired pitch angle (set to 0.0 degrees in this case).
- $$ \( \text{Actual Pitch} \) is the current pitch angle measured by the IMU sensor.
- $$ \( K_p \) is the proportional gain, set to 0.00876.
- $$ \( \text{User PWM} \) is a constant value (set to 185) that provides the base motor speed.
- $$ \( \text{Motor PWM} \) is the calculated PWM value to drive the motor, ensuring it stays within the range of 0 to 200

### Optimal Value

The optimal proportional gain $$ (\( K_p \)) for achieving stable pitch control is determined to be:

$$ \[ K_p = 0.00876 \]


## Roll Control

### Methodology

The roll control of the Spherical Bot is managed using a Proportional Controller (P-controller). The control strategy is implemented to maintain the desired roll angle by adjusting the servo motors and the N20 motor based on feedback from the IMU sensor.

### Mathematical Formula

The Proportional Control equation used is:

$$
P_{\text{error}} = \text{Set Point} - \text{Actual Roll}
$$

$$
\text{Motor PWM} = \max(0, \min(200, K_p \times R_{\text{error}} + \text{User PWM}))
$$

where:
- \( R_{\text{error}} \) is the roll error.
- \( \text{Set Point} \) is the desired roll angle (set to 0.0 degrees in this case).
- \( \text{Actual Roll} \) is the current roll angle measured by the IMU sensor.
- \( K_p \) is the proportional gain, set to 0.00876.
- \( \text{User PWM} \) is a constant value (set to 185) that provides the base motor speed.
- \( \text{Motor PWM} \) is the calculated PWM value to drive the motor, ensuring it stays within the range of 0 to 200.

### Optimal Value

The optimal proportional gain (\( K_p \)) for achieving stable roll control is determined to be:

\[ K_p = 0.00876 \]

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
