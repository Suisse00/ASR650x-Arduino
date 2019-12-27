For the available commands and specs (for UART) read [basic/lora_at](../../basic/lora_at.md).


# void Enable_AT(void)
Configure the UART pins and timer so AT commands are transmitted from/to UART.

Can use a different set of pine by setting #define UART_1_SCB_MODE using one of the following value: UART_1_SCB_MODE_I2C, UART_1_SCB_MODE_SPI, UART_1_SCB_MODE_UART, UART_1_SCB_MODE_EZI2C

# void getDevParam(void)
Fill back [basic/app/LoRaWan_APP.h](../../basic/app/LoRaWan_APP.md) [variables](../../basic/app/LoRaWan_APP.md#Variables) from flash memory.

TODO: Find where it save them

# void printDevParam(void)
Print [basic/app/LoRaWan_APP.h](../../basic/app/LoRaWan_APP.md) [variables](../../basic/app/LoRaWan_APP.md#Variables).

**CONTAINS SENSIBLE INFORMATIONS SUCH AS PRIVATE KEYS**

# void SaveNetInfo(uint8_t *joinpayload, uint16_t size)

Save the LoRaMacDevNonce & LoRaMacAppKey into flash (TODO: and a payload?)

TODO: Find exacly what it save (probably the network keys)

TODO Find what is the public equivalent of LoRaMacDevNonce / LoRaMacAppKey (2 bytes variable? / AppKey?)

# void SaveUpCnt(void)
Save into flash the current UpLinkCounter (uplink frame number?; message number going out of the device) only if variable [KeepNet](../../basic/app/LoRaWan_APP.md#bool-KeepNet) is `true`.

TODO: Clarify if this is indeed the uplink frame number

# void SaveDownCnt(void)
Save into flash the current DownLinkCounter (downlink frame number?; message number going in the device) only if variable [KeepNet](../../basic/app/LoRaWan_APP.md#bool-KeepNet) is `true`.

TODO: Clarify if this is indeed the downlink frame number

# void GetNetInfo(void)
Read back from flash memory the previous saved LoRaWAN network sessions informations according with [basic/app/LoRaWan_APP.h](../../basic/app/LoRaWan_APP.md) [OVER_THE_AIR_ACTIVATION](../../basic/app/LoRaWan_APP.md#bool-OVER_THE_AIR_ACTIVATION).

# void SaveDr(void);
TODO

# bool CheckNetInfo(void)
Return the flash persisted [KeepNet](../../basic/app/LoRaWan_APP.md#bool-KeepNet) status.

TODO: Double check, this is just I guess which doesn't quite make sence with the NetInfoDisable() call in LoRaWAN.Ifskipjoin()

# void NetInfoDisable(void)
TODO

TODO WIP NOTE: Called in LoRaWAN.Ifskipjoin()

# bool AT_user_check(char * cmd, char * content)
Method called by the CubeCell library for programmer to implement their custom AT commands.

It is called before the built-in AT command.

If the AT command engine receive "AT+XXXX=YYYY" then `cmd` parameter will be `XXXX` and `content` will be `YYYY`.
Returning true will prevent the built-in AT command to be processed.

There is an example provided in LoRa/examples/LoRaWan_user_AT