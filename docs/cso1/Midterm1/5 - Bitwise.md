---
layout: default
title: Bitwise 2
parent: Midterm 1
nav_order: 4
---
# Bitwise 2
## Bitwise Operators
- compare each bit of the first operand to the corresponding bit of the second operand

| & | \| | ^ | ~ |
| :------------------------------------------------------------: | :-------------------------------------------------------------: | :--------------------------------------------------------------: | :------------------------------------------------------------: |
| <div style="text-align: center;"><img src="{{ '/images/Screen Shot 2024-01-29 at 2.07.37 PM.png' | relative_url }}" alt="Screenshot" width = 150></div> | <div style="text-align: center;"><img src="{{ '/images/Screen Shot 2024-01-29 at 2.09.20 PM.png' | relative_url }}" alt="Screenshot" width = 150></div> | <div style="text-align: center;"><img src="{{ '/images/Screen Shot 2024-01-29 at 2.07.37 PM 1.png' | relative_url }}" alt="Screenshot" width = 150></div> | <div style="text-align: center;"><img src="{{ '/images/Screen Shot 2024-01-29 at 2.22.39 PM.png' | relative_url }}" alt="Screenshot" width = 150></div> |
| <div style="text-align: center;"><img src="{{ '/images/Screen Shot 2024-02-01 at 1.55.28 PM.png' | relative_url }}" alt="Screenshot" width = 150></div> | <div style="text-align: center;"><img src="{{ '/images/Screen Shot 2024-02-01 at 1.55.59 PM.png' | relative_url }}" alt="Screenshot" width = 150></div>  | <div style="text-align: center;"><img src="{{ '/images/Screen Shot 2024-02-01 at 1.56.20 PM.png' | relative_url }}" alt="Screenshot" width = 150></div>  | <div style="text-align: center;"><img src="{{ '/images/Screen Shot 2024-02-01 at 1.56.40 PM.png' | relative_url }}" alt="Screenshot" width = 150></div> |

## Bitwise Shifts
- move the bit values of a binary object left (<<) / right (>>) x amount
- left shift = multiplying by 2
- right shift = dividing by 2
- with sign extensions (language and hardware specific), the sign bit gets copied over rather than replaced with 0’s (only applicable to right shifts)

| << | >> |
| :------------------------------------------------------------: | :------------------------------------------------------------: |
| - equal to multiplying by 2 | - equal to dividing by 2<br>- sign extension: sign bit gets copied over  rather then replaced with 0’s |
| <div style="text-align: center;"><img src="{{ '/images/Screen Shot 2024-01-29 at 2.18.08 PM.png' | relative_url }}" alt="Screenshot" width = 150></div> | <div style="text-align: center;"><img src="{{ '/images/Screen Shot 2024-01-29 at 2.17.34 PM.png' | relative_url }}" alt="Screenshot" width = 150></div> |
| <div style="text-align: center;"><img src="{{ '/images/Screen Shot 2024-02-01 at 1.41.47 PM.png' | relative_url }}" alt="Screenshot" width = 150></div> | <div style="text-align: center;"><img src="{{ '/images/Screen Shot 2024-02-01 at 1.40.25 PM.png' | relative_url }}" alt="Screenshot" width = 150></div> |

<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-23 at 12.56.02 PM.png' | relative_url }}" alt="Screenshot" width = 500>
</div>

## Setting and Flipping
Setting to 1 using the | operator:
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-01 at 2.00.20 PM.png' | relative_url }}" alt="Screenshot">
</div>

Flipping using the ^ operator:
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-01 at 2.00.49 PM.png' | relative_url }}" alt="Screenshot">
</div>

## Masking and Combining
Masking/extracting using the & operator:
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-01 at 2.01.07 PM.png' | relative_url }}" alt="Screenshot">
</div>

Combining using the | operator:
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-02-01 at 2.01.21 PM.png' | relative_url }}" alt="Screenshot">
</div>

## Parity
- either 1 (total amount of “1” bits is odd)or 0 (total amount of “1” bits is even)
