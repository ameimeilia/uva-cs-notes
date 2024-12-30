---
layout: default
title: Fucntion Pointers 1
parent: Midterm 3
nav_order: 7
---
# Fucntion Pointers 1
## Function Pointer
- holds the memory address of the function to be executed

**Declaration and Initialization**
- general form: `[return type] (*[pointer name])([parameters])`

```C
float div (int a, int b){
	return a/b;
}

int main(){
	float (*ptr) (int, int);    // parenthesis give ptr precedence
	ptr = &div;    // & is optional

	// (* ) is optional -> ptr(10, 20)
	// *ptr(10,20); is INVALID because it invokes the function then
	// tries to dereference it
	float result = (*ptr)(10, 20);
	printf("%f", result);
}
```

**Passing a Function Pointer**

```C
#include <stdio.h>

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
```

## Generic Sort
- `qsort()` function sorts an array with `nmemb` elements of size `size` where `base` points to the start of the array

```C
void qsort(void *base, size_t nmemb, size_t size,
		   int(*compar)(const void*, const void *)); 
```


```C
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

## The Right-Left Rule
**Symbols:**
- `*` means 'pointer to' (left side)
- `[]` means 'array of' (right side)
- `()` means 'function returning' (right side)

**Steps:**
1. Find the identifier (the variable or function name) and start with ‘<\identifier> is'.
2. Check the symbols to the right of the identifier. For example, '()' means 'function returning', and '[]' means 'array of'. Continue right until there are no more symbols or you reach a right parenthesis ')'.
3. Look at the symbols to the left of the identifier. If it's a basic type (like 'int'), state it. Otherwise, use the translations above. Continue left until there are no more symbols or you reach a left parenthesis '(‘. 
4. Repeat steps 2 and 3 as necessary

**Examples**

```C
int *p[];    // "p is an array of pointers to ints"

// "func is a function returning a pointer to a function returning a pointer to an int"
int *(*func())();

// implementation
int *myInt(){
	int* a = malloc(size(int));
	return a;
}

int *(*func())() = myInt;
return func;

// "fun_one is a pointer to a function expecting (char *, double) and returning pointer to array (size 9) of array (size 20) of int"
int (*(*fun_one)(char *, double))[9][20];

```
