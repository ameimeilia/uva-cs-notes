---
layout: default
title: File Descriptors and Memory Errors
parent: Midterm 3
nav_order: 3
---
# File Descriptors and Memory Errors
## File Descriptors
- non-negative integer that uniquely identifies an open file or I/O stream within a process
- OS maintains a file descriptor table for each process to keep track of open files

**`fcntl.h` Header**
- file control header file
- part of C standard library
- manipulates file descriptors and provides additional control over open files

```C
#include <fcntl.h>
#include <unistd.h>

int main() {
	char buffer[1024];
	int input_fd = open("example.txt", O_RDONLY); // returns int file descriptor
	size_t bytesRead = read(input_fd, buffer, sizeof(buffer));
	close(input_fd); // Don't forget to close the file!
	return 0;
}
```

**`0` (stdin)**
- standard input
- used for reading input from the keyboard and other input sources

**`1` (stdout)**
- standard output
- used for writing normal output (typically the console)

**`2` (stderr)**
- standard error
- used for writing error messages or diagnostic information to the console

**`fgets`**

```C
char *fgets(char *s, int size, FILE *stream);    // read from stdin
```

**fwrite**
```C
size_t fwrite(const void *ptr, size_t size, nmemb_t nmemb, FILE *stream);
```

**Example File Descriptor**

```C
#include <stdio.h>
#include <unistd.h>

int main() {
	// Assuming stdin and stdout as file descriptors
	FILE *input_stream = fdopen(0, "r"); // File descriptor 0 is stdin
	
	// Read a line of text from the user
	char buffer[1024];
	printf("Enter a line of text: ");
	fgets(buffer, sizeof(buffer), input_stream);
	
	// Write the entered line back to stdout
	printf("You entered: %s", buffer);
	
	// Close the file descriptor (not necessary, but good practice)
	fclose(input_stream);

	// Assuming stdout as a file descriptor
	FILE *output_stream = fdopen(1, "w"); // File descriptor 1 is stdout
	
	// Message to be written
	const char *message = "Hello, stdout!\n";
	
	// Use fwrite to write to stdout
	size_t message_length = strlen(message);
	fwrite(message, sizeof(char), message_length, output_stream);
	
	// Close the file descriptor (not necessary, but good practice)
	fclose(output_stream);
	
	return 0;
}
```

## perror
- library function in C that prints a descriptive error message to `stderr`
- message based on the global variable `errno` which gets set by system calls/library functions in the event of an error
<div>
  <img src="{{ '/images/Screen Shot 2024-05-04 at 2.44.28 PM.png' | relative_url }}" alt="Screenshot">
</div>

**perror Success Case**
<div>
  <img src="{{ '/images/Screen Shot 2024-05-04 at 2.45.14 PM.png' | relative_url }}" alt="Screenshot">
</div>

## va_list
- variable length parameter list
- ex. `int snprintf(char *s, size_t n, const char *format, ...)`
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-04 at 2.56.24 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

## Linux Permissions
**Types of permissions:**
1. read (`r`) - 4
2. write (`w`) - 2
3. execute (`x`) - 1

**Levels of permissions:**
1. user
2. group
3. other

- only the owner of the file can change permissions
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-05-04 at 2.54.56 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>