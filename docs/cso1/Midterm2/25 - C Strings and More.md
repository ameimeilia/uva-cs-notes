---
layout: default
title: C Strings and More
parent: Midterm 2
nav_order: 10
---
# C Strings and More
## Pointer to a Pointer
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-04-02 at 11.39.08 AM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-04-02 at 11.43.38 AM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

## String Literals in C
- `char *b = "some text";`: the string is stored in the code section, so it is not writable
- `b` is a pointer, `b` and `&b` are NOT the same
<div>
  <img src="{{ '/images/Screen Shot 2024-04-02 at 11.59.47 AM.png' | relative_url }}" alt="Screenshot">
</div>

- `char b[] = "some text";`: a copy of the string is stored on the stack
- ``b` is an array, `a` and `&a` ARE the same
<div>
  <img src="{{ '/images/Screen Shot 2024-04-02 at 12.00.01 PM.png' | relative_url }}" alt="Screenshot">
</div>

## Command Line Arguments

```C
int main(int argc, char **argv){
	if (argc > 0){
		printf("argument was %s", *argv);
	}
}
```

- the first element in the `argv` array is the name of the program itself
- when you call `./a.out Hello` it will print `a.out`

- to print `Hello`:

```C
int main(int argc, char **argv){
	if (argc > 0){
		printf("argument was %s", *(argv + 1));    // or argv[1]
	}
}
```


## Const Key Word
- the `const` keyword defines a read only section of memory
- `const int x = 10` : stores type information as opposed to `#define x 10`

## String Helper Functions

```C
size_t strlen(const char *str)
```
- `size_t` is a flexible type that stores the integer the size of the pointer (32/64)
- `size_t` is unsigned, `ssize_t` is signed
- `const char *str`: prevents the value the pointer points to from being reassigned