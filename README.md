# MakerShop MultiSensor Platform for Smart Homes

* [Suomeksi](./README_FI.md)

MakerShop MultiSensor Platform for Smart Homes is collection of devices that are flashed with ESPHome generated code and controlled with Home Assistant home automation system.

## Devices

Platform will include many different kind of devices to measure enviromental variables such as human presence, temperature, humidity, pressure, ambient light, VOC index etc. and to control devices like air conditioning and lighting. Below is info about boards with default setups. All I/O ports can be configured as user likes, but these are the intended default configurations.

### Already available:
- MultiSensor Pro
  - ESP32-C6
  - HLK-LD2410 / HLK-LD2410C / HLK-LD2450 24GHz Human Presence Sensor
  - 9x I2C (2x5 header have two most usual I2C sensor board pinouts and JST can connect to any pinout)
  - 2x SPI (1.9" TFT Display + ADS1220)
  - 1x 1-Wire (2x connectors for 5v, GND and signal)
  - UART (5v I/O)
  - MQ-series sensors (5v out, 3.3v I/O)
  - PCB antenna or external antenna
  
- MultiSensor Mini
  - ESP32-C6
  - 4x I/O (I2C / SPI)
  - UART (5v I/O)
  - PCB antenna or external antenna

### Planned / under development:
- MultiSensor Advanced
  - ESP32-C6
  - HLK-LD2410 / HLK-LD2410C / HLK-LD2450 24GHz Human Presence Sensor
  - I2C
  - SPI
  - 1-Wire
  - UART (5v I/O)
  - PCB antenna or external antenna
- MultiSensor Voice
- MultiSensor Relay
- MultiSensor LED

## Installing ESPHome

### Installing base system
[ESPHome Install](https://esphome.io/guides/installing_esphome.html)

### Using ESPHome dashboard
[ESPHome Dashboard](https://esphome.io/guides/getting_started_command_line.html#bonus-esphome-dashboard)

In Linux, I have done alias in .bashrc so I can use command `get_esphome` to dive into ESPHome environment and start dashboard:

`alias get_esphome='source $HOME/.esp/esphome/bin/activate && esphome dashboard $HOME/.esp/esphome/config/'`

Just adjust esphome install path and place to store your config files according to your choice. Once dashboard is running, open page `http://localhost:6052/` on your browser.

### Compiling code

ESP32-C6 is relatively new chip from Espressif and ESPHome is defaulting to ESP-IDF v4.4 that doesn't yet support C6 model. Also Arduino framework in ESPHome doesn't support it. So we need to use atleast ESP-IDF v5.1 to succesfully compile code in ESPHome to this chip. Fortunately, this is made quite easy to achieve, we only need to declare what version we want to use. So replace esp32-section in start of your devices .yaml-file with following code:

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

For using A/D converter and UART, some manual modifications is needed for both, ESP-IDF and ESPHome code. ESPHome will copy changed source files to device config-directory compile-time, so once changes are made they will stay and will be shared with all devices even if you run `Clean Build Files`. Changes can be found [here](https://github.com/makershopfi/smarthome/tree/main/.platformio/packages/framework-espidf/components/esp_adc/deprecated) and [here](https://github.com/makershopfi/smarthome/tree/main/esphome/lib/python3.12/site-packages/esphome/components)

### You need to build your device once with changes in esp32-section mentioned above to let ESPHome to download all necessary files.

