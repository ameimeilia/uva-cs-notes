---
layout: default
title: Writing Machine Code
parent: Midterm 1
nav_order: 12
---
# Writing Machine Code
## Exam Question
<div>
  <img src="{{ '/images/Screen Shot 2024-02-18 at 4.03.07 PM.png' | relative_url }}" alt="Screenshot">
</div>

## Instruction Memory and Instruction Register
- takes in an 8-bit address and reads 16-bits
- mux selects between passing 1 or 2 to the adder to increment `PC`, controlled by `C0`
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-18 at 4.11.45 PM.png' | relative_url }}" alt="Screenshot" width="200">
</div>

## Hardwired Control Unit
- use as a lookup table
- takes in the icode and b values to determine the byte size of the instruction
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-18 at 4.13.57 PM.png' | relative_url }}" alt="Screenshot" width="300">
</div>

## Register Instructions
<div>
  <img src="{{ '/images/Screen Shot 2024-02-18 at 4.38.50 PM.png' | relative_url }}" alt="Screenshot">
</div>

## Main Memory Instructions
<div>
  <img src="{{ '/images/Screen Shot 2024-02-18 at 5.05.10 PM.png' | relative_url }}" alt="Screenshot">
</div>

## Exam Question
<div>
  <img src="{{ '/images/Screen Shot 2024-02-18 at 8.14.49 PM.png' | relative_url }}" alt="Screenshot">
</div>