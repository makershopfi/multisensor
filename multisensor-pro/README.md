# Multisensor Pro

### Default config

File multisensor-pro.yaml is basic example config with default settings to start with, it is used just to verify that all interfaces are working. It includes following components:
- 1.9 inch TFT Display based on st7789v (SPI)
- ADS1220 A/D converter (SPI)
  - This component is not yet part of ESPHome but I have made pull request to ESPHome repository and there is still some things to be fixed. Basic A/D conversion and gain settings works.
- LD2410C 24GHz Human Presence Sensor (UART)
- DHT22 Temperature and Humidity Sensor(1-Wire)
- VEML7700 Ambient Light Sensor (I2C)
- BME280 Temperature, pressure and Humidity Sensor (I2C)
- SGP40 VOC Index Sensor (I2C)
- 5v UART
- MQ-7 Carbon Monoxide Gas Sensor

