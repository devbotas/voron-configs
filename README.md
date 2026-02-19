# To configure repository to work on printers themselves:

1. Go to your Github account settings -> Developer settings -> Personal access tokens.
2. Generate a new token for that specific printer, with write access to repository.
3. Create a /home/pi/.netrc file
4. Add content to it:
```
  machine github.com
  login <your-github-login>
  password <token-you-just-generated>
```
5. If not done already, tell git who you are:
```
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```

Done. Now you may push to the repo from the printer directly.

There are two files that are untracked because they are very printer-specific, printer.cfg-template and variables.cfg-template. Clone them into printer.cfg and variables.cfg and modify as needed.

# Configuring CAN

For Seeed CAN FD HAT, follow this: https://wiki.seeedstudio.com/2-Channel-CAN-BUS-FD-Shield-for-Raspberry-Pi/

Then 

```
sudo nano /etc/network/interfaces.d/can0
```
And paste these lines:
```
allow-hotplug can0
iface can0 can static
    bitrate 1000000
    up ip link set $IFACE txqueuelen 65535
```

# Building klippers
## Mini12864 display

https://github.com/VoronDesign/Voron-Hardware/tree/master/STM32_Mini12864
```
[*] Enable extra low-level configuration options
    Micro-controller Architecture (STMicroelectronics STM32)  --->
    Processor model (STM32F042)  --->
    Bootloader offset (No bootloader)  --->
    Clock Reference (Internal clock)  --->
    Communication interface (USB (on PA9/PA10))  --->
    USB ids  --->
    Optional features (to reduce code size)  --->
()  GPIO pins to set at micro-controller startup
```
Klipper won't fit onto flash, so deselect some features from "Optional features" section.

Short BOOT0 pins, reboot, and flash:
```
  make flash FLASH_DEVICE=0483:df11
```

## Huvud board

https://github.com/bondus/KlipperToolboard/blob/master/doc/klipper.md

```
[*] Enable extra low-level configuration options
    Micro-controller Architecture (STMicroelectronics STM32)  --->
    Processor model (STM32F103)  --->
[ ] Only 10KiB of RAM (for rare stm32f103x6 variant)
[ ] Disable SWD at startup (for GigaDevice stm32f103 clones)
    Bootloader offset (2KiB bootloader)  --->
    Clock Reference (8 MHz crystal)  --->
    Communication interface (CAN bus (on PB8/PB9))  --->
(1000000) CAN bus speed
()  GPIO pins to set at micro-controller startup
```

Short BOOT1 to 3.3V, reboot, and flash:

```
make flash FLASH_DEVICE=1209:beba
```

## BTT SKR E3 Turbo
```
[*] Enable extra low-level configuration options
    Micro-controller Architecture (LPC176x)  --->
    Processor model (lpc1769 (120 MHz))  --->
    Bootloader offset (16KiB bootloader)  --->
    Communication interface (USB)  --->
    USB ids  --->
()  GPIO pins to set at micro-controller startup
```
Use SD card for updating. rename klipper.bin to FIRMWARE.BIN.

## BTT SKR Mini E3 V2

https://docs.vorondesign.com/build/software/miniE3_v20_klipper.html

```
[*] Enable extra low-level configuration options
    Micro-controller Architecture (STMicroelectronics STM32)  --->
    Processor model (STM32F103)  --->
[ ] Only 10KiB of RAM (for rare stm32f103x6 variant) (NEW)
[ ] Disable SWD at startup (for GigaDevice stm32f103 clones) (NEW)
    Bootloader offset (28KiB bootloader)  --->
    Clock Reference (8 MHz crystal)  --->
    Communication interface (USB (on PA11/PA12))  --->
    USB ids  --->
(!PA14) GPIO pins to set at micro-controller startup
```
## BTT SKR 1.4

Remove driver jumpers, leave CS/RX only. For the endstop pins to work, DIAG pin must be cut for the corresponding driver.

If connecting Mini 12864 LCD, IDC10 shrouds shall be reversed if brands mismatch (i.e. display is from Fysetc).

To install Katapult follow this:
https://klipper.discourse.group/t/canboot-flash-btt-skr-1-3-1-4-1-4-turbo/3238

```
    Micro-controller Architecture (LPC176x (Smoothieboard))  --->
    Processor model (lpc1768 (100 MHz))  --->
    Build Katapult deployment application (Do not build)  --->
    Communication interface (USB)  --->
    USB ids  --->
()  GPIO pins to set on bootloader entry
[*] Support bootloader entry on rapid double click of reset button
[ ] Enable bootloader entry on button (or gpio) state
[ ] Enable Status LED
```

Klipper:

```
[*] Enable extra low-level configuration options
    Micro-controller Architecture (LPC176x)  --->
    Processor model (lpc1768 (100 MHz))  --->
    Bootloader offset (16KiB bootloader)  --->
    Communication interface (USB)  --->
    USB ids  --->
[*] Optimize stepper code for 'step on both edges'
()  GPIO pins to set at micro-controller startup
```

Note: you can the use `make flash FLASH_DEVICE=/dev/ACMxx` to upload Klipper.

