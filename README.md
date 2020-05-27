# CoolRoom
Monitoring room Project based on i-cube-lrwan (V1.1.5) stack (STM32L072/82 - OTAA LoRaWAN (EU868) - PIR sensor)

This project is the embedded part who allows you to monitor in real time a meeting room.

Hardware:
Based on grasshopper lorawan development board powered by STM32L082 ==> https://www.tindie.com/products/tleracorp/grasshopper-loralorawan-development-board/
**********************************************************************************************************************************
To be compatible with B-L072Z-LRWAN1 (Discovery-Board) you have to change line 115 & 116 to the right mapping of TCXO.

#define RADIO_TCXO_VCC_PORT                       GPIOA
#define RADIO_TCXO_VCC_PIN                        GPIO_PIN_12
**********************************************************************************************************************************

PIR sensor adafruit ==> https://www.adafruit.com/product/189

Software:
i-cube-lrwan V1.1.5 for Keil MDK-ARM

Follows this requirement to increase the code size limitation compilation from 32KB to 256 KB.
https://www2.keil.com/stmicroelectronics-stm32/mdk


Mapping grasshoper:

                                               -------------------
                                               |                 |
                    ----------                 |                 |
                    |        |     Digital out |                 |
                    |        |---------------->| D5(PB2)         |--> LoRaWAN RF (EU868)
                    |  PIR   |                 |                 |
                    | SENSOR |                 |                 |
                    |        |      3V3        |                 |
                    |        |<----------------|                 |
                    |        |      GND        |                 |
                    |        |<----------------|                 |
                    ----------                 |                 |
                                               |                 |
                                               |                 |         
                                    GND        |                 |
                                 ------------> |                 |
                                    VIN        |                 |
                                 ------------> |   Grasshopper   |
                                               |                 |
                                               -------------------

Go to \CoolRoom\i-cube-lrwan_V1.1.5\STM32CubeExpansion_LRWAN_V1.1.5\Projects\Multi\Applications\LoRa\End_Node\MDK-ARM\B-L072Z-LRWAN1\lora.uvprojx
 Add your Device EUI and lorawan connection key (api key & app key) in commissioning.h file.
 Compile your project.
 Plugin STM32L0 board and toggle the RESET button while holding down the BOOT button then from STM32CubeProgrammer load the hexfile.
 
 How does it works:
 
 The PIR scan continuously if someone is detected the PIR interrupt the grasshoper and the grasshopper send a lorawan frame, then falls asleep for at least 2 min, all interrupt are disabled after 2 min the interrupts are enabled again.
 
 
 It's compatible with the Things Networks (https://www.thethingsnetwork.org/) or Chirpstack server (https://www.chirpstack.io/) or others lorawan networks. 