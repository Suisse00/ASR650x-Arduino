High level RF
For low level control see [advanced/SX126x](SX126x.md) or [basic/app/LoRaWan_APP](../basic/app/LoRaWan_APP.md) if you are looking to use LoRaWAN.

* Must call BoardInitMcu() in the setup()
* Available example: LoRa/examples/pingpong

TODO

# PINs constants
RADIO_MOSI*
RADIO_MISO*
RADIO_SCLK*

\* Most be enabled (how?); Named LoRa_MOSI, LoRa_MISO and LoRa_SCLK on the [diagram](https://github.com/HelTecAutomation/ASR650x-Arduino/blob/master/BlockDiagram/HT-AB01.pdf)

# PINs constants (internal?)
RADIO_NSS
RADIO_BUSY
RADIO_DIO_1
RADIO_RESET
RADIO_ANT_SWITCH_POWER