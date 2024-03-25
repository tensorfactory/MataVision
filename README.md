# MataVision (Your Affordable Edge-Based Vision Sensor)

## Overview
Supercharge your projects with MataVision, the low-cost, easy-to-use vision sensor that empowers your electronics with the power of sight.

Building on the simplicity of traditional sensors, MataVision leverages the processing capabilities of the ESP32-CAM or ESP32-S3 boards to bring computer vision to your projects at a fraction of the cost of complex camera systems.

Seamless integration, just like any other sensor, MataVision provides visual data for real-world applications.

Here's what MataVision can do for you:

Multiple Windows Image Classification: Train MataVision to recognize specific objects or scenes, ideal for applications like product sorting, state-change and anomaly detection.
Single Object Tracking: Track the movement of object in real-time, perfect for motion detection, or autonomous systems.
The possibilities are endless! Here are some inspiring applications for MataVision across various industries:

Smart Homes & Retail:
Detect an empty cat's bowl, parcels at the doorstep or door that is not fully closed.
Auto alert salesperson of customer presence or empty shelf.

Industrial Automation:
Automate quality control by training MataVision to detect mixings, wrong orientations or missing components in products.
Perform remote sensing in hazardous or cramped environments where direct-contact sensing is not possible.
Enhance safety with real-time monitoring of machinery activity.

Our user-friendly iOS app empowers you to:

Train your vision system with ease by collecting and categorizing images.
Define object boundaries using intuitive drawing tools for object detections.
Control your vision output through various methods like GPIO pins, I2C, Cloud or Display for maximum flexibility.
You can even incorporate data from other sensors via I2C or trigger a motor motion using the built-in IOs tools.

With MataVision, anyone can unlock the potential of computer vision in their projects.


#### (Supported boards)

<img title="ESP32-CAM Boards" src="images/esp32_cam_boards.png" alt="" width="150"><img title="ESP32-S3 Boards" src="images/esp32_s3_boards.png" alt="" width="155">

- ESP32-CAM (4MB flash, 520KB RAM, 4MB PSRAM).
- ESL32S3-DevkitC-1-N8R8 (8MB flash, 512KB RAM, 8MB PSRAM).


# Firmware Installation

Go to the [firmware folder](https://github.com/tensorfactory/MataVision/tree/main/firmwares) of the ESP board to download the required firmware package. It consists of 4 files (bootloader.bin, partition-table.bin, boot_app0.bin, and firmware.bin). Please choose the right setting according to the ESP chip type for the flashing process.


<br/>

| Filename               | Offset address (ESP32) | Offset address (ESP32S3) |
| ---------------------- |:----------------------:|:------------------------:|
| bootloader.bin         | 0x1000                 | 0x0000                   |
| partition-table.bin    | 0x8000                 | 0x8000                   |
| boot_app0.bin          | 0xe000                 | 0xe000                   |
| firmware.bin           | 0x10000                | 0x10000                  |

<br/>

|           | ESP32  | ESP32S3 |
| --------- |:------:|:-------:|
| SPI speed | 40MHz  | 80MHz   |
| SPI mode  | DIO    | DIO     |
| Baud rate | 460800 | 460800  |

<br/>

### (For Windows users)

We can use the "Flash Download Tools" from [Espressif](https://www.espressif.com/en/support/download/other-tools) to flash firmware bin files to an ESP device. Please follow the below steps for firmware flashing.

1. Connect an ESP device to a computer via USB. The connected port number can be found using Windows' "Device Manager". In this example, the ESP device was connected to COM3. (Note: For some ESP devices, we need to hold the BOOT button when powering ON to start in BOOT mode for firmware flashing.)
   
   <img title="Device Manager." src="images/device_manager.png" alt="" width="300">

2. Start the "Flash Download Tools". Select the correct ESP chip type and click OK.
   
   <img title="Flash Download Tools dialog one." src="images/fdt_esp32_1.png" alt="" width="200">

3. It is a good practice to erase the memory before flashing by clicking on the ERASE button. After erasing, input the paths of all the bin files and their respective offset positions.
   
   <img title="Flash Download Tools dialog two." src="images/fdt_esp32_2.png" alt="" width="300">
   
   Select the correct SPI speed and mode (Please ensure the "DoNotChgBin" option is NOT checked.). After selecting the correct COM port and BAUD rate, click the START button to start flashing.

### (For Mac or Linux users)

For Mac and Linux OS, we need to use a Python module called "Esptool" to flash bin files to an ESP device. We can run the "pip install esptool" command to install this module. After we have installed esptool, follow the below steps for firmware uploading. 

1. Connect an ESP device to a computer via USB (For Mac users, we can identify the connect com port by running "ls /dev/cu.*" ). (Note: For some ESP devices, we need to hold the BOOT button when powering ON to start in BOOT mode for firmware flashing.)

2. It is a good practice to erase the memory before flashing. This can be done by running the following Python script.
   
   `% esptool.py erase_flash`

3. After erasing, run the following Python scripts to flash the respective firmware files. The following scripts assume the connected com port is "/dev/cu.usbserial-21110" and all firmware bin files are placed in path "firmwares/ESP32_.../", please modify according to your setting.
   
   (for ESP32_CAM)

   `% esptool.py -p "/dev/cu.usbserial-21110" -b 460800 --before default_reset --after hard_reset --chip esp32 write_flash -z --flash_mode dio --flash_size 4MB --flash_freq 40m 0x1000 firmwares/ESP32_CAM/bootloader.bin 0x8000 firmwares/ESP32_CAM/partition-table.bin 0xe000 firmwares/ESP32_CAM/boot_app0.bin 0x10000 firmwares/ESP32_CAM/firmware.bin`
   
   <br/>
   
   (for ESP32S3_N8R8)
   
   `% esptool.py -p "/dev/cu.usbserial-21110" -b 460800 --before default_reset --after hard_reset --chip esp32s3 write_flash -z --flash_mode dio --flash_size 8MB --flash_freq 80m 0x0000 firmwares/ESP32S3_N8R8/bootloader.bin 0x8000 firmwares/ESP32S3_N8R8/partition-table.bin 0xe000 firmwares/ESP32S3_N8R8/boot_app0.bin 0x10000 firmwares/ESP32S3_N8R8/firmware.bin`

After successfully flashing firmware to the ESP device, hard reboot it. If everything goes accordingly, the status LED (GPIO 33 in ESP-CAM, and GPIO48/38 in ESP32S3-DevkitC-1-N8R8) will blink, indicating the MataVision is loading before turning to solid ON. If an error has occurred,  the status LED will stay OFF for ESP32 or turn to red for ESP32S3 devices. 

<br/><br/>

# iOS App



## Simple logic instructions(SLI)



## Components for MataVision
MataVision consists of two components. A firmware that enables ESP32 board to function as a vision sensor and an iOS app for managing images collections, 