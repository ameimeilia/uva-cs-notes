---
layout: default
title: Sockets
parent: Midterm 3
nav_order: 9
---
# Sockets
## Client-Server Model
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-04-25 at 5.27.56 PM.png' | relative_url }}" alt="Screenshot" width="450">
</div>

**Client Code (Python)**

```Python
import socket

def connect_server():
	host = '127.0.0.1'    # IP address of server, localhost/loopback address
	port = 8080

	# AF_INET: address family IPv4
	# SOCK_STREAM: TCP (transport), reliable, every packet sent is received
	client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
	client_socket.connect((host, port))
	message = 'Hello, server!'

	# default encoding is UTF-8
	client_socket.sendall(message.encode())

	# 1024 is the buffer to read from
	data = client_socket.recv(1024)
	print(f"Received from server: {data.decode()}")
	
	client_socket.close()
	
	if __name__ == '__main__':
		connect_server()
```

**Server Code**

```Python
import socket

def start_server():
	host = '127.0.0.1'
	port = 12345
	server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
	
	# associates current process with host and port
	server_socket.bind((host, port))

	# 5 is buffer size, listen for up to 5 connections
	server_socket.listen(5)
	print(f"Server listening on {host}:{port}")

# always ON state
while True:

	# conn is the socket, addr is the IP address and port
	conn, addr = server_socket.accept()
	print(f"Connected by {addr}")
	data = conn.recv(1024)
	if not data:
		break
	print(f"Received: {data.decode()}")
	conn.sendall(data)
	conn.close()
server_socket.close()

if __name__ == '__main__':
	start_server()
```

## Network Communications
- socket interface behaves like files → read and write to sockets
- ports: a unique number between 1-65,535 (16 bits wide) assigned to a socket
	- dynamically allocated sockets pick port numbers between 49,152-65,535
- the OS maintains the mapping between sockets and processes
- each socket on the network is uniquely identified by IP and port number
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-04-25 at 6.12.56 PM.png' | relative_url }}" alt="Screenshot" width="450">
</div>

**The Process**
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-04-25 at 6.09.17 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>

## C Code
**Imports**

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <netinet/in.h>

#define PORT 8080
```
- `<unistd.h>`: provides access to POSIX OS API, functions to make system calls to the OS, includes process control, file manipulation, I/O operations, etc.
- `<netinet/in.h>`: used for IP family socket definitions

**File Descriptors**

```c
int main() {
	int server_fd, client_fd;
	struct sockaddr_in address;
	...
```

**Create Socket**

```c
int socket(int domain, int type, int protocol);
```
- `domain`: AF_INET (IPv4 address), AF_INET6 (IPv6 address), or AF_BLUETOOTH
- `type`: TCP SOCK_STREAM (reliable) or UDP SOCK_DGRAM (unreliable)
- `protocol`: most socket types only support a single protocol, normally set to 0

```c
server_fd = socket(AF_INET, SOCK_STREAM, 0);
```

**Set Address**

```c
address.sin_family = AF_INET;
address.sin_addr.s_addr = INADDR_ANY;
address.sin_port = htons(PORT);
```
- `AF_INET`: specifies address type IPv4
- `INADDR_ANY`: specifies that the socket assumes the IP address of the machine the program is running on
- `htons(PORT)`: converts host byte order to network byte order (big-endian)

**Bind and Listen**

```c
bind(server_fd, (struct sockaddr *)&address, sizeof(address));
listen(server_fd, 10);
int addrlen = sizeof(address);
```
- `bind`: associates the IP address and port with the socket
- `listen`: param1 is the socket that will accept incoming connection requests, param2 is the buffer size

**Serve the Page**

```c
while (1) {
	// new_socket is a file descriptor (int)
	new_socket = accept(server_fd, (struct sockaddr *)&address, (socklen_t*)&addrlen);
	write(new_socket, "HTTP/1.1 200 OK\n", 16);
	write(new_socket, "Content-Type: text/html\n\n", 25);
	write(new_socket, "# Hello, World!", 44);
```

**Close File Descriptors**

```c
	close(new_socket);
}
close(server_fd);
```

## HTTP Basics
**Request**
```
GET /index HTTP/1.1\r\n
Host: www.bing.com\r\n
\r\n
```
**Response**
```
HTTP/1.1 200 OK
--- Headers ---
--- Content ----
```

## Overview
<div style="text-align: center;">
  <img src="{{ '/images/Screen Shot 2024-04-25 at 6.47.06 PM.png' | relative_url }}" alt="Screenshot" width="500">
</div>
