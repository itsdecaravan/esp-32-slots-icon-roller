# ESP32 OLED Slot Machine

A beginner-friendly MicroPython toy project for an `ESP32-WROOM` and a small `SSD1306 I2C OLED` display.

This project shows three slot-machine-style reels on the OLED using simple hand-drawn pixel icons such as cherries, stars, diamonds, hearts, lemons, and sevens.

It is a fun display project only:

- no real money
- no gambling
- beginner-friendly wiring
- easy to edit in VS Code or Thonny

## Features

- runs on `ESP32-WROOM`
- uses `MicroPython`
- uses an `SSD1306 128x64 I2C OLED`
- displays 3 slot reels
- uses simple pixel-art icon symbols
- easy to modify for your own symbols and animations

## Hardware

You need:

- `ESP32 Dev Board` or `ESP32-WROOM`
- `SSD1306 I2C OLED` display, ideally `128x64`
- breadboard
- jumper wires
- USB data cable

Optional:

- push button for a future spin input
- LEDs for a future win effect

## Wiring

For the OLED:

- `OLED VCC -> ESP32 3V3`
- `OLED GND -> ESP32 GND`
- `OLED SDA -> ESP32 GPIO21`
- `OLED SCL -> ESP32 GPIO22`

## Wiring Diagram

```text
ESP32                OLED SSD1306
-----                ------------
3V3   -------------- VCC
GND   -------------- GND
GPIO21-------------- SDA
GPIO22-------------- SCL
```

Important:

- use `3V3`, not `5V`
- check the OLED pin labels carefully
- some OLED modules label pins as `GND VCC SCL SDA`

## Project Files

- `main.py` - main MicroPython file for the ESP32
- `esp32.py` - editable copy for local development
- `ssd1306.py` - OLED driver
- `.vscode/tasks.json` - optional VS Code helper tasks

## Firmware

This project was set up for:

- `ESP32-WROOM`
- `ESP32_GENERIC` MicroPython firmware

Official firmware page:

- `https://micropython.org/download/ESP32_GENERIC/`

## Install MicroPython

Install tools on Linux:

```bash
sudo apt update
sudo apt install pipx
pipx ensurepath
source ~/.bashrc
pipx install esptool
pipx install mpremote
```

Find your board:

```bash
mpremote devs
```

Example port:

- `/dev/ttyUSB0`

Erase flash:

```bash
sudo /home/enys/.local/bin/esptool --port /dev/ttyUSB0 erase-flash
```

Flash MicroPython:

```bash
sudo /home/enys/.local/bin/esptool --port /dev/ttyUSB0 --baud 460800 write-flash 0x1000 ESP32_GENERIC-xxxx.bin
```

## Upload Files

Upload the OLED driver:

```bash
mpremote connect /dev/ttyUSB0 fs cp ssd1306.py :ssd1306.py
```

Upload the main program:

```bash
mpremote connect /dev/ttyUSB0 fs cp main.py :main.py
```

Reset the board:

```bash
mpremote connect /dev/ttyUSB0 reset
```

List files on the ESP32:

```bash
mpremote connect /dev/ttyUSB0 fs ls
```

## Run From Thonny

If you prefer Thonny:

1. Install Thonny
2. Set interpreter to `MicroPython (ESP32)`
3. Open `main.py`
4. Save `main.py` to the ESP32
5. Save `ssd1306.py` to the ESP32
6. Run the file

## Current Behavior

The current version focuses on the OLED reel display.

Depending on which version of `main.py` you keep, it can be used in two simple ways:

- automatic rolling icon display
- reset-to-spin style using the `EN` reset button

Note:

- the `EN` button is the reset button, not a normal game input
- real Unicode emoji do not work on SSD1306 with the default MicroPython font
- this project uses hand-drawn pixel icons instead

## Why The Icons Are Not Real Emoji

The SSD1306 display and standard MicroPython text drawing do not support normal Unicode emoji like:

- `🍒`
- `⭐`
- `💎`

So this project draws custom symbols in pixels instead.

## Testing The OLED

Open REPL:

```bash
mpremote connect /dev/ttyUSB0 repl
```

Then test I2C:

```python
from machine import Pin, I2C
i2c = I2C(0, scl=Pin(22), sda=Pin(21))
print(i2c.scan())
```

Expected result:

- `[60]` or `[61]` means the OLED was found
- `[]` means the OLED is not wired correctly

Basic OLED text test:

```python
import ssd1306
oled = ssd1306.SSD1306_I2C(128, 64, i2c)
oled.fill(0)
oled.text("HELLO", 30, 20)
oled.show()
```

## Troubleshooting

### `machine` import warning in VS Code

This is normal.

`machine` is a MicroPython module on the ESP32, not a normal desktop Python module.

### OLED not found

If `i2c.scan()` returns `[]`:

- check `VCC`
- check `GND`
- check `SDA`
- check `SCL`
- try another `GND` pin
- make sure the OLED is powered from `3V3`

### Screen is blank

- make sure `ssd1306.py` is uploaded
- confirm the OLED is `128x64` or adjust the code
- check the I2C address

### Weird characters in terminal

Make sure you are in the MicroPython REPL, not the normal Linux shell:

```bash
mpremote connect /dev/ttyUSB0 repl
```

### ESP32 button confusion

- `EN` resets the board
- `BOOT` is for boot mode
- neither is a proper beginner game button

If you want a real spin button later, wire one to a GPIO pin such as `GPIO14`.

## Future Ideas

- add a real push button for spin
- add scoring
- add win detection
- add LED flashes
- draw larger pixel-art symbols
- add reel-by-reel stopping

## License

Use this project for learning, experimenting, and having fun.
