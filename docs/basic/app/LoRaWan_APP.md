File that put together everything a programmer need to use the LoRaWAN easily.

The programmer just need to implement his logic by handling states trought the DeviceState variable in the `loop()` implementation.

See the LoRa/exemples/LoRaWan as one of the exemple of how to use it.

# Requirements
* You must call `BoardInitMcu();` in your `setup();`
* #include "LoRaWan_APP.h"
* Set variables values (to be able to transmit with the gateway)
  * [DevEui](#uint8_t-DevEui[]), [AppEui](#uint8_t-AppEui[]) & [AppKey](#uint8_t-AppKey[]) (if you use [OTAA](../arduino/ide_configurations.md#LORAWAN_NETMODE)).
  * [NwkSKey](#uint8_t-NwkSKey[]), [AppSKey](#uint8_t-AppSKey[]) & [DevAddr](#uint32_t-DevAddr[]) (if you use [ABP](../arduino/ide_configurations.md#LORAWAN_NETMODE)).

# DeviceState states
## DEVICE_STATE_INIT
Should be the initial state set by the programmer in the `setup()`.

Mean the LoRaWAN hasn't been initialized yet and the programmer should call [LoRaWAN.Init()](#void-Init(DeviceClass_t-class,-LoRaMacRegion_t-region)) with [class](#DeviceClass_t-CLASS) and [region](#LoRaMacRegion_t-REGION).

Programmer should define a `DeviceClass_t CLASS = LORAWAN_CLASS;` and `LoRaMacRegion_t REGION = ACTIVE_REGION;` variables ([LORAWAN_CLASS](../arduino/ide_configurations.md#LORAWAN_CLASS) and [ACTIVE_REGION](../arduino/ide_configurations.md#LORAWAN_REGION) #define come from [Arduino board configurations](basic/arduino/ide_configurations.md)). This is mandatory if you want to use [LoRaWAN.Ifskipjoin()](#void-Ifskipjoin()).

Then call `LoRaWAN.Init(CLASS, REGION);` in the `DEVICE_STATE_INIT` state implementation.

State will change rightaway to [DEVICE_STATE_JOIN](#DEVICE_STATE_JOIN) after calling [LoRaWAN.Init()](#void-Init(DeviceClass_t-class,-LoRaMacRegion_t-region)).

TODO: Could be mandatory somewhere else? Found reference into CubeCell.a

## DEVICE_STATE_JOIN
The [LoRaWAN variable](#LoRaWanClass-(LoRaWAN-variable)) is initialized but not yet connected to a gateway.

Programmer should call [LoRaWAN.Join()](#void-Join()).

State will change to [DEVICE_STATE_SLEEP](#DEVICE_STATE_SLEEP), [DEVICE_STATE_CYCLE](#DEVICE_STATE_CYCLE) or [DEVICE_STATE_SEND](#DEVICE_STATE_SEND) right after calling [LoRaWAN.Join()](#void-Join()).

## DEVICE_STATE_SEND
LoRaWAN is ready to send a frame (message).

Programmer can set [AppDataSize](#uint8_t-AppDataSize) (the size (in bytes) of the payload) and set [AppData](#uint8_t-AppData[LORAWAN_APP_DATA_MAX_SIZE]) (a `uint8_t` array) with the payload (up to [LORAWAN_APP_DATA_MAX_SIZE](#LORAWAN_APP_DATA_MAX_SIZE) bytes (64 by default) - 13 bytes for LoRaWAN) then call [LoRaWAN.Send()](#void-Send()).

TODO: Send() doesn't explicitly update the device state, programmer need to change it to DEVICE_STATE_CYCLE or is the .a library doing it? It call some undefined methods.

## DEVICE_STATE_CYCLE
LoRaWAN has data to send but it hasn't yet been scheduled to be send.

Programmer should call:

    TxDutyCycleTime = APP_TX_DUTYCYCLE + randr( 0, APP_TX_DUTYCYCLE_RND );
    LoRaWAN.Cycle(TxDutyCycleTime);
    DeviceState = DEVICE_STATE_SLEEP;

[TxDutyCycleTime](#uint32_t-TxDutyCycleTime) is in millisecondes. By default APP_TX_DUTYCYCLE is (TODO; probably in the lib) [APP_TX_DUTYCYCLE_RND](#APP_TX_DUTYCYCLE_RND) is 1000 (1sec).

TODO: Cycle() doesn't explicitly update the device state, programmer need to change it to DEVICE_STATE_SLEEP or is the .a library doing it? It call some undefined methods.

## DEVICE_STATE_SLEEP
LoRaWAN is waiting or/and sending/receiving data.
Programmer could do some processing or/and call [LoRaWAN.Sleep()](#void-Sleep()) to put the device in a sleep mode.

The MCU set up interrupt to wake it up at the right.

TODO: I guess there will be no sleep state with CLASS_C? (or mostly not) so it is wrong to only do programmer processing in this state?

# LoRaWanClass (LoRaWAN variable)
## Methods
### void Init(DeviceClass_t class, LoRaMacRegion_t region)
Set the [class](https://www.thethingsnetwork.org/docs/lorawan/classes.html) and [region](https://www.thethingsnetwork.org/docs/lorawan/frequencies-by-country.html) to use.

Calling this method only set members in the class, like a class constructor.

Programmer should define a `DeviceClass_t CLASS = LORAWAN_CLASS;` and `LoRaMacRegion_t REGION = ACTIVE_REGION;` variables ([LORAWAN_CLASS](../arduino/ide_configurations.md#LORAWAN_CLASS) and [ACTIVE_REGION](../arduino/ide_configurations.md#LORAWAN_REGION) #define come from [Arduino board configurations](basic/arduino/ide_configurations.md) then use) then use [CLASS](#DeviceClass_t-CLASS) and [REGION](#LoRaMacRegion_t-REGION) variable as the `Init()` parameters.

This method should be called from [DEVICE_STATE_INIT](#DEVICE_STATE_INIT) state.

State will change rightaway to [DEVICE_STATE_JOIN](#DEVICE_STATE_JOIN) after calling `Init()`.

### void Join()
Connect to a gateway.

This method should be called from [DEVICE_STATE_JOIN](#DEVICE_STATE_JOIN) state.

State will change to [DEVICE_STATE_SLEEP](#DEVICE_STATE_SLEEP), [DEVICE_STATE_CYCLE](#DEVICE_STATE_CYCLE) or [DEVICE_STATE_SEND](DEVICE_STATE_SEND) right after calling Join().

### void Send()
Tell the MCU that the payload has been generated (by setting both [AppDataSize](#uint8_t-AppDataSize) (number of bytes to send) and [AppData](#uint8_t-AppData[LORAWAN_APP_DATA_MAX_SIZE]) (an `uint8_t` array) up to [LORAWAN_APP_DATA_MAX_SIZE](#LORAWAN_APP_DATA_MAX_SIZE) in size. By default [LORAWAN_APP_DATA_MAX_SIZE](#LORAWAN_APP_DATA_MAX_SIZE) is 64).

This method should be called from [DEVICE_STATE_SEND](#DEVICE_STATE_SEND) state.

Once you call `LoRaWan.Send()` you must not call it again until the payload is send which should match the [DEVICE_STATE_SEND](#DEVICE_STATE_SEND) state.

TODO: Send() doesn't explicitly update the device state, programmer need to change it to DEVICE_STATE_CYCLE or is the .a library doing it? It call some undefined methods.

TODO: Shouldn't use AppData, AppDataSize nor Send() until we are in the DEVICE_STATE_SEND state again? Probably a shared buffer with the LoRa engine.

### void Cycle(uint32_t dutycycle)
Schedule (in millisecondes in the `dutycycle` parameter) when the next payload will be send.

Programmer should call this method in [DEVICE_STATE_CYCLE](#DEVICE_STATE_CYCLE) like so:

    TxDutyCycleTime = APP_TX_DUTYCYCLE + randr( 0, APP_TX_DUTYCYCLE_RND );
    LoRaWAN.Cycle(TxDutyCycleTime);
    DeviceState = DEVICE_STATE_SLEEP;

TODO: Cycle() doesn't explicitly update the device state, programmer need to change it to DEVICE_STATE_SLEEP or is the .a library doing it? It call some undefined methods.

### void Sleep()
Put the device on sleep mode (reduce the power consumption).

Will wakeup when needed to send pending data.

### void Ifskipjoin()

See [PassthroughMode](#bool-PassthroughMode), [Mode_LoraWan](#bool-Mode_LoraWan) & [#define LORAWAN_Net_Reserve](../ide_configurations.md#LORAWAN_Net_Reserve)

Initialize (call [LoRaWAN.Init()](#void-Init(DeviceClass_t-class,-LoRaMacRegion_t-region)) then rejoin a network using the same keys as previously aquired and then send an alive package.

Programmer **MUST** define a `DeviceClass_t CLASS = LORAWAN_CLASS;` and `LoRaMacRegion_t REGION = ACTIVE_REGION;` variables ([LORAWAN_CLASS](../arduino/ide_configurations.md#LORAWAN_CLASS) and [ACTIVE_REGION](../arduino/ide_configurations.md#LORAWAN_REGION) #define come from [Arduino board configurations](basic/arduino/ide_configurations.md) then use). This is what this method will use when calling [LoRaWAN.Init()](#void-Init(DeviceClass_t-class,-LoRaMacRegion_t-region).

If [PassthroughMode](#bool-PassthroughMode) is set to false the user has 3 secondes to set `GPIO7` `LOW` if he want to force to rejoin the network by negociating new keys (if applicable).

If [PassthroughMode](#bool-PassthroughMode) is set to true :
* It won't allow user to force renegociating the keys (if applicable) by checking `GPIO7` LOW after 3 secondes.
* It won't send an alive package if it rejoin the network.

Will set the device state to [DEVICE_STATE_SLEEP](#DEVICE_STATE_SLEEP) if it used the saved LoRaWAN keys.

Should be called in the `setup()` or in the [DEVICE_STATE_INIT](#DEVICE_STATE_INIT). In the last case you should

# Variables
## uint8_t AppData[LORAWAN_APP_DATA_MAX_SIZE]
User application data.

This is what you want to send to the device.

You must also set [AppDataSize](#uint8_t-AppDataSize) then you can call [LoRaWAN.Send()](#void-Send()).

By default `LORAWAN_APP_DATA_MAX_SIZE` is 64.

Once you call [LoRaWAN.Send()](#void-Send()) you must not edit AppData until the payload is send which should match the [DEVICE_STATE_SEND](#DEVICE_STATE_SEND) state.

## uint8_t AppDataSize
User application data ([AppData](#uint8_t-AppData[LORAWAN_APP_DATA_MAX_SIZE])) size in bytes to send to the device.

Once you call [LoRaWAN.Send()](#void-Send()) you must not edit AppDataSize until the payload is send which should match the [DEVICE_STATE_SEND](#DEVICE_STATE_SEND) state.

## uint8_t AppPort
Application specific port (from 1 to 223)

Up to the application to assign meaning.
It could be zoning, priority, category, a mix, ...

## uint32_t TxDutyCycleTime
Time (in millisecondes) to schedule the data transfert.

Should also be the value used when calling [LoRaWAN.Cycle()](#void-Cycle(uint32_t-dutycycle)) like so:
    TxDutyCycleTime = APP_TX_DUTYCYCLE + randr( 0, APP_TX_DUTYCYCLE_RND );
    LoRaWAN.Cycle(TxDutyCycleTime);

## bool OVER_THE_AIR_ACTIVATION
Value should always be `LORAWAN_NETMODE` (that match the [Arduino board configuration](basic/arduino/ide_configurations.md)).

If true mean the security authentification will use Over The Air Activation (OTAA) (Prefered method).
If false mean it will use ABP (Less secure).

See [Arduino board configuration](basic/arduino/ide_configurations.md) for more details.

## LoRaMacRegion_t REGION

Value should always be `ACTIVE_REGION` (that match the [Arduino board configuration](basic/arduino/ide_configurations.md)).

Define the country RF specifications to use.

[See this link to know which one to use according with your country](https://www.thethingsnetwork.org/docs/lorawan/frequencies-by-country.html)

## bool LORAWAN_ADR_ON

Value should always be `LORAWAN_ADR` (that match the [Arduino board configuration](basic/arduino/ide_configurations.md)).

[Adaptive Data Rate](https://www.thethingsnetwork.org/docs/lorawan/adaptive-data-rate.html) Adaptive Data Rate (ADR) is a mechanism for optimizing data rates, airtime and energy consumption in the network. ADR should be enabled whenever an end device has sufficiently stable RF conditions. If RF conditions aren't stable it should be, temporarily, disabled until RF conditions are stable again.

## bool IsTxConfirmed
Indicates if the node is sending confirmed or unconfirmed messages.

TODO: MCPS_CONFIRMED if true otherwise MCPS_UNCONFIRMED

## uint32_t APP_TX_DUTYCYCLE
Application data transmission duty cycle in millisecondes.

Default value is 15000? (15 secondes)

## DeviceClass_t CLASS
Value should always be `LORAWAN_CLASS` (that match the [Arduino board configuration](basic/arduino/ide_configurations.md)).

Define the data reception behavior.

| Class | Description |
|-------|-------------|
| A | Mandatory class. Bi-directional communication accepting messages from devices at anytime. After a device transmission the gateway has two windows (fixed in time) to send anything if it has to do.<p>Pro: Device control when it want to receive data. (Best power consumption control)</p>
| B | Inherit from class A + Use scheduled receive windows for downlink (Gateway to a device) messages from the gateway.<p>Pro: Device passively listen for gateway message from time to time.</p><p>Con: Will use slighly more power than CLASS_A.</p>
| C | Inherit from class A + Device is always listening for gateway messages except when it transmits. <p>Pro: Best device reactivity to gateway message.</p><p>Con: Device is always wakeup. (Higher power usage of all classes)</p>


## bool PassthroughMode
Only used in [LoRaWAN.Ifskipjoin()](#void-Ifskipjoin()).
Ignored if [Mode_LoraWan](#bool-Mode_LoraWan) == `false`; 

## uint8_t ConfirmedNbTrials
/*!
* Number of trials to transmit the frame, if the LoRaMAC layer did not
* receive an acknowledgment. The MAC performs a datarate adaptation,
* according to the LoRaWAN Specification V1.0.2, chapter 18.4, according
* to the following table:
*
* Transmission nb | Data Rate
* ----------------|-----------
* 1 (first)       | DR
* 2               | DR
* 3               | max(DR-1,0)
* 4               | max(DR-1,0)
* 5               | max(DR-2,0)
* 6               | max(DR-2,0)
* 7               | max(DR-3,0)
* 8               | max(DR-3,0)
*
* Note, that if NbTrials is set to 1 or 2, the MAC will not decrease
* the datarate, in case the LoRaMAC layer did not receive an acknowledgment
*/

## bool Mode_LoraWan
Default value: true

Use with [PassthroughMode](#bool-PassthroughMode) in [LoRaWAN.Ifskipjoin()](#void-Ifskipjoin()).

If set to true will use LoRaWAN otherwise will use LoRa (physical layer only without the LoRaWAN protocol)

## bool KeepNet
Value should always be [LORAWAN_Net_Reserve](../ide_configurations.md#LORAWAN_Net_Reserve) (that match the [Arduino board configuration](basic/arduino/ide_configurations.md)).

If true will save keys in `FLASH` generated from [LoRaWAN.Join()](#void-Join()) to use them again when calling [LoRaWAN.Ifskipjoin()](#void-Ifskipjoin()) instead of generating new keys.
(Only useful when [OVER_THE_AIR_ACTIVATION](#bool-OVER_THE_AIR_ACTIVATION) is set to true).

## uint8_t DevEui[]
Default value: { 0x22, 0x32, 0x33, 0x00, 0x00, 0x88, 0x88, 0x02}

Device 64 bits unique identifier (EUI-64) (big endian).

> In this application the value is automatically generated by calling BoardGetUniqueId function
-- Semtech

[Over The Air Activation (OTAA)](../arduino/ide_configurations.md#LORAWAN_NETMODE) only.

## uint8_t AppEui[]
Default value: { 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00 }

Application 64 bits unique identifier (EUI-64) (big endian)

In the LoRaWAN specification it is now named JoinEUI.

[Over The Air Activation (OTAA)](../arduino/ide_configurations.md#LORAWAN_NETMODE) only.

## uint8_t AppKey[]
Default value: { 0x88, 0x88, 0x88, 0x88, 0x88, 0x88, 0x88, 0x88, 0x88, 0x88, 0x88, 0x88, 0x88, 0x88, 0x66, 0x01 }

AES encryption/decryption cipher application key.

**MUST BE CHANGED**
**CONFIENTIAL - KEEP IT SECURE**

[Over The Air Activation (OTAA)](../arduino/ide_configurations.md#LORAWAN_NETMODE) only.

## uint8_t NwkSKey[]
Default value: { 0xd7, 0x2c, 0x78, 0x75, 0x8c, 0xdc, 0xca, 0xbf, 0x55, 0xee, 0x4a, 0x77, 0x8d, 0x16, 0xef,0x67 }

AES encryption/decryption cipher network session key.

**MUST BE CHANGED**
**CONFIENTIAL - KEEP IT SECURE**

[Activation By Personalization (ABP)](../arduino/ide_configurations.md#LORAWAN_NETMODE) only.

## uint8_t AppSKey[]
Default value: { 0x15, 0xb1, 0xd0, 0xef, 0xa4, 0x63, 0xdf, 0xbe, 0x3d, 0x11, 0x18, 0x1e, 0x1e, 0xc7, 0xda,0x85 }

AES encryption/decryption cipher application session key.

**MUST BE CHANGED**
**CONFIENTIAL - KEEP IT SECURE**

[Activation By Personalization (ABP)](../arduino/ide_configurations.md#LORAWAN_NETMODE) only.

## uint32_t DevAddr[]
Default value: 0x007e6ae1

> In this application the value is automatically generated using a pseudo random generator seeded with a value derived from BoardUniqueId value if LORAWAN_DEVICE_ADDRESS is set to 0
-- Semtech

Device address on the network (big endian)

[Activation By Personalization (ABP)](../arduino/ide_configurations.md#LORAWAN_NETMODE) only.

## DeviceState
Contains the current state of the LoRaWAN engine.

See [DeviceState states](#DeviceState-states).

# #define
# APP_TX_DUTYCYCLE_RND 
Default value: 1000 (1sec)

Maximum value to randomly select in milliseconds to add on top of [APP_TX_DUTYCYCLE](#uint32_t-APP_TX_DUTYCYCLE) when scheduling (when device status is [DEVICE_STATE_CYCLE](#DEVICE_STATE_CYCLE)) when to send data.

**NOTE**: It is up to the programmer to use both `APP_TX_DUTYCYCLE_RND` and [APP_TX_DUTYCYCLE](#uint32_t-APP_TX_DUTYCYCLE) then set the result into [TxDutyCycleTime](#uint32_t-TxDutyCycleTime) variable.

See [DEVICE_STATE_CYCLE](#DEVICE_STATE_CYCLE) device state or [LoRaWAN.Cycle()](#void-Cycle(uint32_t-dutycycle)).

# LORAWAN_APP_DATA_MAX_SIZE 
Default value: 64
> if use AT mode, don't modify this value or may run dead
-- Semtech

Keep in mind the maximum application data size vary with the Spreading Factor and that you must reserve 13 bytes for LoRaWAN.

# LORAWAN_PUBLIC_NETWORK 
Default value: true

Indicates if the end-device is to be connected to a public network.
Should be false when it is a private network.