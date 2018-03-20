# bp-cubemx

Exploring ST's CubeMX and HAL on a STM32F103C8T6 dev-board (BluePill).

Starting from a simple blinky application all key interfaces to external devices
such as I2C, SPI, UART, USB, and CAN will be implemented.

In addition a CLI based build and debug process will be defined.

SEGGER's j-link pro is used as JTAG adapter.


## blinky

Using CubeMX
* Pin PC13 of STM32F103C8T6 is configured as GPIO output.
* Source code and Makefile are generated automatically.

Makefile is modified
* BINPATH is pointing to the gnu arm toolchain
* C_SOURCES is cleared from duplicates which cause compiling errors.

Firmware build/blinky.hex is loaded to the STM32F103C8T6.

