# C: Advanced I/O, advanced preprocessor

## I/O

OS is a collection of pieces
  - Tells you how to deal with process management (pause a process, resume a process, start a process, etc.)
  - Main memory management
  - File management
  - I/O system management
  - Networking
  - Protection

System Calls
  - Interface between the process and the OS
  - Processes do file operations using File Descriptors (FDs)
    - Really just non-negative integers
  - Several ways to get them
    - Open a file
    - Create a pipe
    - Create socket connection
    - Inherit when a process is created
  - Three FDs are usually preopen:
    - 0: standard input (STDIN_FILENO...)
    - 1: standard output
    - 2: standard error
  - Standard I/O library are not system calls, they're built on top of system calls

Fundamental I/O routines do byte-oriented I/O
  - ssize_t read(int fd, void * buffer, size_t length) // ssize_t = signed size_t - for errors, tells you how much you read successfully
  - ssize_t write(int fd, const void * buffer, size_t length)

Transfer upto "length" bytes between fd and buffer

Return value:
  - 1: error - np data transferred
  - 0: no data transferred
  - else: number of bytes "transferred"

read():
  - Returns actual count of bytes transferred
  - May be less than expected length for many reasons
    - Input from a terminal:
      - Only get upto one line of input
    - Input from a file:
      - EOF encountered before buffer is full
    - Input from a socket (something that can be used to transfer data between two machines):
      - Can return any number of bytes

write():
  - Returns count of bytes transferred into OS buffer
    - Not necessarily physically written to the device yet
    - Maybe less than the specified buffer size

sc_io_ex
```
#include <unistd.h>

#define BUF_LENGTH 512

int main() {
  char buffer[BUF_LENGTH];
  int n;

  while((n = read(STDIN_FILENO, buffer, BUF_LENGTH)) > 0 ) {
    write(STDOUT_FILENO, buffer, n);
  }

  return 0;
}
```

OS buffers all I/O
  - Increases efficiency by compensating for device latency (figures out all the changes it needs to make before actaully making them)
  - Reduces the amount of physical I/O (much faster to read in bulk when talking about, for example, the disc)

Input
  - Process issues first read 
    - First logical read causes first physical read
    - First physical read gets more data than requested into the buffer
  - On subsequent logical reads, more data is copied from buffer

Output
  - 
