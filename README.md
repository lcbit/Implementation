A temperature and humidity sensor for my 4B25 coursework in University of Cambridge. The program uses the BME680 sensor on the Warp platform to test the real-time temperature and humidity of the environment.


## Structure
The code running on the system is found in the folder Warp-firmware/src/boot/ksdk1.1.0 for testing the sensor. The Warp firmware is intended to be a demonstration and meassurement environment for testing the Warp hardware.

It provides facilities that we use to perform tests on the hardware such as changing the I2C and SPI bit rate, changing the Cortex-M0 clock frequency, and so on. Having a menu interface allows us to perform various experiments without having to re-compile and load firmware to the system for each experiment.

The Warp firmware is a tool for experiment. People can also use it as a baseline for building real applications by modifying it to remove the menu-driven functionality and linking in only the sensor drivers that needed.

The core of the firmware is in warp-kl03-ksdk1.1-boot.c. The drivers for the individual sensors are in devXXX.c for sensor XXX. In this case, devBME680.c for the BME680 gas sensor. The section below briefly describes all the source files in this directory and the changes I modified to achieve the project.

## Source File Descriptions
**CMakeLists.txt**
This is the CMake configuration file. Edit this to change the default size of the stack and heap.

**SEGGER_RTT.***
This is the implementation of the SEGGER Real-Time Terminal interface. Do not modify.

**SEGGER_RTT_Conf.h**
Configuration file for SEGGER Real-Time Terminal interface. You can increase the size of BUFFER_SIZE_UP to reduce text in the menu being trimmed.

**SEGGER_RTT_printf.c**
Implementation of the SEGGER Real-Time Terminal interface formatted I/O routines. Do not modify.

**devADXL362.***
Driver for Analog devices ADXL362.

**devAMG8834.***
Driver for AMG8834.

**devAS7262.***
Driver for AS7262.

**devAS7263.***
Driver for AS7263.

**devAS726x.h**
Header file with definitions used by both devAS7262.* and devAS7263.*.

**devBME680.***
Driver for BME680, this is the files used the most in the project.
To increase the accuracy of the measurement and make calibration. The bit precision is updated in the devBME680.c file to read msb, lsb and xlsb in hex ADC value. And then implent the Bosch codes (attached in the folder of Warp-firmware/src/boot/BME680_driver) to convert the BME680 ADC values into temperature, pressure and humidity values from calibration data. 

**devBMX055.***
Driver for BMX055.

**devCCS811.***
Driver for CCS811.

**devHDC1000.***
Driver forHDC1000 .

**devIS25WP128.***
Driver for IS25WP128.

**devISL23415.***
Driver for ISL23415.

**devL3GD20H.***
Driver for L3GD20H.

**devLPS25H.***
Driver for LPS25H.

**devMAG3110.***
Driver for MAG3110.

**devMMA8451Q.***
Driver for MMA8451Q.

**devPAN1326.***
Driver for PAN1326.

**devSI4705.***
Driver for SI4705.

**devSI7021.***
Driver for SI7021.

**devTCS34725.***
Driver for TCS34725.

**gpio_pins.c**
Definition of I/O pin configurations using the KSDK gpio_output_pin_user_config_t structure.

**gpio_pins.h**
Definition of I/O pin mappings and aliases for different I/O pins to symbolic names relevant to the Warp hardware design, via GPIO_MAKE_PIN().

**startup_MKL03Z4.S**
Initialization assembler.

**warp-kl03-ksdk1.1-boot.c**
The core of the implementation. This puts together the processor initialization with a menu interface that triggers the individual sensor drivers based on commands entered at the menu. You can modify warp-kl03-ksdk1.1-boot.c to achieve a custom firmware implementation using the following step:

Comment out the sensors that are not used during the project. That is comment out all the sensors except BME680.

You can inspect the baseline firmware to see what functions are called when you enter commands at the menu. You can then use the underlying functionality that is already implemented to implement your own custom tasks.

**warp-kl03-ksdk1.1-powermodes.c**
Implements functionality related to enabling the different low-power modes of the warp platform.

**warp.h**
Constant and data structure definitions. Here uses this to define data register addresses of BME680.
