TODO

enum BoardPowerSources USB_POWER BATTERY_POWER

/*!
 * \brief Disable interrupts
 *
 * \remark IRQ nesting is managed
 */
void  BoardDisableIrq()


/*!
 * \brief Enable interrupts
 *
 * \remark IRQ nesting is managed
 */
 void  BoardEnableIrq()

/*!
 * \brief Initializes the mcu.
 */
void BoardInitMcu()

/*!
 * \brief Resets the mcu.
 */
void BoardResetMcu()

/*!
 * \brief Initializes the boards peripherals.
 */
void BoardInitPeriph()

/*!
 * \brief De-initializes the target board peripherals to decrease power
 *        consumption.
 */
void BoardDeInitMcu()

/*!
 * \brief Gets the current potentiometer level value
 *
 * \retval value  Potentiometer level ( value in percent )
 */
uint8_BoardGetPotiLevel()
/*!
 * \brief Measure the Battery voltage
 *
 * \retval value  battery voltage in volts
 */
uint32_t BoardGetBatteryVoltage()

/*!
 * \brief Get the current battery level
 *
 * \retval value  battery level [  0: USB,
 *                                 1: Min level,
 *                                 x: level
 *                               254: fully charged,
 *                               255: Error]
 */
uint8_t BoardGetBatteryLevel()

/*!
 * Returns a pseudo random seed generated using the MCU Unique ID
 *
 * \retval seed Generated pseudo random seed
 */
uint32_t BoardGetRandomSeed();

/*!
 * \brief Gets the board 64 bits unique ID
 *
 * \param [IN] id Pointer to an array that will contain the Unique ID
 */
void BoardGetUniqueId( uint8_t *id );

/*!
 * \brief Get the board power source
 *
 * \retval value  power source [0: USB_POWER, 1: BATTERY_POWER]
 */
uint8_t GetBoardPowerSource( void );

struct BoardVersion_s
    {
        uint8_t Rfu;
        uint8_t Revision;
        uint8_t Minor;
        uint8_t Major;
    }Fields;
    uint32_t Value;
    
/*!
 * \brief Get the board version
 *
 * \retval value  Version
 */
BoardVersion_t BoardGetVersion( void );

int FLASH_update(uint32_t dst_addr, const void *data, uint32_t size);
int FLASH_read_at(uint32_t address, uint8_t *pData, uint32_t len_bytes);