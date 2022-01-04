# Joypad Input

## FF00 - P1/JOYP - Joypad (R/W)

The eight Game Boy action/direction buttons are arranged as a 2x4
matrix. Select either action or direction buttons by writing to this
register, then read out the bits 0-3.

```
Bit 7 - Not used
Bit 6 - Not used
Bit 5 - P15 Select Action buttons    (0=Select)
Bit 4 - P14 Select Direction buttons (0=Select)
Bit 3 - P13 Input: Down  or Start    (0=Pressed) (Read Only)
Bit 2 - P12 Input: Up    or Select   (0=Pressed) (Read Only)
Bit 1 - P11 Input: Left  or B        (0=Pressed) (Read Only)
Bit 0 - P10 Input: Right or A        (0=Pressed) (Read Only)
```

::: tip NOTE

When using the joypad, the buttons bounce off the switches inside the Game
Boy. In order to prevent false readings, most programs use a debounce routine.

A basic debounce routine is to allow a short time for readings to settle after
making a selection. It is common to do this by reading multiple times from
this port (the first reads generate enough delay for the input to stabilize,
but only the final read is actually used). Then, compare the reading to a
previous reading using `XOR` and keep the changed buttons using `AND`.

:::

## Usage in SGB software

Beside for normal joypad input, SGB games mis-use the joypad register to
output SGB command packets to the SNES, also, SGB programs may read out
gamepad states from up to four different joypads which can be connected
to the SNES. See SGB description for details.
