---
layout: default
title: Undefined Behavior
parent: Midterm 3
nav_order: 1
---
# Undefined Behavior
- behavior for which the International Standard imposes no requirements

**Examples**

```c
int main(){
	// behavior changes based on compiler
	
	// shift count overflow
	// when calling printf(), 1 << 32 never gets moved to %rsi
	printf("%d\n", 1 << 32);

	// integer overflow
	// INT_MAX + 1 is never calculated, a constant is moved to %rsi
	printf("%d\n", (INT_MAX + 1) < 0);

	// without optimizations: 011...11 -> 111...10 -> 111...11 = -1
	// with optimizations: INT_MAX
	x = INT_MAX * 2 / 2

	// the print statement should never execute
	// the compiler will optimize it away
	int a[5];
	a[x] = 0;
	if (x >= 5){
		printf("do we need this?");
	}
	
	// dividing by zero
	// the compiler will optimize it away
	int a /= x;
	if (x == 0){
		printf("unreachable");
	}

	// will it wrap?
	// the compiler will remove the entire statement
	int a;
	if (a + 1 > a){
		a++;
	}

	// a is never initialized, in an indeterminate state
	// compiles to ret
	int function(){
		int a;
		if (a == 0){
			return 0;
		}
		if (a != 0){
			return 0;
		}
	}
	
	return 0;
}
```

**Note on Memory**

```c
// OS doesn't always reserve memory until you write to it
// volatile tells the compiler the variable may change at any time without code
// don't asuume that values uninitialized are static/stable
volatile char * buf = malloc(1);
if (buf == NULL){
	return;
}
char c1 = buf[0];
char c2 = buf[0];
assert(c1 == c2);
```

**Promotion**

```c
void my_func(unsigned short a, unsigned short b){
	unsigned int x;
	x = (unsigned int)((int)a * (int)b);    // no adder for 16 bits, promote to int
	if (x >= 0x800000000){    // can never happen since x is unsigned
		printf("%u >= %u", x, 0x800000000);
	} else {
		printf("%u < %u", x, 0x800000000)
	}
}
my_func(656635, 65535);    // prints 4294836255 < 2147483648
```

## Padding in Structs

```c
struct name_tag{
	int y;
	char * x;    // this is the largest element size
	char z;    //size of char * is used for all other elements
}
```

<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-04-11 at 12.16.56 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>