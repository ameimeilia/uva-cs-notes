---
layout: default
title: Code and Circuits
parent: Midterm 1
nav_order: 8
---
# Code and Circuits
## Demux
- connects one input to one of the N outputs
<div style="text-align: center;">
  <img src="{{ '/images/Screenshot 2024-09-12 at 9.58.33 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

<div style="text-align: center;">
  <img src="{{ '/images/Screenshot 2024-09-12 at 9.59.29 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

## Register File
**32-Bit Register File**
- temporary storage location
- stores immediately needed variables
- allows you to simultaneously read from two registers and write into one register
<div style="text-align: center;">
  <img src="{{ '/images/Screenshot 2024-09-12 at 10.00.04 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

**Read From a Register File**
- use muxes to read from a register file
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-07 at 5.10.23 PM.png' | relative_url }}" alt="Screenshot" width="450">
</div>

**Write to a Register File**
- use demuxes to write to a register file
- additional inputs: write enable (`WE`), write data (`WD`), address of register to be written to (`A3`)
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-07 at 5.17.42 PM.png' | relative_url }}" alt="Screenshot" width="450">
</div>

## Additional Memory Components
**Program Counter**
- to track where we are in a program
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-05 at 2.31.57 PM.png' | relative_url }}" alt="Screenshot" width="200">
</div>
<div style="text-align: center">
  <img src="{{ '/images/Screen Shot 2024-02-05 at 2.31.19 PM.png' | relative_url }}" alt="Screenshot" width="250">
</div>

**Instruction Memory**
- stores the program
- read data (`RD`) for a given address (`A`)
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-07 at 6.17.22 PM.png' | relative_url }}" alt="Screenshot" width="150">
</div>

**Data Memory**
- contains data needed by the program
- read data (`RD`) for a given address (`A`)
- write data (`WD`) for a given address (`A`)
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-07 at 6.24.47 PM.png' | relative_url }}" alt="Screenshot" width="150">
</div>