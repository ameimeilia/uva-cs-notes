---
layout: default
title: Endian and Floating Point
parent: Midterm 1
nav_order: 6
---
# Endian and Floating Point
## Endianness
- **Little endian**: least significant byte at lowest address
- **Bit endian**: most significant byte at lowest address
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-01-29 at 2.38.16 PM.png' | relative_url }}" alt="Screenshot">
</div>

## Floating Point
- scientific notation in base 2

**Floats** have 3 parts:
1. A sign bit; 0 for positive, 1 for negative in the highest-order bit’s place
2. An exponent, represented as a biased integer, in between the sign bit and fraction
	- If all 1’s or all 0’s → the number being represented is unusual
1. A fraction, represented as a sequence of bits, in the low-order bits of the number
	- If the exponent suggested this was not the normal-case number, may have special meaning

**4 Cases of Floats:**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-01-29 at 2.47.08 PM.png' | relative_url }}" alt="Screenshot" width="600">
</div>

## Exam Questions
<div style="text-align: center;">
  <img src="{{ '/images/Screenshot 2024-09-12 at 9.53.11 PM.png' | relative_url }}" alt="Screenshot">
</div>