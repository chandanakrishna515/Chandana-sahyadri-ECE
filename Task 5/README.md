# Moisture Sensor using VSDSquadron Mini

## Project Overview
This project demonstrates the implementation of a moisture sensor using the *VSDSquadron Mini*, a RISC-V based development kit. The core concept revolves around detecting moisture presence using a sensor. When moisture is detected, an LED indicator is activated to signal the presence of moisture; otherwise, it remains off.

The project leverages the capabilities of the *VSDSquadron Mini*, including its GPIO ports and on-board programming features, to create an efficient and compact moisture detection system.

## Components Required
- *VSDSquadron Mini*
- *LED*
- *Moisture Sensor*
- *Breadboard*
- *Jumper Wires*
- *PlatformIO*
- *VS Code* for software development

## Circuit Connection
### 1. LED Connection
- Positive end (*Anode) connected to **GPIO Pin 6* of VSDSquadron Mini.
- Negative end (*Cathode) connected to **GND*.

### 2. Moisture Sensor Connection
- *VCC* of the moisture sensor to *VCC* on VSDSquadron Mini.
- *Output* to *GPIO Pin 0* of VSDSquadron Mini.
- *GND* to ground.

## How to Program
### 1. Environment Setup
- Install *PlatformIO* and *VS Code* for software development and project compilation.

### 2. Include Necessary Libraries
c
#include <ch32v00x.h>
#include <debug.h>


### 3. GPIO Configuration
c
void GPIO_Config(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0};
    
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE);
    
    // Configure LED pin as output
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_6;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOD, &GPIO_InitStructure);
}


### 4. Main Logic
c
int main(void) {
    uint8_t moistureStatus = 0;
    
    // System initialization
    GPIO_Config();
    
    while(1) {
        moistureStatus = GPIO_ReadInputDataBit(GPIOD, GPIO_Pin_0);
        
        if(moistureStatus == 0) {
            GPIO_WriteBit(GPIOD, GPIO_Pin_6, RESET); // LED OFF
        } else {
            GPIO_WriteBit(GPIOD, GPIO_Pin_6, SET); // LED ON
        }
    }
}


## How It Works
The moisture sensor continuously monitors the soil or a surface for moisture. Upon detecting moisture, the sensor’s *output pin goes high, and the connected **GPIO pin on the VSDSquadron Mini* reads this change. Depending on the moisture status, the system either *activates or deactivates an LED*, providing a visual indication of the detected moisture level.

## Repository Structure

├── src
│   ├── main.c  # Main program file
│   ├── config.h  # GPIO configurations
│
├── platformio.ini  # PlatformIO project settings
│
├── README.md  # Project documentation


## Troubleshooting
1. *LED does not turn ON:*
   - Ensure correct wiring and check GPIO assignments.
   - Verify moisture sensor output using a multimeter.

2. *Compilation Errors:*
   - Check if *PlatformIO* is correctly installed.
   - Ensure required libraries are included.

3. *Unexpected behavior:*
   - Check the power supply to the moisture sensor.
   - Use debug prints to verify GPIO readings.

