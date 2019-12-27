# LORAWAN_REGION
[See this link to know which one to use according with your country](https://www.thethingsnetwork.org/docs/lorawan/frequencies-by-country.html)

You can access this configuration in your source code by using the #define `ACTIVE_REGION`.

Advance topic - Configurations are in cores/asr650x/kernel/protocols/lorawan/lora/mac/region/Region<COUNTRY + FREQUENCY>.h

## Values
| Value | Description | #define value |
|-------|-------------|---------------|
| REGION_AS923 | Know as AS923 | LORAMAC_REGION_AS923 |
| REGION_AU915 | Know as AU915 | LORAMAC_REGION_AU915 |
| REGION_CN470 | Know as CN470 | LORAMAC_REGION_CN470 |
| REGION_CN779 | Know as CN779| LORAMAC_REGION_CN779 |
| REGION_EU433 | Know as EU433| LORAMAC_REGION_EU433 |
| REGION_EU868 | Know as EU868| LORAMAC_REGION_EU868 |
| REGION_KR920 | Know as KR920| LORAMAC_REGION_KR920 |
| REGION_IN865 | Know as IN865| LORAMAC_REGION_IN865 |
| REGION_US915 | Know as US915 | LORAMAC_REGION_US915 |
| REGION_US915_HYBRID | | LORAMAC_REGION_US915_HYBRID |

# LORAWAN_CLASS

Define when your device is expecting to receive data.

For more detail read [Classes](https://www.thethingsnetwork.org/docs/lorawan/classes.html).

You can access this configuration in your source code by using the #define `LORAWAN_CLASS`.

## Values

| Value | Description | #define value |
|-------|-------------|---------------|
| CLASS_A | Mandatory class. Bi-directional communication accepting messages from devices at anytime. After a device transmission the gateway has two windows (fixed in time) to send anything if it has to do.<p>Pro: Device control when it want to receive data. (Best power consumption control)</p> | CLASS_A |
| CLASS_B | Inherit from class A + Use scheduled receive windows for downlink (Gateway to a device) messages from the gateway.<p>Pro: Device passively listen for gateway message from time to time.</p><p>Con: Will use slighly more power than CLASS_A.</p> | CLASS_B |
| CLASS_C | Inherit from class A + Device is always listening for gateway messages except when it transmits. <p>Pro: Best device reactivity to gateway message.</p><p>Con: Device is always wakeup. (Higher power usage of all classes)</p>| CLASS_C |


# LORAWAN_NETMODE
Define how your device will authentificate itself into the network.

You can access this configuration in your source code by using the #define `LORAWAN_NETMODE`.

## Values
| Value | Description | #define value |
|-------|-------------|---------------|
| OTAA | Over The Air Authentication (Recommended, more secure) Private keys are generated dynamicaly when joining the LoRaWAN by the device. | true |
| ABP | Authentication By Personalisation. Private keys are generated once, generally not by the device, then permanently saved into the device. | false |


# LORAWAN_ADR

[Adaptive Data Rate (ADR)](https://www.thethingsnetwork.org/docs/lorawan/adaptive-data-rate.html) is a mechanism for optimizing data rates, airtime and energy consumption in the network. ADR should be enabled whenever an end device has sufficiently stable RF conditions. If RF conditions aren't stable it should be, temporarily, disabled until RF conditions are stable again.

You can access this configuration in your source code by using the #define `LORAWAN_ADR`.

## Values

| Value | #define value |
|-------|--------------|
| ON | true |
| OFF | false |


# LORAWAN_Net_Reserve

Persist LoRaWAN keys after joining it.

Either it is defined only in exemples (but actually never used) or one of the compiled library is using it.

You can access this configuration in your source code by using the #define `LORAWAN_Net_Reserve`.

## Values

| Value | Description | #define value |
|-------|-------------|---------------|
| ON | Save the network info to flash so the device doesn't need to rejoin LoRaWAN when powered off and on. | true |
| OFF || false |


# LORAWAN_AT_SUPPORT

You can access this configuration in your source code by using the #define `AT_SUPPORT`.

For more detail see [basic/lora_at](../lora_at.md).

## Values

| Value | #define value |
|-------|---------------|
| ON | 1 |
| OFF | 0 |

# LORAWAN_RGB

You can access this configuration in your source code by using the #define `LoraWan_RGB`.

For more detail see [basic/rgb](../rgb.md).

## Values

| Value | Description | #define value |
|-------|-------------|--------------|
| ACTIVE | Use the built-in RGB led to indicate the LoRa status.<ul><li>![#500000](https://placehold.it/15/500000/000000?text=+) Sending data</li><li>![#500050](https://placehold.it/15/500050/000000?text=+) Device joined another device/gateway</li><li>![#000050](https://placehold.it/15/000050/000000?text=+) Device is listening  for any incoming data. (For Class B this is the first window)</li><li>![#505000](https://placehold.it/15/505000/000000?text=+) (Class B only) Device is listening for any incoming data (2nd and last window)</li><li> ![#005000](https://placehold.it/15/005000/000000?text=+) Data received</li></ul> | 1 |
| INACTIVE || 0 |