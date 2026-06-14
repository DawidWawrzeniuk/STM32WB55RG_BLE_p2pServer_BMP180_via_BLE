STM32WB55 BLE P2P Server — Temperature & Pressure Display
This project implements a Bluetooth Low Energy (BLE) GATT Server running on the STM32WB55 microcontroller.
The server receives environmental data (temperature + pressure) from a BLE client device and displays it on an SSD1306 OLED screen.

The project is based on the official P2P Server example from STMicroelectronics and extends it with:

custom 6‑byte BLE payload decoding

temperature & pressure handling

OLED rendering

BMP180 sensor integration on the client side
