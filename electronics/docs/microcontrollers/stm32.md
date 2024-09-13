# STM32

STM32 are a family of microcontrollers manufactured by ST Miroelectronics. They use the ARM Cortex-M processor cores. ARM Cortex-M is a group of 32-bit RISC processor cores which are optimized for low energy applications, which is perfect for microcontrollers. Here the 32-bit means that the hardware inside (like adders, multipliers, etc) are designed to work with 32-bit data. That is all for the jargon 🤓.

These microcontrollers are made for performance. STM32 are more powerful than your regular arduino boards in terms of clock speed (32MHz-168MHz), memory capacity and more peripherals.

*NOTE: Arduino R4 series ownwards also use 32-bit ARM processors have comparable processing power compared to STM32. However they can be much more expensive.*

But to work with STM32 microcontrollers, you need to go deeper into how they work on the inside unlike an arduino.

# STM32 Development Boards

Some of the STM32 development boards that we have available in the ERC inventory are listed below.

## Blue Pill (STM32F103C8T6)

This boards is not an official development board sold by STMicro, they are manufactured by chinese companies. Sometimes you may get a counterfeit chip (CKS32F103C8T6 or something).

From personal experience, I do not recommend using this board if its your first time. It has given me a lot of headache. I would recommend one of the other two in the list.

![](/electronics/images/stm32-blue-pill.jpg)

- Max Speed: 72MHz
- RAM: 20kb
- Flash Memory: 64kb (as reported by datasheet)
- Peripherals: 3xUART, 2xSPI, 2xI2C, 12-bit 10 channel ADC,  USB-CDC and HID Capable through microUSB
- IO: Upto 37 GPIO Pins
- Programming Interface: None (Needs STLink V2 programmer or USB to UART converter)

**Original STLink**

Reliable, identified by STMicro Softwares, Relatively Expensive

![](/electronics/images/stlink-original.webp)

**Chinese Knockoff STLink**

Cheap and somewhat reliable, may not be identified by STMicro Software

![](/electronics/images/stlink-chinese.webp)

**USB to UART Converter**

Commonly used to program multiple microcontrollers

![](/electronics/images/usb-uart.webp)

- Max Speed: 72MHz
- RAM: 20kB
- Flash Memory: 64kB (as reported by datasheet)
- Peripherals: 3xUART, 2xSPI, 2xI2C, 12-bit 10 channel ADC,  USB-CDC and HID Capable through microUSB
- IO: Upto 37 GPIO Pins
- Programming Interface: None (Needs STLink V2 programmer or USB to UART converter)

## Nucleo-F401RE (STM32F401RET6)

An official board manufactured by STMicro. Comes with an STLink programmer on the board. The one we have is the Nucleo-401RE but there are other Nucleo boards with different microcontrollers. Also has an Arduino UNO R3 shield on the board.

![](/electronics/images/nucle-401re.webp)

- Max Speed: 84MHz
- RAM: 96kB
- Flash Memory: 512kB (as reported by datasheet)
- Peripherals: 4xUART, 3xSPI, 3xI2C, 12-bit 16 channel ADC,  USB-CDC and HID Capable through microUSB
- IO: Upto 50 GPIO Pins
- Programming Interface: On-board STLink

## STM32F4-Discovery (STM32F407VG)

An official development boards manufactured by STMicro. Also comes with an onboard STLink programmer.

![](/electronics/images/stm32f4_disco.webp)

- Max Speed: 168MHz
- RAM: 192kB
- Flash Memory: 1MB (as reported by datasheet)
- Peripherals: 4xUSART/2xUART, 3xSPI, 3xI2C, 12-bit 24 channel ADC, 12-bit DAC,  USB-CDC and HID Capable through microUSB
- IO: Upto 82 GPIO Pins
- Programming Interface: On-board STLink Programmer

*There may be some incorrect specs and there definitely are more specs than mentioned but I am too lazy to check.*

# Table of Contents
1. [Programming an STM32](#programming-stm32)

# <a name="programming-stm32" id="programming-stm32"></a>Programming an STM32

You may think what is there in programming a microcontroller. Just download a software, write code and press upload. That is true when you use Arduino. After reading this, you will realize some of the powers the Arduino platform gives you.

The methods listed below are ordered from easiest to hardest.

## Using the Arduino IDE (STM32duino)

Yes you read that right, you can program many microcontrollers other than arduino using the Arduino IDE, including the STM32.

1. Install the following software for your OS
- [Arduino IDE 2](https://www.arduino.cc/en/software)
- [STM32CubeProgrammer](https://www.st.com/en/development-tools/stm32cubeprog.html) (Only if you are not flashing the bootloader)
2. In the Arduino IDE, open 'Preferences' and add the following link to the 'Additional Boards Manager URL'

`https://github.com/stm32duino/BoardManagerFiles/raw/main/package_stmicroelectronics_index.json`

3. Open board manager, search for **STM32 MCU based boards by STMicroelectronics** and install it. If it is already installed then its fine.
4. Now to test if the installation worked. Write the following sketch

```cpp
#define LED_BUILTIN PC13 // PC13 is the led pin on the blue pill, change according to board

// the setup function runs once when you press reset or power the board
void setup() {
  // initialize digital pin LED_BUILTIN as an output.
  pinMode(LED_BUITLIN, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {
  digitalWrite(LED_BUILTIN, HIGH);  // turn the LED on (HIGH is the voltage level)
  delay(1000);                      // wait for a second
  digitalWrite(LED_BUILTIN, LOW);   // turn the LED off by making the voltage LOW
  delay(1000);                      // wait for a second
}
```
In the 'Tools' menu, select the board as per the type of chip on the development board (STM32F1 in this case, STM32F4 for nucleo and discovery) and the upload method as STM32CubeProgrammer (SWD).

Connect the STLink V2 to the BluePill as follows:

| Blue Pill Pin | STLink Pin |
| ------------- | ---------- |
| 3V3           | 3.3V       |
| GND           | GND        |
| SWDIO/DIO     | SWDIO      |
| SWDCLK/CLK    | SWCLK      |

*These pins are available separately on top of the board.*

For Nucleo and Discovery, you just need to connect the cable. Now hit upload and it should work.
