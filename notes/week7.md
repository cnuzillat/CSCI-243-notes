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

Double Buffering
  - Two OS buffers
  - Load two blocks instead of just one
  - Load another block into one of the buffers when you're done with it

Stdio.h library is build on top of system calls

I/O connections are called streams
  - Two types of streams:
    - Binary stream (byte oriented - raw bytes of data)
      - No interpretation done by the library
      - The files are unstructured
    - Text stream (character oriented)
      - Each byte is represents a character
      - Files are structured

Program Address Space (program buffer <--> stdio buffer) <--> OS Address Space (Buffer) <--> Secondary Storage 
  - There are two buffers in the program address space because the OS buffer is shared
  - You want your program to rely on the OS buffer as little as possible (less system calls)

OS buffer optimzes the physical I/O (I/Os coming from the secondary storage)

Stdio buffer is used to optimize number of requests to the OS (number of system calls)

Stdio buffer is tied to program address space, so if the program crashes, everything in the buffer is lost

fflush() transfers everything from the stdio buffer to the OS buffer

fsync() transfers everything from the OS buffer to the secondary storage buffer

Can cause problems (e.g. debugging output to a file isn't flushed until the stdio buffer fills up. If the program aborts, the buffer may not actually be physically written out)

r - opening for reading (the file must exist)

w - open for writing (creates file if it doesn't exist). Deletes content and overwrites the file

a - open for appending (creates a file if it doesn't exist)

r+ - open for reading and writing (the file must exist)

w+ - open for reading and writing (if the file exists, it deletes the content and overwrites the file, otherwise it creates an empty new file)

a+ - open for reading and writing (append the file if it exists)

rb, wb, ab, rb+, wb+, ab+ - says that its binary

## Examples

bin_out.c
```
#include <stdlib.h>
#include <stdio.h<>
#include <string.h>
#include <errno.h>
#include <unistd.h>
#include <sys/stat.h>
#include <sys/types.h>

typdef struct FileData {
  double aDouble;
  int anInt;
  short aShort;
  char aChar;
} FileData_T;

int main(int argc, char * argv[]) {
  const int MAXMSG = 120;
  char emsg[MAXMSG];

  if (argc != 2) {
    fprintf(stderr, "Usage: %s ...\");
    return EXIT_FAILURE
  }
  argc--;

  FILE * fp = NULL; // stdio library uses file pointers rather than file descriptors

  struct stat statbuf;
  const size_t NAME_MAX = 512;
  char fpath[NAME_MAX];

  sprintf(fpath, "./%s", argv[argc]);

  if (stat(fpath, &statbuf) != 0) {
    if (errno != ENOENT) { // errno stores the error number whenever a system call fails
      sprintf(emsg, "stat(%s) %s\n", argv[argc], strerror(errno));
      fprintf(stderr, "%s", emsg);
      return EXIT_FAILURE;
    }
  }
  else {
    if (statbuf.st_size > 0) {
      sprintf(emsg, "Will not write non-zero file size %lu\n", statbuf.st_size);
      fprintf(stderr, "%s", emsg);
      return EXIT_FAILURE;
    }
  }
  fp = fopen(argv[argc], "wb");

  if (fp == NULL) {
    sprintf(emsg, "fopen(%s) %s", argv[argc], strerror(errno));
    fprintf(stderr, "%s", emsg);
    return EXIT_FAILURE;
  }
  printf("Will write binary content to file %s\n", argv[argc]);

  FileData_T contents[] = {
    {1.23, 391, 461, 'a'},
    {2.31, 913, 564, 'b'},
    {3.12, 139, 645, 'c'}
  };

  size_t nwrote = 0;
  size_t length = sizeof(contents);
  size_t count = sizeof(contents) / sizeof(FileData_T);

  if ((nwrote = fwrite(&count, sizeof(size_t), 1, fp)) != 1) {
    sprintf(emsg, "fwrite() %s\n", strerror(errno));
    fprintf(stderr, "%s", emsg);
    return EXIT_FAILURE;
  }
  if ((nwrote = fwrite(contents, length, count, fp)) != count) {
    sprintf(emsg, "fwrite() %s\n", strerror(errno));
    fprintf(stderr, "%s", emsg);
    return EXIT_FAILURE;
  }
  if (fclose(fp) == -1) {
    perror("fclose()"); // prints exactly why the last system call failed
  }

  return EXIT_SUCCESS;
}
```

bin_in.c
```
#include <>

typedef struct FileData {
  double aDouble;
  int anInt;
  short aShort;
  char aChar;
} FileData_T;

int main(int argc, char * argv[]) {
  const int MAXMSG = 120;
  char emsg[MAXMSG];

  if(argc < 2) {
    // usage
    return EXIT_FAILURE;
  }
  argc--;

  FILE * fp = NULL;
  fp = fopen(argv[argc]), "rb");

  if (fp == NULL) {
    sprintf(emsg, "fopen(%s) %s\n", argv[argc], strerror(errno));
    fprintf(stderr, "%s", emsg);
    return EXIT_FAILURE;
  }
  FileData_T element;

  size_t nread = 0;
  size_t length = sizeof(element);
  size_t count = 0;

  do {
    nread = fread(&count, sizeof(size_t), 1, fp);
  } while (0);
}
```
