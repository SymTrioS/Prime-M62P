# Prime-M62P

![Prime-M62P Board](https://github.com/SymTrioS/Prime-M62P/blob/main/Jpg/Prime-M62P.jpg)

### A Heterogeneous Computing Platform with Three Integrated Processing Units

---

## Overview

The Prime-M62P is a versatile development board that integrates three distinct computing architectures into a single platform:

- **User Interface Processor** - ARM Cortex-A7 based 1.2GHz Linux system for application development and I/O management
- **Hardware Processing Unit** - FPGA for custom logic and hardware acceleration
- **Microcontroller** - ARM Cortex-M4 for real-time analog and digital signal processing

The board includes integrated development tools accessible via a single USB Type-C connection, including a microcontroller programmer/debugger (CMSIS-DAP), dual-channel USB-to-UART converter, and FPGA configuration interface.

---

## Table of Contents

- [Architecture](#architecture)
- [Hardware Specifications](#hardware-specifications)
- [Getting Started](#getting-started)
- [Display Configuration](#display-configuration)
- [Development Resources](#development-resources)
- [Factory Configuration](#factory-configuration)

---

## Architecture

![Functional Diagram](https://github.com/SymTrioS/Prime-M62P/blob/main/Jpg/Prime-M62P_v24_Func.jpg)

### System Components

#### **Allwinner V3S Application Processor**
- ARM Cortex-A7 microprocessor, maximum core frequency up to 1.2G with 512Mbit DDR2 RAM;  
- Hardware H.264 video and JPEG/MJPEG decoding up to 1080p@30fps, H.264 encoder by 720p@60fps;  
- External Real time clock chip DS1307;  
- External FLASH: W25N02G (256MB);  
- External eMMC: KLM8G1G (8GB), switchable between FPGA and V3S;  
- Peripheral interfaces: Ethernet, USB-A Host, SDIO, UART, SPI, I²C, GPIO, audio, mic;  
- The board contains connectors for Ethernet, USB, uSD, 4pin audio jack and speaker connector;  
- Display support: LCD interface (up to 1024×768 resolution);  
- Camera interface: up to 5 MP sensor, MIPI-CSI2 interface;  
- The board contains connectors for connecting flat LCD display cables combined with a responsive touch screen (40 pin), as well as additional connectors for a capacitive touch screen (6pin) and a digital camera (20pin);  
- Operating system: Linux (Debian 12 supported).  

#### **Altera EP1C6T144 FPGA**
- Logic resources: 5,980 LES/flip-flops;  
- The amount of built-in configurable RAM is 92160 bits;  
- External eMMC: KLM8G1G (8GB), switchable between FPGA and V3S;  
- External SDRAM: 256 Mbit (16M × 16-bit);  
- External configuration flash memory: 16 Mbit P25Q16, connected to the system microcontroller;  
- Analog front-end: two 3MSP 10-bit ADC, one 8 channels 24-bit ADC and two 16-bit DAC converters;  
- Interfaces: SDIO, GPIO, user-configurable I/O; On the connectors, using GPIO pins that support LVDS, LVTTL, and PCI standards, it is possible to create arbitrary user interfaces specified by the FPGA configuration;  
- One connector, with a 2.54mm pitch, is made with a HUB-75 pinout for the possibility of connecting an LED panel;  
- Development environment: QuartusII-9.1sp2 IDE (`.rbf` bitstream format).  

#### **STM32F412RET Microcontroller**
- ARM Cortex-M4 core at 100 MHz;  
- Memory: 512 KB Flash, 256 KB SRAM;  
- External storage: 8 MB SPI Flash (W25Q64), 8 KB I²C EEPROM (M24C64);  
- Integrated analog peripherals: 7ch×12-bit ADC;  
- Communication interfaces: USB, UART, SPI, I²C, CAN, GPIO;  
- 7 pins of a 12-bit ADC and SPI/GPIO pins are output to the external connector, which can be used to connect the SPI display;  
- Transceivers with CAN and RS485 interfaces with contacts on the external connector are connected to the microcontroller, with the possibility of connecting a built-in 120 Ohm load resistor;  
- A two-channel receiver-transmitter of the RS232 interface with contacts on the external connector is connected to the microcontroller;  
- Debug interface: CMSIS-DAP via USB Type-C;  
- Display support: SPI/GPIO connector for external LCD SPI-displays;  
- Firmware format: Intel HEX (`.hex`)  

---

## Hardware Specifications

### V3S Microprocessor

| Parameter | Specification |
|-----------|---------------|
| Architecture | ARM Cortex-A7 |
| Clock Frequency | 1200 MHz |
| System Memory | 64 MB DDR2 |
| External FLASH | 256 MB |
| Video Decoder | H.264 up to 1080@30fps |
| Video Encoder | H.264 up to 720@60fps |
| Host Interface | USB-A |
| Ethernet | 10/100 Mbit |
| Serial Interfaces | UART, SPI, I²C |
| General I/O | GPIO |
| Display Support | LCD interface (up to 1024×768 pixels) |
| Camera Support | Up to 5 MP sensor |
| Operating System | Linux (Debian 12) |

### FPGA - Altera EP1C6T144

| Parameter | Specification |
|-----------|---------------|
| Logic Elements | 5,980 LES/flip-flops |
| Embedded Memory | 92160 bit RAM |
| External Memory | 256 Mbit SDRAM (16M×16-bit) |
| External Flash | 16 Mbit P25Q16 |
| Analog Inputs | One 8 channels 24-bit ADS1248 converters |
| Analog Inputs | Two 10-bit ADS7884 converters |
| Analog Outputs | Two 16-bit DAC8830 converters |
| Development Tool | QuartusII IDE |
| Bitstream Format | `.rbf` |

### STM32F412RET Microcontroller

| Parameter | Specification |
|-----------|---------------|
| Core | ARM Cortex-M4 |
| Clock Frequency | 100 MHz |
| Flash Memory | 512 KB |
| SRAM | 256 KB |
| External Flash | 8 MB W25Q64 (SPI) |
| External EEPROM | 8 KB M24C64 (I²C) |
| Integrated ADC | 7×12-bit channels|
| Communication | USB, UART, SPI, I²C, CAN |
| General I/O | GPIO, analog I/O |
| Debug Interface | CMSIS-DAP-S via USB Type-C |
| Display Connector | SPI/GPIO |
| Firmware Format | `.hex` |

---

## Getting Started

### Prerequisites

- USB Type-C cable
- Terminal emulator software (e.g., PuTTY, Tera Term, minicom)
- Serial port settings: 115200 baud, 8N1

### Quick Start

1. **Linux image**
   
   The pre-prepared Linux files and instructions for writing them to the uSD card are located in the **uSD** directory.  Instructions for building your own version of the Linux system image are located in the **Doc** directory.
   
2. **Connect the Board**
   
   Connect the Prime-M35P to your computer using the USB Type-C connector.

3. **Verify Device Enumeration**
   
   Three USB devices should appear in your system:
   
   - **CMSIS-DAP-S** - Microcontroller debugger interface
   - **COM Port 1** - V3S UART console
   - **COM Port 2** - STM32F412 UART debug output

   ![COM Port Detection](https://github.com/SymTrioS/Prime-M62P/blob/main/Jpg/Com_45.jpg)

4. **Access the Linux Console**
   
   Open COM Port 1 in your terminal emulator (115200 8N1). When a uSD drive containing a Linux image is inserted, you will see boot messages and a login prompt.
   
   **Default credentials (Debian 12):**
   - Username: `root`
   - Password: `root`
   
   For instructions on building a custom Linux image, refer to the documentation in the `Doc` folder.

5. **Monitor Microcontroller Output**
   
   Open COM Port 2 to view real-time diagnostic output from the STM32F373. The test firmware reports:
   - Cyclic DAC voltage values;
   - Data from the two FPGA-connected ADS7884 converters.

6. **Configuring the FPGA**

   The PrimeFPGA utility is used to configure the FPGA, as well as write configuration data to long-term memory. Available for download in Windows and Linux versions.
   
   - Windows x64 https://disk.yandex.ru/d/OT3DqWM2cuohtw
   - Linux x64   https://disk.yandex.ru/d/2vthDuQ_mu_6SQ
     
   *The work with the CMSIS-DAP-S microcontroller debugger and the PrimeFPGA utility should be separated in time. The board's system controller supports only one of the connections at a time.*

   ![PrimeFPGA_utility](https://github.com/SymTrioS/Prime-M62P/blob/main/Jpg/PrimeFPGA_123.jpg)
     
     -*When the "Connect" button is clicked and the connection to the board is successful, the board type and serial number is displayed, as well as the expected configuration file extension for this board. The button will change its function to "Disconnect".*
     
     -*Use the "file" button to select the desired configuration file. After it is successfully opened, the number of configuration bytes will be displayed.*
   
   ![PrimeFPGA_utility](https://github.com/SymTrioS/Prime-M62P/blob/main/Jpg/PrimeFPGA_456.jpg)
     
     -*When you click the "Config" button, the FPGA configuration process starts, the start is accompanied by the message "begin...". The successful end of the process is indicated by the message "Done!". During the configuration process, the green LED of the system controller is lit continuously. This configuration is not saved after the board is powered off.*
     
     -*You can reset the downloaded configuration by clicking the "RST" button,upon successful configuration reset, the message "RST OK" will appear, or by downloading a new configuration from another file.*

   ![PrimeFPGA_utility](https://github.com/SymTrioS/Prime-M62P/blob/main/Jpg/PrimeFPGA_789.jpg)
     
     -*By pressing the "Save" button, the configuration is saved in the flash memory of the board. The message at the beginning of the recording is "begin...", the successful completion is "Done!". The green LED of the system controller flashes during recording. When writing the configuration to the flash memory, the configuration of the FPGA chip itself does not occur. The recorded configuration will be loaded every time the board is restarted: on reset or power on.*
     
     -*You can delete the recorded configuration by clicking on the "DEL" button. However, the configuration will not be loaded when the board is restarted. The message on successful deleted is "DEL OK".*

     -*To start an application running Linux, run the command **sudo -u <user_name> ./PrimeFPGA.sh** from the program directory. If the warnings "dconf-WARNING..." appear in the terminal, they can be removed by installing: **sudo apt install dbus-x11***
     
### LED Indicators

During operation:
- **Red LED** - Indicates FPGA register access (external ADC121 readout)

---

## Development Resources

### Microcontroller Firmware

Test firmware is available for two development environments:

- **IAR Embedded Workbench:**
- **Visual Studio Code:**

![Development Environments](https://github.com/SymTrioS/Prime-M62P/blob/main/Jpg/Prime-M35P_IAR_VSC.jpg)

### FPGA Development

The FPGA test project for QuartusII IDE is available at:

[EP1C3T144](https://github.com/SymTrioS/EP1C6T144)

![QuartusII IDE](https://github.com/SymTrioS/Prime-M62P/blob/main/Jpg/QuartusII.jpg)

### Prebuilt Binaries

Ready-to-use binaries are located in the `Bin` directory:
- **STM32F412 firmware** - `.hex` format
- **FPGA configuration** - `.rbf` format

---

## Factory Configuration

The board ships with preloaded test firmware and FPGA configuration. All factory binaries are available in the `Bin` directory for restoration or reference.

---

## Display Configuration

### LCD Panel Connection

The board supports LCD panels with a 40-pin FFC (flat flex cable) connector. Compatible display sizes include:

- 4.3-inch displays (480×272 or 800×480 pixels)
- 5.0-inch displays (800×480 pixels)
- 7.0-inch displays (800×480 or 1024x768 pixels)

**Touchscreen support:**
- Resistive touchscreens: Supported
- Capacitive touchscreens: Supported

![LCD 40-Pin Connector](https://github.com/SymTrioS/Prime-M62P/blob/main/Jpg/M62_LCD_40pin.jpg)

## Additional Resources

- **Documentation:** See the `Doc` folder for detailed hardware specifications and Linux build instructions
- **Schematics:** Available in the repository for reference
- **Community Support:** Open an issue on GitHub for technical questions or bug reports

---

## License

Please refer to the repository license file for terms of use.

---

**Note:** This board is designed for development and prototyping. Ensure proper thermal management and power supply specifications are met for your specific application requirements.

