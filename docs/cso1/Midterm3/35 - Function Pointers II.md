---
layout: default
title: Function Pointers 2
parent: Midterm 3
nav_order: 8
---
# Function Pointers 2
## Function Pointers cont.
**Functions that Return Function Pointers**

```C
float div(int x, int y){
	return ((float)x)/y;
}

float (*return_func())(int, int) {
	return div;
}
```

**TypeDef with Function Pointers**

```C
typedef float (*function_type)(int, int);

function_type returnDivider(){
	return div;
}
```

**Structs with Function Pointers**

```C
#include <stdiol.h>
void myfunction(); //Function prototype

struct MyStruct{
	void (*funcPtr)();
};

void myFunction(){
	printf("Hello from the inside");
}

int main(){
	struct MyStruct example;
	example.funcPtr = myFunction;
	example.funcPtr();
	return 0;
}
```

## Enums
- user-defined data type that consists of a set of named integer constants
- used to define a list of named values that represent a finite set of possible values for a variable

```C
enum Name{
	Constant_1,
	Constant_2,
	...
};
```

 **Examples**

```C
#include <stdio.h>

// the first element in the array is implicitly assigned 0
// can be explicitly assigned: SUNDAY = 1
enum Day {SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY};

int main() {
	enum Day today;
	today = WEDNESDAY;
	
	if (today == WEDNESDAY) {
		printf("It's the middle of the week.\n");
	}
	
	return 0;
} 
```

```C
enum Direction {
	Up,
	Down,
	Left,
	Right
};

void move(enum Direction dir) {    // so readable!!!
	switch (dir) {
		case Up:
			// Move up
			break;
		case Down:
			// Move down
			break;
		case Left:
			// Move left
			break;
		case Right:
			// Move right
			break;
	}
}
```

## Volatile and Extern
**Volatile**

```C 
volatile int flag = 0;    // variable can get changed in another location

void interrupt_handler() {
	// This could be an interrupt handler
	// invoked by hardware
	flag = 1;
}

int main() {
	while (!flag) {
		// Waiting for the flag to be set by the interrupt
	} 
	// Proceed once the flag is set
	return 0;
}
```

**Extern**

```C
// In some other file, let's say 'file1.c'
int count = 5;

// In the current file
extern int count;    // declare a variable/function that has external linkage

int main() {
	// Now count can be used here
	printf("Count is %d\n", count);
	return 0;
}
```

## Bit Fields
- used to specify the width ≤ to that of a type
- limitations: can have pointers due to alignment/padding

```C
// data_type: int, signed int, or unsigned int.
// member_name: the name of the bit field
// width_of_bit-field: The number of bits in the bit field
struct {
	data_type member_name : width_of_bit-field;
};
```

**Example**

```C
// Space optimized representation of the date

struct date {
	// d has value between 0 and 31, so 5 bits are sufficient
	unsigned int d : 5;
	// m has value between 0 and 15, so 4 bit are sufficient
	unsigned int m : 4;
	int y;
};

int main(){
	// prints "Size of date is 8 bytes"
	printf("Size of date is %lu bytes\n", sizeof(struct date));
	struct date dt = { 31, 12, 2014 };
	
	// prints "Date is 31/12/2014"
	printf("Date is %d/%d/%d", dt.d, dt.m, dt.y); 
	return 0;
}   // the date is 8 bytes instead of 6 due to padding
```