# ESP-IDF framework changed source files

### ADC

All files here are needed to make ESP32-C6 native A/D converter to work in ESPHome. Some newer chips have only 1 ADC so we need to hide second ADC code from compiler if there is only one ADC declared in chip and ESP32-C6 needs to be included in some header and source files. Also we need to add folder with one source file for ESP32-C6.

#### Just copy files to their corresponding locations and all should be fine
