# MakerShop Smart Home Ecosystem

* [Suomeksi](./README_FI.md)

MakerShop Smart Home Ecosystem is collection of devices that are flashed with ESPHome generated code and controlled with Home Assistant home automation system.

## Devices

Ecosystem will include many different kind of devices to measure enviromental variables such as human presence, temperature, humidity, pressure, ambient light, VOC index etc. and to control devices like air conditioning and lighting.

### Already available:
- MultiSensor Pro
  - ESP32-C6
  - HLK-LD2410 / HLK-LD2410C / HLK-LD2450 24GHz Human Presence Sensor
  - 9x I2C
  - 2x SPI (Display / ADS1220)
  - 1-Wire
  - UART (5v I/O)
  - MQ-series sensors (5v out, 3.3v I/O)
  - PCB antenna or external antenna
  
- MultiSensor Mini
  - ESP32-C6
  - 4x I/O (I2C or SPI)
  - UART (5v I/O)
  - PCB antenna or external antenna

### Planned / under development:
- MultiSensor Basic
  - ESP32-C6
  - HLK-LD2410 / HLK-LD2410C / HLK-LD2450 24GHz Human Presence Sensor
  - I2C
  - SPI
  - 1-Wire
  - UART (5v I/O)
  - PCB antenna or external antenna
- VoiceControl
- Relay Mini / Basic / Pro
- LED control

## Installing ESPHome

### Installing base system
[ESPHome Install](https://esphome.io/guides/installing_esphome.html)

### Using ESPHome dashboard
[ESPHome Dashboard](https://esphome.io/guides/getting_started_command_line.html#bonus-esphome-dashboard)

In Linux, I have done alias in .bashrc so I can use command `get_esphome` to dive into ESPHome environment and start dashboard:

`alias get_esphome='source $HOME/.esp/esphome/bin/activate && esphome dashboard $HOME/.esp/esphome/config/'`

Just adjust esphome install path and place to store your config files according to your choice.

### Compiling code

ESP32-C6 is relatively new chip from Espressif and ESPHome is defaulting to ESP-IDF v4.4 that doesn't yet support C6 model. Also Arduino framework in ESPHome doesn't support it. So we need to use atleast ESP-IDF v5.1 to succesfully compile code in ESPHome to this chip. Fortunately, this is made quite easy to achieve, we only need to declare what version we want to use. So replace `esp32:`-section in start of your devices .yaml-file with following code:

- `esp32:`
- `  board: esp32-c6-devkitc-1`
- `  flash_size: 4MB`
- `  framework:`
- `    type: esp-idf`
- `    version: 5.2.1`
- `    platform_version: 6.7.0`
- `    sdkconfig_options:`
- `      CONFIG_ESPTOOLPY_FLASHSIZE_4MB: y`
- `  variant: esp32c6`

Also we need to change `logger`-section:
- `# Enable logging`
- `logger:`
- `  hardware_uart: USB_CDC`

...and code should compile with no problem.

### Using ADC and UART

For using A/D converter and UART, we need to do some manual modifications both ESP-IDF and ESPHome code. ESPHome will copy changed source files to device config-directory when compiled, so once changes are made they will stay across devices even if you run `Clean Build Files`.


