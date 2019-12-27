TODO

enum LOG_LEVEL {
    LL_NONE,
    LL_ERR,
    LL_WARN,
    LL_DEBUG,
    LL_VDEBUG,
    LL_ALL    
}

Must #define CONFIG_DEBUG_LINKWAN in order to enable log

ERR_PRINTF(format, ...);
WARN_PRINTF(format, ...);
DBG_PRINTF(format, ...);
VDBG_PRINTF(format, ...);
PRINTF_RAW(...)
PRINTF_AT(...)
DBG_PRINTF_CRITICAL(p)

/**
 * @brief  Initializes the debug
 * @param  None
 * @retval None
 */
void DBG_Init(void);
int DBG_LogLevelGet();
void DBG_LogLevelSet(int level);