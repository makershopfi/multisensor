# MakerShop MultiSensor-alusta älykoteihin

- [In English](./README.md)

MakerShop MultiSensor-alusta älykoteihin on kokoelma laitteita jotka käyttävät ESPHomen generoimaa koodia ja jotka voidaan liittää Home Assistant kotiautomaatiojärjestelmään.

## Laitteet

MultiSensor-alusta tulee kasvamaan monenlaisilla laitteilla joilla voi seurata esim. ihmisten läsnäoloa eri tiloissa, lämpötilaa, suhteellista ilmankosteutta, ilmanpainetta, ympäristön valoisuutta, ilmanlaatua ja ohjata sen perusteella erilaisia laitteita kuten ilmastointijärjestelmää sekä valaistusta. Alla yhteenveto tämänhetkisestä tilanteesta laitteista niiden perusasetuksilla.

### Saatavilla:
- MultiSensor Pro
  - ESP32-C6
  - HLK-LD2410 / HLK-LD2410C / HLK-LD2450 24GHz läsnäolotunnistin
  - 9x I2C (2x5 liitin jossa kaksi yleisintä I2C anturilevyn pinouttia ja JST PH joka tietenkin voidaan kytkeä mihin vaan)
  - 2x SPI (1.9" TFT Näyttö + ADS1220)
  - 1x 1-Wire (2x liitäntä 5v, GND ja signaalille, helpottaa useamman anturin kytkemistä)
  - UART (5v I/O)
  - MQ-sarjan kaasuanturit (5v out, 3.3v I/O)
  - Piirilevyantenni tai ulkoinen antenni
 
- MultiSensor Mini
  - ESP32-C6
  - 4x I/O (I2C / SPI)
  - UART (5v I/O)
  - Piirilevyantenni tai ulkoinen antenni

- MultiSensor Mini Shield
  - MQ-7 CO kaasuanturi
 
### Suunnitelmissa / työn alla:
- MultiSensor Advanced
  - ESP32-C6
  - HLK-LD2410 / HLK-LD2410C / HLK-LD2450 24GHz läsnäolotunnistin
  - I2C
  - SPI
  - 1-Wire
  - UART (5v I/O)
  - Piirilevyantenni tai ulkoinen antenni
- MultiSensor Mini Shieldejä
- MultiSensor Voice
- MultiSensor Relay
- MultiSensor LED

## ESPHome asennus

### Perusjärjestelmän asennus
[ESPHome Install](https://esphome.io/guides/installing_esphome.html)

### ESPHome dashboardin käyttäminen
[ESPHome Dashboard](https://esphome.io/guides/getting_started_command_line.html#bonus-esphome-dashboard)

Linuxissa olen tehnyt aliaksen .bashrc-tiedostoon jolloin voin antaa komennon `get_esphome` siirtyäkseni ESPHome-ympäristön sisään ja käynnistämään dashboardin:

```
alias get_esphome='source $HOME/.esp/esphome/bin/activate && esphome dashboard $HOME/.esp/esphome/config/'
```

Muuta tarvittaessa ESPhomen asennuspolku ja kansio johon haluat ESPHomen tallentavan laitteiden asetustiedostot ja lähdekoodit. Kun dashboard on käynnissä, avaa selaimessa sivu
```
http://localhost:6052/
```
niin pääset lisäämään / muokkaamaan laitteita.

### Koodin kääntäminen

ESP32-C6 on suhteellisen uusi tuttavuus Espressifiltä ja ESPHomessa uusin virallinen ESP-IDF versio on 4.4 joka ei vielä C6-versiota tue, kuten ei myöskään Arduinon ympäristö. ESP-IDF pitää olla vähintään veriota 5.1 jotta tuki C6-mallille löytyy. Onneksi ESPHomessa tämä on tehty kohtuullisen helpoksi, tarvitsee vain määritellä mitä versioita haluaa käyttää. Versiot määritellään .yaml-tiedoston alkupuolella esp32-osiossa alla olevan mukaisesti:

```
esp32:
  board: esp32-c6-devkitc-1
  flash_size: 4MB
  framework:
    type: esp-idf
    version: 5.2.1
    platform_version: 6.7.0
    sdkconfig_options:
      CONFIG_ESPTOOLPY_FLASHSIZE_4MB: y
  variant: esp32c6
```

Myös `logger`-osioon lisätään käytettävä loggaus-liityntä:
```
# Enable logging
logger:
  hardware_uart: USB_CDC
```

...ja nyt koodin pitäisi kääntyä ongelmitta.

### ADC ja UART käyttö

#### Laitteen koodi pitää kääntää kertaalleen ennen muutoksia koska ESPHome lataa tässä vaiheessa automaattisesti tarvittavat lähdekoodit oikeaan paikkaan.

ESP32-C6 natiivin A/D-muuntimen ja UARTin käyttäminen vaatii muutaman muutoksen ESP-IDF ja ESPHome lähdekoodeihin. ESPHome kopioi käytettävät koodit aina kääntämisen yhteydessä config-kansioon laitteen kansion alle, joten nämä kertaalleen tehdyt muutokset säilyvät ja toimivat myös muissa laitteissa myöhemminkin vaikka poistaisi laitekansiot. Muutokset löytyvät [täältä](https://github.com/makershopfi/smarthome/tree/main/.platformio/packages/framework-espidf/components/esp_adc/deprecated) ja [täältä](https://github.com/makershopfi/smarthome/tree/main/esphome/lib/python3.12/site-packages/esphome/components)
