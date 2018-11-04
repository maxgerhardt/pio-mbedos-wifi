# Platformio + mbedos + WiFi

## Description

The PIO compilable version of https://github.com/ARMmbed/mbed-os-example-wifi/. 

## Compilation

Look into `platformio.ini` to understand what's going on.
For different boards, reconfigure this file.

## Lib versions

This repo contains a bug-fixed snapshot of the ESP8266 driver module in respect to [this issue](https://github.com/ARMmbed/esp8266-driver/pull/111). 

Repo was pulled 4th Nov 2018.

## AT Firmware

The ESP8266 must be running the AT firmware of at least version 1.7. 

Download: https://www.espressif.com/en/support/download/at?keys=&field_type_tid[]=14

How to flash a firmware on your ESP8266: 
 * Flash Download Tools from [here](https://www.espressif.com/en/support/download/overview)
 * See thread [here](https://github.com/espressif/ESP8266_NONOS_SDK/issues/168)

I had to follow [this procedure](https://github.com/espressif/ESP8266_NONOS_SDK/issues/179) to flash it to my NodeMCU with 4MB flash. Above image runs by default with 16Mbit flash map.

ESP-01 is not supported because the AT firmware can't fit on the flash. 


## Pinouts 

|mbed-board   | ESP8266  | Notes |
|---|---|---|
| TX  | RXD0 (GPIO1)  | choose a hardware serial pin for mbed-board
| RX  | TXD0 (GPIO3)  | choose a hardware serial pin for mbed-board
| RTS  | CTS0 (IO13)  | optional, NodeMCU D7
| CTR  | RTS0 (IO15)  | optional, NodeMCU D8

If you do not set the RTS/CTS hardware flow control pins to "NC" (not connected) in the `platformio.ini`.

For [my board](https://os.mbed.com/platforms/ST-Nucleo-L152RE/#board-pinout), I used  the pins for the `USART1` peripheral, namely
 * UART1_TX: PA_9
 * UART1_RX: PA_10
 * UART1_CTS: PA_11
 * UART1_RTS: PA_12

You must always use the correct hardware pins for you board, this is not software bit-banged.

Your ESP8266 should have a good power supply. I used a NodeMCU board powered via USB, separate from the Nucleo L152RE's power supply. 

To let two devices with different power supplies communicate, you must connect their GNDs together!

## Output

Expected output should be

```
WiFi example
Hello 2
Mbed OS version 5.10.1

Scan:
Network: wifi_name_here pass secured: WPA2 BSSID: XX:XX:XX:XX:XX:XX RSSI: -48 Ch: 11
[.. more networks ..]
22 networks available.

Connecting to wifi_name_here pass...
Success

MAC: XX:XX:XX:XX:XX:XX
IP: 192.168.1.135
Netmask: 255.255.255.0
Gateway: 192.168.1.1
RSSI: -47

Sending HTTP request to www.arm.com...
sent 58 [GET / HTTP/1.1]
recv 64 [HTTP/1.1 200 OK]

Done
```

## Credits

All code was written by the mbedos people. Example configuration file by me.
