A 16-bit-calculator(4-Bit Signed Hexadecimal) in [CircuitJS](https://www.falstad.com/circuit/)

# 4-Bit Signed Hexadecimal Calculator — CircuitJS1 Project

## Overview

This is a digital logic circuit saved in **CircuitJS1 (Falstad Circuit Simulator)** text format.  
It implements a **simple calculator for 4-bit signed hexadecimal numbers**, performing **addition and subtraction** on two's-complement operands and displaying the result.

The whole design is purely combinational/sequential logic — no microcontroller, no code.

---

## File

| File | Description |
|------|-------------|
| `circuitJS-16bit-calculator.txt` | CircuitJS1 plain-text circuit file |

To run it, open [Falstad CircuitJS1](https://www.falstad.com/circuit/), go to **File → Open**, and load the text file.

---

## What It Does

- Operands: **4-bit signed hexadecimal numbers** (range `-8 … +7`, i.e. `0x0`–`0xF` interpreted as two's complement)
- Operations: **Addition (+)** and **Subtraction (−)**
- Displays: result, bOverflow.

---

## Circuit Sections (annotated in the file)

| Section | Position | Purpose |
|---------|----------|---------|
| **Operand A display** | Left | Shows operand A in hexadecimal |
| **Operand A input controls** | Left | 16 switches for `0`–`F`, plus sign-toggle and clear |
| **Operation control** | Center-left | Add/Subtract toggle button |
| **Processing unit** | Center | `Xorer` / `Xorer1` XOR modules, adder, two's-complement logic |
| **Operand B input controls** | Right | Same as A: `0`–`F`, sign, clear |
| **Operand B display** | Right | Shows operand B in hexadecimal |
| **Result display** | Bottom-left | Converts two's complement to sign/magnitude, drives decoders |
| **Overflow detection** | Bottom-right | `Overflow` module; asserts when result exceeds signed range |

---

## Custom Sub-Circuits

Defined with `.` (subcircuit header) and instantiated with `410`:

| Name | Type | Function |
|------|------|----------|
| `Xorer` | Custom | 4-input XOR gate array (input/control XOR) |
| `Xorer1` | Custom | Variant XOR array (second operand / B-path) |
| `4-time-clock` | Counter + latches | Generates a 4-phase clock and timing signals |
| `IO-lock` | AND-gate matrix | Input/output enable / lock logic |
| `E-Display` | Display module | Result display (E), includes demux + 7-segment decoder |
| `R-Display` | Display module | Result display (R) |
| `O-Display` | Display module | Result/status display (O) |
| `Overflow` | Logic | Signed-overflow determination (truth table `001→1`, `110→1`, `???→0`) |

---

## How to Use

1. **Set operand A** — toggle the 16 switches in the A input area (hex `0`–`F`); use the sign button for negative values; use clear to reset.
2. **Set operand B** — same as A in the B input area.
3. **Choose operation** — press the button in the operation-control area to switch between addition and subtraction.
4. **Read the result** — observe the E, R, and O display blocks (hexadecimal).
5. **Check overflow** — the overflow indicator in the bottom-right section lights up when the signed result is out of the 4-bit range.

> **Tips (from the original annotations):**
> - Press buttons a little longer — the simulation update rate may otherwise miss a very short click.
> - It is recommended to enable **Options → Disable Editing** so the layout is not accidentally changed.

---

## Technical Notes

- **Number representation:** 4-bit two's complement, hex digits `0x0`–`0xF`.
- **Valid signed range:** `-8` (`0x8`) to `+7` (`0x7`).
- **Addition of two numbers** uses XOR gates for conditional inversion plus a ripple-carry adder structure.
- **Subtraction** is implemented as `A + (~B + 1)`.
- Result is converted from two's complement back to sign/magnitude before being sent to the 7-segment decoders.
- All annotations in the original file are in Chinese; this README is the English counterpart.
