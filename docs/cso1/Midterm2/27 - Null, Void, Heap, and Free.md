---
layout: default
title: Null, Void, Heap, and Free
parent: Midterm 2
nav_order: 12
---
# Null, Void, Heap, and Free
## Null Pointer

```C
#include<stdio.h>
#define NULL 0

int main)(){
	int *ptr = NULL;
	printf("%d", *ptr);    // dereferencing NULL can crash the program
	return 0;
}

```
## Void Pointer
- pointer not associated with a data type
- can’t be dereferenced, need to cast

```C
int a = 7;
char x = 'y';

void *p = NULL;
p = &a;
p = &x;    // VALID!

char q = *p;    // not allowed

// cast p to a char*
// derefence to get char
char q = *((char*)p);
```
## Stack vs Heap
- heap can be explicitly managed using functions

**malloc**
`void* malloc(size_t size)`
- size is the number of bytes to allocate
- since the return type is a void pointer, it must be cast when called
- ex. `int * a = (int *)malloc(sizeof(int) * 8)`
<div>
  <img src="{{ '/images/Screen Shot 2024-04-02 at 1.41.50 PM.png' | relative_url }}" alt="Screenshot">
</div>

**calloc**
`void* calloc(size_t nmemb, size_t size);`
- takes in the number of elements and the size of each one
- sets memory to 0
<div>
  <img src="{{ '/images/Screen Shot 2024-04-02 at 1.42.06 PM.png' | relative_url }}" alt="Screenshot">
</div>

**realloc**
`void* realloc(void *ptr, size_t size);`
- takes in a pointer to the memory to resize and the new size in bytes
- memory in newly reserved space is not set
<div>
  <img src="{{ '/images/Screen Shot 2024-04-02 at 1.42.20 PM.png' | relative_url }}" alt="Screenshot">
</div>

## Free
`void free(void *ptr);`
- dangerous because `y` can still access the space
- only works for space on the heap
<div>
  <img src="{{ '/images/Screen Shot 2024-04-02 at 3.37.06 PM.png' | relative_url }}" alt="Screenshot">
</div>

- to fix: set pointer to null once freed

```C
free(p);
p = NULL;
```
