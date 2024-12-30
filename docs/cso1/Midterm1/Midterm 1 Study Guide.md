---
layout: default
title: Midterm 1 Study Guide
parent: Midterm 1
nav_order: 16
---
# Midterm 1 Study Guide
## Boolean Algebra
**Logic Gates**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 4.39.33 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

**Transistors**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-01-22 at 12.38.55 PM.png' | relative_url }}" alt="Screenshot" width="450">
</div>

**Logic Gates with Transistors**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-01-22 at 12.47.19 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

<div>
  <img src="{{ '/images/Screen Shot 2024-01-22 at 1.03.16 PM.png' | relative_url }}" alt="Screenshot">
</div>

## Binary Arithmetic
**2-bit Adder**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 4.51.37 PM.png' | relative_url }}" alt="Screenshot" width="450">
</div>

**4-bit Adder**
- sum = `XOR`
- carry when 2 true inputs
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 4.52.03 PM.png' | relative_url }}" alt="Screenshot" width="450">
</div>

**Ripple Carry Adder**
- half-adder + full-adder + full-adder…
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-01-24 at 2.31.34 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-01-28 at 4.07.18 PM.png' | relative_url }}" alt="Screenshot" width="250">
</div>

## Signed Bitwise and Hex
**Sign Bit**: highest bit represents sign
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.01.02 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

**Bias**: middle number represents 0
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.07.44 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.10.34 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

**Twos Complement**: top most bit represents negative $-2^n$
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.14.03 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.17.42 PM.png' | relative_url }}" alt="Screenshot" width="300">
</div>

**Overflow**: adding two numbers of the same sign = the opposite sign
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-01-28 at 4.50.12 PM.png' | relative_url }}" alt="Screenshot" width="200">
</div>

**Hexadecimal**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-01-26 at 2.41.44 PM.png' | relative_url }}" alt="Screenshot" width="300">
</div>

## Bitwise Operations
**Operators**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.23.54 PM.png' | relative_url }}" alt="Screenshot" width="600">
</div>

**Shifts**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.25.44 PM.png' | relative_url }}" alt="Screenshot" width="600">
</div>

**Setting**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.29.27 PM.png' | relative_url }}" alt="Screenshot" width="450">
</div>

**Flipping**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.29.45 PM.png' | relative_url }}" alt="Screenshot" width="450">
</div>

**Masking**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.30.08 PM.png' | relative_url }}" alt="Screenshot" width="450">
</div>

**Combining**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.30.28 PM.png' | relative_url }}" alt="Screenshot" width="450">
</div>

**Parity**
- `1` = total # of `1` bits is odd
- `0` = total # of `1` bits is even

## Endian and Floating Point
**Endianness**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-01-29 at 2.38.16 PM.png' | relative_url }}" alt="Screenshot" width="450">
</div>

**Floating Point**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 7.55.08 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 8.02.57 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 8.03.09 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

## ISA Components
**Muxes**
<div>
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.39.09 PM.png' | relative_url }}" alt="Screenshot">
</div>

**Mux as a Look-up Table**
<div style="text-align: center;">
  <img src="{{ '/images/Screenshot 2024-09-12 at 9.55.05 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

**Clocks**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.44.31 PM.png' | relative_url }}" alt="Screenshot" width="350">
</div>

**Flip-Flops**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.49.06 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

**3-bit Counter Using Registers/Flip-Flops**
<div>
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.51.09 PM.png' | relative_url }}" alt="Screenshot">
</div>

**Demux**
<div>
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.53.23 PM.png' | relative_url }}" alt="Screenshot">
</div>

**Register File**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.54.41 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

**Reading and Writing to a Register File**
<div>
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.57.31 PM.png' | relative_url }}" alt="Screenshot">
</div>

**ALU**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 6.21.24 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

**Additional Memory Components**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 6.02.29 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

**Instruction Memory and Instruction Register**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 6.50.56 PM.png' | relative_url }}" alt="Screenshot" width="300">
</div>

**Hardwired Control Unit**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 6.52.17 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

## ISA Code
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 6.24.01 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

**Register Instructions**
<div>
  <img src="{{ '/images/Screen Shot 2024-02-18 at 4.38.50 PM.png' | relative_url }}" alt="Screenshot">
</div>

**Main Memory Instructions**
<div>
  <img src="{{ '/images/Screen Shot 2024-02-18 at 5.05.10 PM.png' | relative_url }}" alt="Screenshot">
</div>

**icode 5**
- `b = 0, 1, 2` can be executed with the `ALU`
- data path is long, make sure clock is slow enough for all data to propagate
<div>
  <img src="{{ '/images/Screen Shot 2024-02-18 at 8.29.30 PM.png' | relative_url }}" alt="Screenshot">
</div>

- `b = 3` can be executed by adding `PC` as an input to the mux leading to `WD3`
<div>
  <img src="{{ '/images/Screen Shot 2024-02-18 at 9.02.44 PM.png' | relative_url }}" alt="Screenshot">
</div>

**icode 6**
- `b = 0` can be executed by adding `IM` as an input to the mux leading to `WD3`
<div>
  <img src="{{ '/images/Screen Shot 2024-02-18 at 9.04.44 PM.png' | relative_url }}" alt="Screenshot">
</div>

- `b = 3` can be executed by adding a mux to the address input of the data memory
<div>
  <img src="{{ '/images/Screen Shot 2024-02-18 at 9.06.34 PM.png' | relative_url }}" alt="Screenshot">
</div>

- `b = 1, 2` can be executed by adding a mux to the second input of the `ALU`
<div>
  <img src="{{ '/images/Screen Shot 2024-02-18 at 9.10.45 PM.png' | relative_url }}" alt="Screenshot">
</div>

**icode 7**
<div>
  <img src="{{ '/images/Screen Shot 2024-02-18 at 9.23.43 PM.png' | relative_url }}" alt="Screenshot">
</div>

**Processing Stages**
<div>
  <img src="{{ '/images/Screen Shot 2024-02-18 at 9.32.32 PM.png' | relative_url }}" alt="Screenshot">
</div>

## Functions, Push, and Pop
**Functions**
- save `PC + 2` to be accessed by `ret` (return instruction), set `PC = M\[PC + 1\]`
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-18 at 9.37.02 PM.png' | relative_url }}" alt="Screenshot" width="600">
</div>

**The Stack**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 6.30.13 PM.png' | relative_url }}" alt="Screenshot" width="450">
</div>

**ISA extended R=1**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-25 at 10.36.11 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>
1. `R2` = first parameter
2. `R3` = second parameter
3. `R0` = return value of the function
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-18 at 9.59.09 PM.png' | relative_url }}" alt="Screenshot" width="300">
</div>

**Recursive Function Example**
<div>
  <img src="{{ '/images/Screen Shot 2024-02-25 at 11.05.33 PM.png' | relative_url }}" alt="Screenshot">
</div>

<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-25 at 11.52.11 PM.png' | relative_url }}" alt="Screenshot" width="200">
</div>
<div>
  <img src="{{ '/images/Screen Shot 2024-02-25 at 11.51.26 PM.png' | relative_url }}" alt="Screenshot">
</div>
