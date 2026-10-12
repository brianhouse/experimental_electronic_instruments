# Daisy board JSON: component types

Every component type available in the board JSON (`compile.json`) when compiling for the Daisy from plugdata.

Source: `hvcc/generators/c2daisy/json2daisy/resources/component_defs.json`. plugdata 0.9.3 uses hvcc 0.17.1; its list is identical to 0.17.2's except for `Switch3` output values (noted below).

Each component's name (`knob1`, `led1`, ...) becomes a send/receive name in the patch. Some components also generate extra names with suffixes (e.g. `button1_press`).


## Basic I/O

One pin: `"pin": N`

| Type | Direction | Names | Notes |
|---|---|---|---|
| `AnalogControl` | in | `name` | Pot or analog sensor, 0–1. Options: `flip`, `invert`, `slew` |
| `AnalogControlBipolar` | in | `name` | −1 to 1, for CV inputs. Option: `slew` |
| `Switch` | in | `name`, `name_press`, `name_fall`, `name_seconds` | Button or toggle. `name_seconds` = time held |
| `GateIn` | in | `name`, `name_trig` | Digital gate input. Option: `invert` |
| `Led` | out | `name` | 0–1, dimmable (software PWM). Option: `invert` |
| `GateOut` | out | `name` | Digital on/off |

```json
"knob1":   { "component": "AnalogControl", "pin": 15 },
"button1": { "component": "Switch", "pin": 17 },
"led1":    { "component": "Led", "pin": 14 }
```


## Onboard LED

No pin.

| Type | Direction | Names | Notes |
|---|---|---|---|
| `UserLed` | out | `name` | The Seed's built-in LED. On/off only (any non-zero value = on). Not yet test-compiled |

```json
"led0": { "component": "UserLed" }
```


## Multi-pin

`"pin": { "a": N, "b": N, ... }`

| Type | Direction | Pins | Names | Notes |
|---|---|---|---|---|
| `Switch3` | in | `a`, `b` | `name` | 3-position (on-off-on) switch. Outputs 0.5 (center), 1, 1.5 in plugdata 0.9.3 (hvcc 0.17.1); 0, 1, 2 from hvcc 0.17.2 |
| `Encoder` | in | `a`, `b`, `click` | `name`, `name_press`, `name_rise`, `name_fall`, `name_seconds` | Rotary encoder. `name` = turn increment |
| `RgbLed` | out | `r`, `g`, `b` | `name_red`, `name_green`, `name_blue`, `name_white`, `name` | `name` sets all three |

```json
"enc1": { "component": "Encoder", "pin": { "a": 1, "b": 2, "click": 3 } }
```


## DAC (CV out)

No pin; uses the two DAC outputs (pins 29 and 30).

| Type | Direction | Names | Notes |
|---|---|---|---|
| `CVOuts` | out | `name1`, `name2` | 0–1 in → 0–3.3V out. Control rate (main loop), not audio |


## I2C sensors

`"pin": { "scl": N, "sda": N }`

| Type | Part | Names |
|---|---|---|
| `Icm20948` | 9-DoF IMU | `name` (= accel x), `name_accel_x/y/z`, `name_gyro_x/y/z`, `name_magnet_x/y/z` |
| `Mpr121` | 12-ch capacitive touch | `name`, `name_ch0`…`name_ch11`, `name_ch0_raw`…`name_ch11_raw` |
| `Apds9960` | Gesture / proximity / color | `name`, `name_gest`, `name_prox`, `name_red`, `name_green`, `name_blue`, `name_clear` |
| `Tlv493d` | 3-axis magnetic (hall) | `name`, `name_x/y/z`, `name_amount`, `name_azimuth`, `name_polar` |
| `Dps310` | Barometric pressure | `name`, `name_temp`, `name_press`, `name_alt` |
| `NeoTrellis` | Adafruit 4×4 button pad | `name`, `name_0`…`name_15`, `name_N_falling`, `name_N_state` |
| `NeoTrellisLeds` | NeoTrellis LEDs (out) | `name`, `name_0`…`name_15`. Needs `parent` |


## Expanders

A parent component defines the chip; child components reference it with `parent` and `index`.

| Parent | Children | Use |
|---|---|---|
| `CD4051` | `CD4051AnalogControl` | 8-channel analog multiplexer — more pots than the Seed has ADC pins |
| `CD4021` | `CD4021Switch` | Shift register — more buttons |
| `PCA9685` | `PCA9685Led`, `PCA9685RgbLed` | 16-channel PWM LED driver (I2C) |
| `i2c` | — | Bus definition used by the I2C expanders |


Anything not on this list requires writing C++.
