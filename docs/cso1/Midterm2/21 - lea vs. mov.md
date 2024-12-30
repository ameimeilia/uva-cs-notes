---
layout: default
title: lea vs. mov
parent: Midterm 2
nav_order: 6
---
# lea vs. mov
## Swap()
- `()` indicates the value stored at the memory of the address in the register
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-03-15 at 2.25.39 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

## leaq vs. movq
- `leaq` calculates an address and loads it into the destination
- `movq` moves content and will access memory if the source operand is a memory location
<div>
  <img src="{{ '/images/Screen Shot 2024-03-15 at 2.37.36 PM.png' | relative_url }}" alt="Screenshot">
</div>

**lea Tricks**
```
leaq (%rax, %rax, 4), %rax
rax <- rax * 5
rax <- address-of(memory[rax + rax * 4])
```

```
leaq (%rbx, %rcx), %rdx
rdx <- rbx + rcx
rdx <= address-of(memory[rbx + rcx])
```