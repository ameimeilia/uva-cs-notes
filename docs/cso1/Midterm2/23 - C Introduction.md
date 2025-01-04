---
layout: default
title: C Introduction
parent: Midterm 2
nav_order: 8
---
# C Introduction
## C Main Entry
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-03-26 at 9.45.28 AM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

**Status Codes**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-03-26 at 9.50.13 AM.png' | relative_url }}" alt="Screenshot" width="350">
</div>

**Debugging**
- `clang -g puts.c -o puts.out`: the -g flag allows for line level debugging
- `lldb` allows you to step into a function and see the corresponding assembly

## C Syntax
**Types**
- each type has a certain size
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-03-26 at 10.05.33 AM.png' | relative_url }}" alt="Screenshot" width="150">
</div>

```c
int x = 3;
int number_of_bytes = sizeof(x);    // sizeof(x) returns 4
```

**printf**
- follows the format `printf([specifier], [argument])`
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-03-26 at 10.40.07 AM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

**Variables**
`int [variable_name];    // declares an integer variable`
- 32 bits from a 64 bit address are dedicated to storing the integer value
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-03-26 at 2.04.05 PM.png' | relative_url }}" alt="Screenshot" width="350">
</div>

- uninitialized variables are free-floating and their value may change
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-03-26 at 2.13.49 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

**Pointers**
`int *pointer = &variable;    //& gives the address of the variable`
- hold memory addresses
- uninitialized pointers are also free-floating

`int variable2 = *pointer` sets variable2 to the value the pointer points to

`*pointer = 4` goes to the memory location and sets the value to 4

## scanf and the Stack
- `scanf`: performed a formatted read
<div>
  <img src="{{ '/images/Screen Shot 2024-03-27 at 12.39.08 AM.png' | relative_url }}" alt="Screenshot">
</div>
