* Must call Enable_AT() in setup()
* Use some [basic/app/LoRaWan_APP.h](app/LoRaWan_APP.md) [variables](app/LoRaWan_APP.md#Variables)

[AT commands](https://docs.heltec.cn/download/cubecell/CubeCell_Series_AT_Command_User_Manual_V0.2.pdf)

Programmer can add custom AT command by implementing [AT_user_check(char * cmd, char * content)](../CubeCellLib/cores/at_command.md#bool AT_user_check(char * cmd, char * content))

For more details read the [CubeCellLib/cores/at_command](../CubeCellLib/cores/at_command.md)