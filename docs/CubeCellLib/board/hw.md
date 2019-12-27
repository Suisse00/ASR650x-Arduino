TODO

typedef enum
{
  e_LOW_POWER_RTC = (1 << 0),
  e_LOW_POWER_GPS = (1 << 1),
  e_LOW_POWER_UART = (1 << 2), /* can be used to forbid stop mode in case of uart Xfer*/
} e_LOW_POWER_State_Id_t;

void HW_Reset(int mode);
char *HW_Get_MFT_ID();
uint32_t HW_Get_MFT_Baud();
bool HW_Set_MFT_Baud(uint32_t baud);
char *HW_Get_MFT_SN();
char *HW_Get_MFT_Rev();
char *HW_Get_MFT_Model();
uint8_t HW_GetBatteryLevel();
void HW_GetUniqueId( uint8_t *id );
uint32_t HW_GetRandomSeed( );
void BoardDriverInit();