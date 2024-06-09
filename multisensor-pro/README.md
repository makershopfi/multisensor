# Multisensor Pro

### Default config

File multisensor-pro.yaml is basic example config with default settings to start with, it is used just to verify that all interfaces are working. It includes following components:
- 1.9 inch TFT Display based on st7789v (SPI)
- ADS1220 (SPI)
  - This component is not yet part of ESPHome but I have made pull request to ESPHome repository and there is still some things to be fixed. Basic A/D conversion and gain settings works.
- LD2410C (UART)
- DHT22 (1-Wire)
- VEML7700 (I2C)
- BME280 (I2C)
- SGP40 (I2C)
- 5v UART
- MQ-7

