---
layout: default
title: Midterm 3 Study Guide
parent: Midterm 3
nav_order: 11
---
# Midterm 3 Study Guide
## Undefined Behavior
**Examples of Undefined Behavior**

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

	// promotion: implicit conversion of lower->higher ranking data types
	void my_func(unsigned short a, unsigned short b){
		unsigned int x;
		// no adder for 16 bits, promote to int
		x = (unsigned int)((int)a * (int)b);
		if (x >= 0x800000000){    // can never happen since x is unsigned
			printf("%u >= %u", x, 0x800000000);
		} else {
			printf("%u < %u", x, 0x800000000)
		}
	}
	my_func(656635, 65535);    // prints 4294836255 < 2147483648
	
	return 0;
}
```

## Allocators and Header Files (unfinished)
<div>
  <img src="{{ '/images/Screen Shot 2024-04-03 at 2.22.57 PM.png' | relative_url }}" alt="Screenshot">
</div>

**Allocators**


**Header Files**



## Files Descriptors
**File Descriptors**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-04 at 7.30.45 PM.png' | relative_url }}" alt="Screenshot" width="200">
</div>
- `<fcntl.h>`: file control header file

```c
#include <stdio.h>
#include <unistd.h>

int main() {
	FILE *input_stream = fdopen(0, "r"); // File descriptor 0 is stdin
	
	// Read a line of text from the user
	char buffer[1024];
	printf("Enter a line of text: ");
	fgets(buffer, sizeof(buffer), input_stream);
	
	// Write the entered line back to stdout
	printf("You entered: %s", buffer);
	
	// Close the file descriptor (not necessary, but good practice)
	fclose(input_stream);

	FILE *output_stream = fdopen(1, "w"); // File descriptor 1 is stdout
	const char *message = "Hello, stdout!\n";
	
	// Use fwrite to write to stdout
	size_t message_length = strlen(message);
	fwrite(message, sizeof(char), message_length, output_stream);
	
	// Close the file descriptor (not necessary, but good practice)
	fclose(output_stream);
	
	return 0;
}
```

**`perror`**
- prints error message to `stderr` based on `errno` variable
- if no error, doesn’t terminate and `errno` = Success
<div>
  <img src="{{ '/images/Screen Shot 2024-05-04 at 2.44.28 PM.png' | relative_url }}" alt="Screenshot">
</div>

**`va_list`**
- variable length parameter list
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-04 at 2.56.24 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

**Linux Permissions**
user → group → other
1. read (`r`) - 4
2. write (`w`) - 2
3. execute (`x`) - 1

ex. `-rw-r–r– (6 4 4)`

## syscall, memcpy, Generic Swap
**`syscall`**
1. `%rax`: system call number to tell the kernel
2. `%rdi, %rsi, %rdx, %r10, %r8, %r9`: used for passing up to 6 arguments
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-04 at 3.40.48 PM 1.png' | relative_url }}" alt="Screenshot" width="150">
</div>

3. `syscall` instruction switches into kernel mode and invokes the system call based on the value in `%rax`
 4. the return value is placed in `%rax`, indicates success or error code

```c
.global _start
.text
_start:
	# write(1, message, 18)
	mov $1, %rax ;             // syscall number for write (1)
	mov $1, %rdi ;             // arg1, file descriptor 1 (stdout)
	lea message(%rip), %rsi ;  // arg2, load the address of the message
	mov $18, %rdx ;            // arg3, message length (18 bytes)
	syscall ;                  // perform the system call

.section .rodata ;             // Read-only data section
message: ;                     // Label for the message
	.ascii "Computer Systems 1" ;
```

<div>
  <img src="{{ '/images/Screen Shot 2024-05-04 at 3.32.11 PM 1.png' | relative_url }}" alt="Screenshot">
</div>

**`memcpy` and `memmove`**

```c
void *memcpy(void *dest, const void *src, size_t n);    // returns a pointer to dest

// can use with overlapping dest and src memory
void *memmove(void *dest, const void *src, size_t n);
```

**Generic Swap**

```c
void swap(void *data1ptr, void *data2ptr, size_t nbytes) {
	// store a copy of data1 in temp storage
	void *temp = malloc(nbytes);
	memcpy(temp, data1ptr, nbytes);

	// copy data2 to the location of data1
	memcpy(data1ptr, data2ptr, nbytes);

	//copy data1 in temporary storage to the location of data2
	memcpy(data2ptr, temp, nbytes);

	free(temp);
}
```

**`strsep`**

```c
// stringp is a pointer to the string that we want to parse
// delim is a string that contains multiple delimiters
char *strsep(char **stringp, const char *delim);
```

<div>
  <img src="{{ '/images/Screen Shot 2024-05-04 at 4.25.15 PM 1.png' | relative_url }}" alt="Screenshot">
</div>

```c
#include <stdio.h>
#include <string.h>

