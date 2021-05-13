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

# Building klippers
## Mini12864 display

https://github.com/VoronDesign/Voron-Hardware/tree/master/STM32_Mini12864

  [*] Enable extra low-level configuration options
      Micro-controller Architecture (STMicroelectronics STM32)  --->
      Processor model (STM32F042)  --->
      Clock Reference (Internal clock)  --->
      Communication interface (USB (on PA9/PA10))  --->
      USB ids  --->
  [ ] Specify a custom step pulse duration
  ()  GPIO pins to set at micro-controller startup

Short BOOT0 pins, reboot, and flash:

  make flash FLASH_DEVICE=0483:df11

## Huvud board

https://github.com/bondus/KlipperToolboard/blob/master/doc/klipper.md

  [*] Enable extra low-level configuration options
      Micro-controller Architecture (STMicroelectronics STM32)  --->
      Processor model (STM32F103)  --->
      Bootloader offset (2KiB bootloader (HID Bootloader))  --->
      Clock Reference (8 MHz crystal)  --->
      Communication interface (USB (on PA11/PA12))  --->
      USB ids  --->
  [ ] Specify a custom step pulse duration
  ()  GPIO pins to set at micro-controller startup

BOOT1 to 3.3V, reboot, and flash:

    make flash FLASH_DEVICE=1209:beba

## BTT SKR E3 Turbo

[*] Enable extra low-level configuration options
    Micro-controller Architecture (LPC176x (Smoothieboard))  --->
    Processor model (lpc1769 (120 MHz))  --->
[*] Target board uses Smoothieware bootloader (NEW)
    Communication interface (USB)  --->
    USB ids  --->
[ ] Specify a custom step pulse duration
()  GPIO pins to set at micro-controller startup

Use CD card for updating.