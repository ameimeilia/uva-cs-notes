---
layout: default
title: Type Def and Structs
parent: Midterm 2
nav_order: 11
---
# Type Def and Structs
## Array of Strings
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-04-02 at 12.33.00 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

<div>
  <img src="{{ '/images/Screen Shot 2024-04-02 at 12.34.20 PM.png' | relative_url }}" alt="Screenshot">
</div>
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-04-02 at 12.34.58 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

## Array Access

```c
strings[1][2] <-> (*(strings + 1))[2] <-> *((*(strings + 1)) + 2)

OR

char **x = strings;
x[1][2] <-> (*(x + 1))[2] <-> *((*(x + 1)) + 2)
```
- accesses the letter at index 2 of the word at index 1

## Two-Dimensional Arrays
- arrays must be stored in 1-dimension
- **row-major ordering**: rows are stored stacked one after the next
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-04-02 at 12.45.45 PM.png' | relative_url }}" alt="Screenshot" width="400">
</div>

## Type Def
- allows you to provide an alias for a type
- `typedef [type_info] [new_name];`

**Example**

```c
typedef int dimension;

dimension height = 3;
dimension width = 4;

dimension area = width * height
printf("%d\n", area);
```

```c
typedef char* string;

string name = "Daniel";
string AI = "maybe";

printf("%s\n, name")
```
## Structs
- composite data type that allows you to group variables of different data types under a single name

**Format**

```c
struct struct_name {    //struct_name is optional
	data_type member1;
	data_type member2;
	// more members
};    //remember semicolon!!!
```

**Example**

```c
struct student {
	int year;
	float grade;
};

struct student daniel;    // OR use typedef
// typedef struct student studentType
daniel.year = 4;
daniel.graded = 77.7;
```

**Combining typedef and struct**

```c
typedef struct student {
	int year;
	float grade;
} studentType;

// further optimized to:

typedef struct {
	int year;
	float grade;
} studentType;
```

**Structs and Pointers**
- `(*pointer).member` can be written as `pointer->member`

```c
typedef struct {
	char name[50];
	float grade;
} studentType;

studentType daniel;
studentType *pointer = &daniel;

pointer->grade = 77.7;
float grade = pointer->grade;
printf("grade %0.2f", grade);
```

**Structs in Memory**
- structs are represented as a block of memory big enough to hold all fields
- fields are ordered according to the order they appear in code
- type information is lost
<div>
  <img src="{{ '/images/Screen Shot 2024-04-02 at 1.12.18 PM.png' | relative_url }}" alt="Screenshot">
</div>