int main() {
	char string[] = "a,b,c,d"; // The string to be tokenized
	char *token;
	char *rest = string;
	
	while ((token = strsep(&rest, ",")) != NULL) {
		printf("%s\n", token);
	}    // prints a \n b \n c \n d \n
	
	return 0;
}
```

<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-04 at 4.32.02 PM 1.png' | relative_url }}" alt="Screenshot" width="200">
</div>

## Mini Compiler
**Overview**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-05 at 2.43.12 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

**Lex**
- input `.l` file containing regular expressions and corresponding C code actions
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-05 at 3.03.09 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>
- compile with Lex to generate a C source file `lex.yy.c`, then compile the C source file with the main program
- reads input and divides into tokens

**Yacc**
- input `.y` file containing formal grammar descriptions in Backus-Naur Form and corresponding C code actions
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-04-17 at 1.26.37 PM.png' | relative_url }}" alt="Screenshot" width="550">
</div>
- compile with Yacc to generate C source file `y.tab.c` and header file `y.tab.h`, then compile the C source file with the main program
<div>
  <img src="{{ '/images/Screen Shot 2024-04-17 at 1.41.04 PM.png' | relative_url }}" alt="Screenshot">
</div>
- reads tokens and constructs a syntax tree using bottom up parsing
<div>
  <img src="{{ '/images/Screen Shot 2024-04-17 at 2.05.20 PM.png' | relative_url }}" alt="Screenshot">
</div>
<div>
  <img src="{{ '/images/Screen Shot 2024-05-05 at 3.46.32 PM.png' | relative_url }}" alt="Screenshot">
</div>

**Code Generation**
<div>
  <img src="{{ '/images/Screen Shot 2024-04-17 at 2.10.46 PM.png' | relative_url }}" alt="Screenshot">
</div>
<div>
  <img src="{{ '/images/Screen Shot 2024-05-05 at 4.38.21 PM.png' | relative_url }}" alt="Screenshot">
</div>
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-05 at 4.46.04 PM.png' | relative_url }}" alt="Screenshot" width="">
</div>

## Function Pointers, Generic Sort
**Function Pointers**

```c
#include <stdio.h>

##### GENERAL FORM #####
float div (int a, int b){
	return a/b;
}

int main(){
	float (*ptr) (int, int);   // [return type] (*[pointer name])([parameters]) 
	ptr = &div;
	float result = (*ptr)(10, 20);
	printf("%f", result);
}

##### PASSING A FUNCTION POINTER #####
int add(int a, int b) {
	return a + b;
}

int operate(int (*func)(int, int), int x, int y) {
	return func(x, y);
}

int main() {
	int (*funcPtr)(int, int) = add;
	int result = operate(funcPtr, 5, 3);
	printf("%d\n", result);
	return 0;
}

##### RETURNING FUNCTION POINTER #####
float div(int x, int y){
	return ((float)x)/y;
}

float (*return_func())(int, int) {
	return div;
}

##### TYPEDEF AND STRUCTS #####
typedef int (*Operation)(int, int); // Define a function pointer type

// Define a struct to hold two integers and a function pointer
typedef struct {
    int a;
    int b;
    Operation op;
} Calculator;

int add(int x, int y) {
    return x + y;
}

int subtract(int x, int y) {
    return x - y;
}

int main() {
    Calculator calc1 = {5, 3, add};       
    Calculator calc2 = {10, 7, subtract}; 
    printf("Result of addition: %d\n", calc1.op(calc1.a, calc1.b));
    printf("Result of subtraction: %d\n", calc2.op(calc2.a, calc2.b));

    return 0;
}
```

**Right-Left Rule**

```c
/*  * = pointer to, [] = array of, () = function returning
 *  1. "<variable or function> is..."
 *  2. go right until no symbols or )
 *  3. go left until no symbols or (
 *  4. repeat 2-3
 */ 

int *p[];    // "p is an array of pointers to ints"

// "func is a function returning a pointer to a function returning a pointer to an int"
int *(*func())();
```

**Generic Sort**
- `qsort()` function sorts an array with `nmemb` elements of size `size` where `base` points to the start of the array

```c
void qsort(void *base, size_t nmemb, size_t size,
		   int(*compar)(const void*, const void *)); 
```

```c
#include <stdio.h>
#include <stdlib.h>

int compareInts(const void *a, const void *b) {
	return (*(int *)a - *(int *)b);    // cast the type pointer
}

int main() {
	int arr[] = {10, 5, 15, 3, 12, 7};
	int n = sizeof(arr) / sizeof(arr[0]); 
	
	// Using qsort to sort the array
	// compareInts is automatically treated as a function pointer
	qsort(arr, n, sizeof(int), compareInts);
	return 0;
}
```

## Enums, Volatile, Extern
**Enums**

```c
// enums define a set of named integer constants
// the first element in the array is implicitly assigned 0

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

**Volatile**

```c 
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

```c
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

**Bit Fields**

```c
// bit fields used to specify width of struct members
struct date {
	// d has value between 0 and 31, so 5 bits are sufficient
	unsigned int d : 5;
	// m has value between 0 and 15, so 4 bit are sufficient
	unsigned int m : 4;
	int y;
};

int main(){
	// prints "Size of date is 8 bytes" (8 bytes instead of 6 due to padding)
	printf("Size of date is %lu bytes\n", sizeof(struct date));
	struct date dt = { 31, 12, 2014 };
	
	// prints "Date is 31/12/2014"
	printf("Date is %d/%d/%d", dt.d, dt.m, dt.y); 
	return 0;
}
```

## Sockets
**Client Server Model**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-04 at 5.25.00 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

**The Process**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-04-25 at 6.09.17 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

<div>
  <img src="{{ '/images/Screen Shot 2024-05-04 at 5.20.46 PM.png' | relative_url }}" alt="Screenshot">
</div>

## Buffer Overflow
**Padding in Structs/Unions**
- aligns members for optimal memory access
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-04 at 5.35.42 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

**`gets()`, Socket and Buffer Overflow**
- `gets()` doesn’t check the bounds of the buffer → memory leaks
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-04-30 at 4.54.35 PM.png' | relative_url }}" alt="Screenshot" width="300">
</div>

- if we read from the socket then an attacker can execute arbitrary code

```c
void vulnerable_function(int sockfd) {    // dangerous!!
	char buffer[7];
	fgets(s);
}
```

<div>
  <img src="{{ '/images/Screen Shot 2024-04-30 at 4.59.36 PM.png' | relative_url }}" alt="Screenshot">
</div>
