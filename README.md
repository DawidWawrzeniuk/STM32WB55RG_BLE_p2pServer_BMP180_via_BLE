# STM32WB55 BLE P2P Server — Temperature & Pressure Display
This project implements a Bluetooth Low Energy (BLE) GATT Server running on the STM32WB55 microcontroller.
The server receives environmental data (temperature + pressure) from a BLE client device and displays it on an SSD1306 OLED screen.

The project is based on the official P2P Server example from STMicroelectronics and extends it with:

*custom 6‑byte BLE payload decoding

*temperature & pressure handling

*OLED rendering

*BMP180 sensor integration on the client side





# 📡 BLE Architecture Overview
The server exposes a Peer-to-Peer (P2P) Service with two characteristics:

**1. Write Characteristic (Client → Server)**
UUID: P2P_WRITE_CHAR_UUID  
Properties: WRITE_WITHOUT_RESP | READ  
Max length: 6 bytes

**Payload format:**

Byte	Meaning
0	Device ID
1	Temperature (°C)
2	Pressure LSB
3	Pressure
4	Pressure
5	Pressure MSB



**Example decoding:**
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



**2. Notify Characteristic (Server → Client)**

UUID: P2P_NOTIFY_CHAR_UUID  

Properties: NOTIFY  

Length: 2 bytes


Used to send button/LED status back to the client.

# 📥 BLE Data Handling (p2p_server_app.c)
Incoming BLE writes are processed inside:

```c
case P2PS_STM_WRITE_EVT:
```
**The server:**

1.Logs raw bytes

2.Validates payload length

3.Extracts temperature

4.Extracts pressure (if available)

5.Updates global variables



**Example logging:**
````c
APP_DBG_MSG("RAW (%d bytes): ", len);
for(uint8_t i = 0; i < len; i++)
{
    APP_DBG_MSG("0x%02X ", raw[i]);
}
APP_DBG_MSG("\n");
````

#📺 OLED Display Rendering
The server displays the received values on an SSD1306 OLED using DMA‑driven I2C.

**Rendering loop (main.c):**

````c
if(hi2c1.hdmatx->State == HAL_DMA_STATE_READY)
{
    SSD1306_Clear(BLACK);

    extern uint8_t temp;
    extern uint32_t pressure;

    uint32_t pressure_hPa = pressure / 100;

    char txt[20];
    sprintf(txt, "Temperature: %d C", temp);
    GFX_DrawString(10, 30, txt, WHITE, BLACK);

    char txt2[20];
    sprintf(txt2, "Pressure: %lu hPa", pressure_hPa);
    GFX_DrawString(10, 40, txt2, WHITE, BLACK);

    SSD1306_Display();
}
````

#⏱ Timer & Refresh Logic
A 1 ms RTC wakeup interrupt increments a software timer.
The OLED refreshes only when:

```c
hi2c1.hdmatx->State == HAL_DMA_STATE_READY
This prevents I2C bus contention and ensures smooth rendering.
```
#🌡 Global Variables (Server)
```c
uint8_t temp = 0;
uint32_t pressure = 0;
These are updated exclusively by BLE events.
```
#📤 BLE Client Payload Format
The BLE client sends:

````c
uint8_t payload[6];
payload[0] = 0x01;          // Device ID
payload[1] = temperature;   // 1 byte
payload[2] = pressure >> 0;
payload[3] = pressure >> 8;
payload[4] = pressure >> 16;
payload[5] = pressure >> 24;
````
Transmission:
````c
aci_gatt_write_without_resp(connHandle, charHandle, 6, payload);
````























