---
layout: default
title: Push, Pop, and Functions
parent: Midterm 1
nav_order: 13
---
# Push, Pop, and Functions
## icode 5
- `b = 0, 1, 2` can be executed with the `ALU`
- data path is long, make sure clock is slow enough for all data to propagate
<div>
  <img src="{{ '/images/Screen Shot 2024-02-18 at 8.29.30 PM.png' | relative_url }}" alt="Screenshot">
</div>

- `b = 3` can be executed by adding `PC` as an input to the mux leading to `WD3`
<div>
  <img src="{{ '/images/Screen Shot 2024-02-18 at 9.02.44 PM.png' | relative_url }}" alt="Screenshot">
</div>

## icode 6
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

## icode 7
<div>
  <img src="{{ '/images/Screen Shot 2024-02-18 at 9.23.43 PM.png' | relative_url }}" alt="Screenshot">
</div>

## Processing Stages
<div>
  <img src="{{ '/images/Screen Shot 2024-02-18 at 9.32.32 PM.png' | relative_url }}" alt="Screenshot">
</div>

## Functions
- the location of the function can be hard-coded, but the return location cannot
- the solution is to save `PC = next instruction`, then read and jump back to that location after executing the function
- save `PC + 2` to be accessed by `ret` (return instruction), set `PC = M\[PC + 1\]`
<div>
  <img src="{{ '/images/Screen Shot 2024-02-18 at 9.37.02 PM.png' | relative_url }}" alt="Screenshot">
</div>

## The Stack
- used for recursive calls
- the stack holds function states and their associated return addresses
- the stack grows to lower addresses → functions are pushed to the bottom of the stack
- `RSP` (register stack pointer) holds the location of the TOP of the stack in memory
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-18 at 9.53.09 PM.png' | relative_url }}" alt="Screenshot" width="250">
</div>

## Push and Pop Instructions
**Push**
- decrements the `RSP`
- pushes to the top of the stack

**Pop**
- returns the value at the top of the stack
- increments `RSP`