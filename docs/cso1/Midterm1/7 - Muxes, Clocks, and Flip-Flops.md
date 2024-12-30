---
layout: default
title: Muxes, Clocks, and Flip-Flops
parent: Midterm 1
nav_order: 7
---
# Muxes, Clocks, and Flip-Flops
## Muxes
- selector `S` input selects between one of $N$ inputs to connect to output
- represented each input with $log_2N$ bits
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-05 at 11.56.05 AM.png' | relative_url }}" alt="Screenshot" width="600">
</div>

<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-05 at 12.25.40 PM.png' | relative_url }}" alt="Screenshot" width="600">
</div>

**Mux as a Look-up Table**
<div style="text-align: center;">
  <img src="{{ '/images/Screenshot 2024-09-12 at 9.55.05 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

## Clocks
- produces a periodic signal
- **period/cycle** = length of time for one clock cycle ($1 / frequency$)
- **frequency** = $1 / period$
- ex. Intel Core i-9 3.0GHz processor → 3,000,000,000 cycles/sec where every cycle executes one instruction
<div style="text-align: center;">
  <img src="{{ '/images/Screenshot 2024-09-12 at 9.55.57 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

**Clocks Using Ring Oscillators**
<div style="text-align: center;">
  <img src="{{ '/images/Screenshot 2024-09-12 at 9.57.21 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

## Flip-Flops
**Positive Edge-Triggered D Flip-Flop**
Inputs:
1. `D` - Data in (the single bit to be stored)
2. `Clock` - output wave determines when the values change and how long they remain

Outputs:
1. `Q` - value of the internal state
2. $\overline{Q}$ - the inverse of Q

<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-05 at 1.21.47 PM.png' | relative_url }}" alt="Screenshot" width="550">
</div>

**Registers Using Flip-Flops**
- a memory unit that stores a bit for 1+ clock cycles
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-05 at 1.56.05 PM.png' | relative_url }}" alt="Screenshot" width="550">
</div><div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-05 at 1.57.53 PM.png' | relative_url }}" alt="Screenshot" width="550">
</div>

**3-Bit Counter Using a Register + Ripple-Carry Adder**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-05 at 1.59.12 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

## Exam Questions
<div style="text-align: center;">
  <img src="{{ '/images/Screenshot 2024-09-12 at 9.58.01 PM.png' | relative_url }}" alt="Screenshot">
</div>