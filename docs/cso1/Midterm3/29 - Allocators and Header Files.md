---
layout: default
title: Allocators and Header Files
parent: Midterm 3
nav_order: 2
---
# Allocators and Header Files
<div>
  <img src="{{ '/images/Screen Shot 2024-04-03 at 2.22.57 PM.png' | relative_url }}" alt="Screenshot">
</div>

## scanf

```c
int scanf(const char *format, ...)
// ... indiciates that the function accepts variable length arguments
// char *: this contains the format specifiers
```

**Example**

```c
int a;
int b;

printf("Enter two number: ");
scanf("%d %d", &a, &b);
```

## Malloc Examples

```c
// use dynamically allocated memory on the stack to read in a file
// vs. declaring variables on the stack: might exceed/take up unnecessary space
int main(){
	int *array, i, n;
	scanf("%d", &n);
	array = (int *)malloc(n*sizeof(int));
	for(i = 0; i < n; i++){
		scanf("%d", &array[i]);
	}
}
```

<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-04-09 at 10.08.14 AM.png' | relative_url }}" alt="Screenshot" width="450">
</div>

## Allocators Implementing Malloc
Allocators:
1. can’t control the number or size of allocated blocks
2. must allocate blocks from free memory
3. can manipulate and modify only free memory
4. can’t move the allocated blocks once they are `malloc`’d

- **fragmentation**: losing space after freeing a pointer
- instead of memory addresses, use word addresses (int-sized)

**Knowing How Much Memory to Free**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-04-09 at 10.13.33 AM.png' | relative_url }}" alt="Screenshot" width="450">
</div>

**Keeping Track of Free Blocks**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-04-09 at 10.13.55 AM.png' | relative_url }}" alt="Screenshot" width="450">
</div>

**Picking a Block for Allocation**
- fragmentation: losing space after freeing a pointer
- methods (still have fragmentation):
	1. best fit
	2. next fit

## Header Files
- API that provides the method definition so we can use a library

```c
#include <filename.h>    // for files in system/default directory
#include "filename.h"    // for files in the same directory as source file
```

**Example**

```c
#ifndef EXAMPLE_HEADER_FILE
#define EXAMPLE_HEADER_FILE

/** Encrypts a string using a key
@param input_string String to be encrypted
@part key key to use for encryption. **/
#include <string.h>
	void encrypt(char * input_string, const char * key);

#endif
```
## FILE IO

```c
FILE *fopen(const char *filename, const chat *mode);
```

| mode | desc. |
| ---- | ---------------------------------------------------------------------------------------------------------------- |
| r    | Opens an existing text file for reading purposes. |
| w    | Opens a text file for writing. If it does not exist, then a new file is created. Here your program will start writing content from the beginning of the file. |
| a    | Opens a text file for writing in appending mode. If it does not exist, then a new file is created. Here your program will start appending content in the existing file content. |
| r+   | Opens a text file for both reading and writing. |
| w+   | Opens a text file for both reading and writing. It first truncates the file to zero length if it exists, otherwise creates a file if it does not exist. |
| a+   | Opens a text file for both reading and writing. It creates the file if it does not exist. The reading will start from the beginning but writing can only be appended. |

**Closing Files**

```c
int fclose(FILE *fp);
```

**Example**

```c
#include <stdio.h>
#include <stdlib.h>

int main() { 
	// file pointer variable to store the value returned by fopen
	FILE* fptr;
	
	// opening the file in read mode
	fptr = fopen("filename.txt", "r");
	
	// checking if the file is opened successfully
	if (fptr == NULL) {
		printf("The file is not opened. The program will " "now exit."); =
		exit(0);
	}
	
	return 0;
}
```