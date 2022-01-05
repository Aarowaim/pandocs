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

Most programs read from this port several times in a row (the first reads are
used as a short delay, allowing the inputs to stabilize, and only the value
from the last read is actually used). It is recommended to allow an interval
of 2 additional reads after selecting the direction buttons, and 6 additional
reads after selecting the action buttons.

:::

## Usage in SGB software

Beside for normal joypad input, SGB games mis-use the joypad register to
output SGB command packets to the SNES, also, SGB programs may read out
gamepad states from up to four different joypads which can be connected
to the SNES. See SGB description for details.

## Debounce Routines

When using the joypad, the buttons bounce off the switches inside the Game
Boy. In order to prevent false readings, many systems use a debounce routine.
Due to the Game Boy's slow clock speeds and typical game genres, debounce may
not always prove necessary.

::: tip NOTE

Interrupts from the joypad are filtered by a 4 sample hardware debouncer to
reduce spurious interrupts.

Due to flaws in DMG CPU B, it is effectively a 2 sample debouncer at 1/4 the
intended sample rate.

:::

Bounce will be most noticeable in games with long animations or quicktime
events. In genres like platformers, movement actions can be undone quickly
enough that a few bounces may not matter to gameplay.

A basic debounce routine has these stages:

1. Select the buttons that you will read and allow a short interval for their
accumulated capacitive charge to bleed out, and for input circuitry to
stabilize.
2. Read the input.
3. Compare the reading with one or more previous values to suppress transient
electromagnetic interference. Use `or` on raw input (0 is a pressed
button), or `and` if you use `cpl`.
4. Save the input for the next call to the routine. When using `xor` to detect
transitions, take care to save the original, unXORed reading first.
