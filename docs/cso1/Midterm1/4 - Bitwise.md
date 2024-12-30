---
layout: default
title: Bitwise 1
parent: Midterm 1
nav_order: 4
---
# Bitwise 1
## Negative Numbers
**Sign Bit**: the highest order bit indicates the sign of the number
- doesn’t work with ripple carry adders
- used in floating-point numbers
<div>
  <img src="{{ '/images/Screen Shot 2024-01-26 at 2.08.33 PM.png' | relative_url }}" alt="Screenshot" width="300">
  <img src="{{ '/images/Screen Shot 2024-01-26 at 2.09.45 PM.png' | relative_url }}" alt="Screenshot" width="300">
</div>

**Bias**: find the middle number and have it represent 0
- doesn’t work with ripple carry adder
- addition, subtraction, and comparisons work the same for signed and unsigned numbers
- used to represent the exponents of floating-point numbers stored in binary scientific notation
<div>
  <img src="{{ '/images/Screen Shot 2024-01-26 at 2.13.32 PM.png' | relative_url }}" alt="Screenshot" width="300">
  <img src="{{ '/images/Screen Shot 2024-01-26 at 2.17.43 PM.png' | relative_url }}" alt="Screenshot" width="300">
</div>
- to find the middle: $floor( (2^n - 1) / 2 )$
- to convert from decimal to bias: add N to the decimal value then convert as if unsigned
- from original to biased: representation = original + bias
- from biased to original: original = representation - bias
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-03 at 5.10.34 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

**Twos complement**: top most bit represents negative $-2^n$
- works with ripple carry adder
- to convert from between signs:
	1. flip the bits
	2. add 1
- addition, subtraction, and multiplication work the same for signed and unsigned values
- used for signed integers
<div>
  <img src="{{ '/images/Screen Shot 2024-01-28 at 4.46.41 PM.png' | relative_url }}" alt="Screenshot" width="300">
  <img src="{{ '/images/Screen Shot 2024-01-26 at 2.21.23 PM.png' | relative_url }}" alt="Screenshot" width="300">
</div>

**Overflow**: if $\oplus + \oplus = \circleddash, or \circleddash + \circleddash = \oplus$
- carry is not considered overflow
<div>
  <img src="{{ '/images/Screen Shot 2024-01-28 at 4.50.12 PM.png' | relative_url }}" alt="Screenshot" width="200">
  <img src="{{ '/images/Screen Shot 2024-01-28 at 4.50.26 PM.png' | relative_url }}" alt="Screenshot" width="200">
</div>

## Hexadecimals
- group the binary number into nibbles, convert, then put them together
- prefix for hex is 0x
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-01-26 at 2.41.44 PM.png' | relative_url }}" alt="Screenshot" width="300">
</div>

<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-01-28 at 10.08.42 PM.png' | relative_url }}" alt="Screenshot" width="450">
</div>

## Exam Questions
<div style="text-align: center;">
  <img src="{{ '/images/Screenshot 2024-09-12 at 9.52.27 PM.png' | relative_url }}" alt="Screenshot">
</div>