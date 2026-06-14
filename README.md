# STM32WB55 BLE P2P Server — Temperature & Pressure Display
This project implements a Bluetooth Low Energy (BLE) GATT Server running on the STM32WB55 microcontroller.
The server receives environmental data (temperature + pressure) from a BLE client device and displays it on an SSD1306 OLED screen.

The project is based on the official P2P Server example from STMicroelectronics and extends it with:

custom 6‑byte BLE payload decoding

temperature & pressure handling

OLED rendering

BMP180 sensor integration on the client side





# 📡 BLE Architecture Overview
The server exposes a Peer-to-Peer (P2P) Service with two characteristics:

1. Write Characteristic (Client → Server)
UUID: P2P_WRITE_CHAR_UUID  
Properties: WRITE_WITHOUT_RESP | READ  
Max length: 6 bytes

Payload format:

Byte	Meaning
0	Device ID
1	Temperature (°C)
2	Pressure LSB
3	Pressure
4	Pressure
5	Pressure MSB



Example decoding:
````c
uint8_t device_id = raw[0];
uint8_t temp      = raw[1];

uint32_t press =
      ((uint32_t)raw[2] << 0)
    | ((uint32_t)raw[3] << 8)
    | ((uint32_t)raw[4] << 16)
    | ((uint32_t)raw[5] << 24);

pressure = press;
````
