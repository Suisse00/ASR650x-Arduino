# Basic

|Include|Static variable|Description|#define name|[Diagram name](https://github.com/HelTecAutomation/ASR650x-Arduino/blob/master/PinoutDiagram/HTCC-AB01.pdf)|WIP Note|
|-------|--------|----------|----|------------|----------|
|SPI.h|SPI|SPI Hardware|MOSI, MISO, SCK|MOSI, MISO, SCK|
|Serial.h|Serial|Serial Hardware|UART_RX, UART_TX|RX TX||
|Wire.h||TwoWire library||||
||Wire||SDA, SCL|SDC, SCL||
||Wire1||SDA, SCL|SDC, SCL|TODO: Same as Wire? The only difference between Wire and Wire1 is a constructor parameter that is unused.|
|[LoRaWan_APP.h](../app/LoRaWan_APP.md)|[LoRaWAN](../app/LoRaWan_APP.md#LoRaWanClass-(LoRaWAN-variable))|Fully implement the LoRaWan protocols layers.|RADIO_MOSI* RADIO_MISO* RADIO_SCLK*|LoRa_MOSI LoRa_MISO LoRa_SCLK||

\* TODO: Must be enabled? (how?) Diagram show two pin numbers.

# Avanced

|Include|Static variable|Description|#define name|[Diagram name](https://github.com/HelTecAutomation/ASR650x-Arduino/blob/master/PinoutDiagram/HTCC-AB01.pdf)|WIP Note|
|-------|--------|----------|----|------------|----------|
|[LoRa_APP.h](../app/LoRa_APP.md)|[LoRa](../app/LoRa_APP.md#LoRaClass-(LoRa-variable))|A simple exemple of LoRa RF (physical layer) implementation.|RADIO_MOSI* RADIO_MISO* RADIO_SCLK*|LoRa_MOSI LoRa_MISO LoRa_SCLK|Used only in the AT exemple. The ping pong exemple use Radio.|
|Included ([radio.h](../../advanced/radio.md))|[Radio](../../advanced/radio.md#Radio_s-(variable-Radio))|Generic High level RF driver. For low level RF see [advanced/SX126x](../../advanced/SX126x.md).|RADIO_MOSI* RADIO_MISO* RADIO_SCLK*|LoRa_MOSI LoRa_MISO LoRa_SCLK||

\* TODO: Must be enabled? (how?) Diagram show two pin numbers.