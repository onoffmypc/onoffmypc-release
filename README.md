# OnOffMyPC — ESP32 Firmware

Control your PC remotely using an ESP32 microcontroller and the [OnOffMyPC](https://onoffmypc.com) service.

## Features

- Wake-on-LAN to power on your PC
- Power off and reset via GPIO-controlled transistors
- Room temperature and humidity monitoring (DHT22)
- Secure WebSocket connection to the cloud relay
- LED status indicator (fast blink = connecting, slow = ready, solid = online)

## Requirements

- ESP32 development board (e.g. ESP32 DevKit V1)
- DHT22 temperature/humidity sensor
- PlatformIO CLI or VS Code with the PlatformIO extension

## Wiring

| Signal          | ESP32 Pin |
|-----------------|-----------|
| DHT22 data      | D4        |
| Status LED      | D2        |
| PC power sense  | D34       |
| Power button    | D26       |
| Reset button    | D27       |

Use a 330Ω resistor in series with the status LED.  
Use NPN transistors (e.g. 2N2222) driven by D26/D27 to simulate button presses.  
Connect D34 to the PC's power LED header (3.3 V max).

## Setup

1. **Create an account** at [app.onoffmypc.com](https://app.onoffmypc.com) and add a device. Copy the device token shown — you will not see it again.

2. **Configure the firmware**

   Copy `src/config.example.h` to `src/config.h` and fill in your values:

   ```cpp
   #define WIFI_SSID     "YourWiFi"
   #define WIFI_PASSWORD "YourPassword"
   #define DEVICE_ID     "paste-device-id-here"
   #define DEVICE_TOKEN  "paste-token-here"
   #define PC_MAC        "AA:BB:CC:DD:EE:FF"   // your PC's MAC address
   ```

   To find your PC's MAC address run `ipconfig /all` (Windows) or `ip link` (Linux).

3. **Flash**

   ```bash
   pio run -e esp32dev --target upload
   pio device monitor
   ```

   The LED blinks fast while connecting to WiFi, slow while connecting to the relay, then goes solid when online.

## Releases

Pre-built `.bin` files are published on the [Releases](../../releases) page.  
Flash with esptool:

```bash
esptool.py --port /dev/ttyUSB0 write_flash 0x0 firmware.bin
```

## License

MIT — see [LICENSE](LICENSE)
