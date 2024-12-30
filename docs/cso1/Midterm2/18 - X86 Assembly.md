---
layout: default
title: X86 Assembly
parent: Midterm 2
nav_order: 3
---
# X86 Assembly
## Compilation Pipeline Overview
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-03-13 at 2.10.15 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

## From C to an Executable
**C → Assembly**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-03-13 at 2.57.36 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

**Assembly → Object File**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-03-13 at 2.58.11 PM.png' | relative_url }}" alt="Screenshot" width="450">
</div>

**Inspect Object File**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-03-13 at 2.58.46 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

**Objdump**
- note: address of the function to call and the location of the string is missing
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-03-13 at 3.02.04 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

**Object File → Executable**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-03-13 at 3.08.59 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

- addresses are now filled in
- stored in little endian
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-03-13 at 3.09.53 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

**Overview**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-03-13 at 3.11.43 PM.png' | relative_url }}" alt="Screenshot" width="600">
</div>

## Pushing and Popping with Moves
- no need to use register
<div>
  <img src="{{ '/images/Screen Shot 2024-03-14 at 11.33.03 AM.png' | relative_url }}" alt="Screenshot">
</div>
