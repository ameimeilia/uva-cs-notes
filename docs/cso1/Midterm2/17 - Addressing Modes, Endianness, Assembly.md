---
layout: default
title: Addressing Modes, Endianness, Assembly
parent: Midterm 2
nav_order: 2
---
# Addressing Modes, Endianness, Assembly
- the stack will start after the kernel

## AT&T Syntax
- suffixes determine how many bytes are processed
- instructions that change <32 bits preserves the rest of the register
- note: X86 will truncate if the constant is larger than the destination
- `$`: constant
- `%`: register
- no `$`: memory address
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-28 at 2.07.48 PM.png' | relative_url }}" alt="Screenshot" width="250">
</div>

<div style="text-align: 300]]   ![[Screen Shot 2024-02-28 at 2.11.37 PM.png;">
  <img src="{{ '/images/Screen Shot 2024-02-28 at 2.11.03 PM.png' | relative_url }}" alt="Screenshot" width="360">
</div>

## Assembly
- Assembly is more precise than C
<div>
  <img src="{{ '/images/Screen Shot 2024-02-28 at 2.19.51 PM.png' | relative_url }}" alt="Screenshot">
</div>
<div>
  <img src="{{ '/images/Screen Shot 2024-02-28 at 2.20.02 PM.png' | relative_url }}" alt="Screenshot">
</div>
<div>
  <img src="{{ '/images/Screen Shot 2024-02-28 at 2.20.17 PM.png' | relative_url }}" alt="Screenshot">
</div>

<div>
  <img src="{{ '/images/Screenshot 2024-09-12 at 10.05.20 PM.png' | relative_url }}" alt="Screenshot">
</div>
<div>
  <img src="{{ '/images/Screenshot 2024-09-12 at 10.05.50 PM.png' | relative_url }}" alt="Screenshot">
</div>
<div>
  <img src="{{ '/images/Screenshot 2024-09-12 at 10.06.11 PM.png' | relative_url }}" alt="Screenshot">
</div>

## GUI
<div>
  <img src="{{ '/images/Screen Shot 2024-03-05 at 12.23.01 AM.png' | relative_url }}" alt="Screenshot">
</div>
<div>
  <img src="{{ '/images/Screen Shot 2024-03-05 at 12.23.11 AM.png' | relative_url }}" alt="Screenshot">
</div>

## Bracket Syntax
- brackets `()` represent value in memory
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-03-05 at 12.24.33 AM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

## Computed Addresses
- **displacement(base, index, scale) = base + (index * scale) + displacement**
- base = starting point in memory
- scale = size of value (limited to 1, 2, 4, 8)
- displacement and base are signed
<div style="text-align: center;">
  <img src="{{ '/images/Screenshot 2024-09-12 at 10.02.28 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

<div style="text-align: center;">
  <img src="{{ '/images/Screenshot 2024-09-12 at 10.01.28 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>