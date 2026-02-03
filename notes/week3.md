# The OS: Memory, program layout, C: Functions, structures, strings

## Examples

define
```
#define A 5 + 5
#define B A * A

#include <stdio.h>
int main() {
  int x = B; // reads as int x = 5 + 5 * 5 + 5 to the compiler
  printf("%d\n", x);
}
```
Output:
```
35
```

Remainder Operator
```
#include <stdio.h>

int main() {
  int number = 10;
  char * result;

  result = (number > 0) ? "Positive" : "non - positive";

  printf("the number is %s\n", result);

  return 0;
}
```
Output:
```
the number is Positive
```

## Precedence and Associativity

Precedence | Associativity
  - `()` `[]` `.` `->` | left-to-right
  - `++` `--` `+` `-` `!` `~` `(type)` `*` `&` `sizeof` | right-to-left
  - `*` `/` `%` | right-to-left
  - `+` `-` | left-to-right
  - `<<` `>>` | left-to-right
  - `<` `<=` `>` `>=` | left-to-right
  - `!=` `==` | left-to-right
  - `&` | left-to-right
  - `^` | left-to-right
  - `|` | left-to-right
  - `&&` | left-to-right
  - `||` | left-to-right
  - `?:` | right-to-left
  - `+=` `-=` `*=` `/=` `%=` `&=` `^=` `|=` `<<=` `>>=` `=` | right-to-left
  - `,` | left-to-right
    - Ex. 100 + 200 / 10 - 3 * 10
    - Ex.
      ```
      int a = 10
      int b = 5
      int c = 2
      a = b = c // a = 2
      ```
    - Ex.
      ```
      int a = 50
      int b = 75
      int c = 100
      int x = a < b < c // x = true; a < b is true, so 1
      ```

```
#include <stdio.h>

int main() {
  int x = 1;
  int y = x++;

  x = 1;
  int z = ++x // increment x first, then assign that value to z

  printf("%d\n", x); // 2
  printf("%d\n", y); // 1
  printf("%d\n", z); // 2
}
```

```
#include <stdio.h>

int main() {
  int x = 1;
  int y = x++ + ++x;

  x = 1;

  // ASSEMBLY
  int z = ++x + x++; // increment x first, then assign that value to z

  printf("%d\n", x); // 3
  printf("%d\n", y); // 4
  printf("%d\n", z); // 5
}
```

## Strings

No such type

All string manipulations are done using char arrays

Trailing NUL byte (`'\0'`)
  - NUL byte is false, 0

`{'h', 'e', 'l', 'p', '\0'}` - you can have more characters after `'\0'`, but it's not part of the string

```
int length(char data[]) {
  int num = 0;
  while(data[num] != '\0') {
    num++;
  }
  return num;
}

int cpy(char dst[], char src[]) {
  int num = 0;
  while (dst[num] = src[num]) { // copies source to destination until the NUL byte
    num++;
  }
  return num;
}

int concat(char dst[], char src[]) {
  int num1, num2;

  num1 = 0;
  while (dst[num1]) {
    num1++;
  }

  num2 = 0;
  while(dst[num1] = src[num2]) { // overwrites the NUL byte at the end of dst[] and concats
    num1++;
    num2++;
  }
  return num1;
}
```

Use size_t when trying to get length of char arrays, instead of integers in case it overflows

`<string.h>` 

`strcmp`
```
#include <string.h>
#include <stdio.h>

int main() {
  char str1[] = "hello world";
  char * str2 = "hello world";

  int res = strcmp(str1, str2)

  printf("%d\n", res);
}
```
Output:
```
0
```
  - strcmp returns 0 if strings are equal
  - return lexicographical difference

Can't compare strings like `str1 == str2`

## Memory Layout of a Process

Running instance of a program is a process

Rectangle to showcase memory
  - Stack region is automatically managed (has local variables)
  - Heap region within memory to represent dynamicallly allocated (while the code is running the memory requirments of something might change, programmer is responsible for managing) memory
  - BSS region within memory to represent zero initialized or un-initialized global variables
  - Data region within memory to represent non-zero initialized global variables
  - Text region within memory to represent executable instructions
    - Data and text region are "executable"
    - Stack and heap region grow towards each other (there is also some free space in between them)

```
#include <stdio.h>

int total = 10;

int square(int x) {
  return (x * x);
}

int sos(int x, int y) {
  int z = square(x + y);
  return z;
}

int main() {
  int a = 10, b = 5;
  total = sos(a, b);
  printf(...);
}
```
  - Stack
    - Stack frame (square() - x)
    - Stack frame (sos() - x, y, z
    - Stack frame (main() - a, b)
      - Once the function gets called and evalutated the stack frame gets popped (the function and all its variables)

## Operating System and Memory

Current understanding - Process <--> ISA (Instruction set architecture - Interface) <--> Hardware
  - This works for things like elevators, simple machinery, old school calculators (maybe)

Better system (but still some issues) - P1, P2, ..., Pn <--> ISA <--> Hardware
  - Issues that occur
    - Multiple processes might want to occur at the same time
    - One process overwrites another in memory
    - Processes might not trust each (can't read/write to the memory that current process is using)

Even better system - P1, P2, ..., Pn <--> ABI (Application binary interface) <--> OS Kernel <--> ISA <--> Hardware
  - OS is the only thing able to directly access the Hardware

Goals of the OS Kernel
  - Protection
    - Processes can not access each other's data
  - Abstraction
    - Hide the underlying details of the hardware
      - Allows for same API, regardless of hardware (easier to program)
  - Resource Management
    - How the processes share hardware
      - Context switching
        - The OS comes in a saves the state of the old process that waws running and loads the state of the new process that is going to run
          - P1 -> OS -> P2 -> OS -> P1 -> OS -> P2...

Virtual Memory
  - Abstracts the storage resources of the machine
  - Gives the illusion that the processes have access to a large (OS tells processes that the main memory has more memory than it actually does), private (no other process can access that space), contiguous (continuous) storage
  - You can achieve this by having some sort of virtual address (is large, private, and contiguous) go into an address mapper, which maps it to a physical address (may or may not be large, private, or contiguous)
    - Address mapper could just take in the virtual address, add a constant (base resistor - offset of any prior memory being used), and get the physical address
    - Bound resistor throws exceptions if a process tries to access any memory that goes beyond its limit
    - Issues with this
      - Processes not ending concurrently can cause gaps in the physical memory
      - Processes that need dynamically allocated memory
      - This issue is known a segmentation

Issues with Segmentation
  - What if programs change memory requirements while running
  - What happens if programs start and stop at different times
    - Can lead to fragmentation (your memory is in a state where not all free space is contiguous)
      - Can fix this by having a generalized way of creating multiple bases and bounds to be able to split processes between memory
      - A table is used as middleman to take addresses and map them to a physical address
        - Wastes lots of memory just by storing the memory (tables are stored in the memory)
          - Can create a page (page memory systems)
        - Still more inefficient then segmentation because of having to go through a table to access memory

Can create the illusion that processes need by shuffling the pages (moving old ones to the secondary storage when they are done being used)

## Pass by Reference and Pass by Value

```
char n = 4;
char * ptr = &n;
*ptr = 10;
func(*ptr)
```

You can use pointers to pass by reference (gives ability to access memory in different stack frames)

Passing Arguments to a Function
  - Pass by value
  - Pass by reference

Pass by Value
  - The values of the actual parameter are copied to the function's formal parameters
    ```
    void f(int a) { // formal parameter
      ...
    }

    int main() {
      int b;
      f(b); // actual parameter
    }
    ```

Pass by Reference
  - The address of the actual parameters are passed to the functions formal parameter (NOT THE COMPLETE PICTURE - MORE NUANCED)
  - Really pass by value under the hood (the value is actually copied)

Examples

```
// Pass by value
#include <stdio.h>

void swap(int x, int y) {
  int temp = x;
  x = y;
  y = temp;
  printf("x = %d, y = %d\n", x, y);
}

int main() {
  int x = 10;
  int y = 20;

  swap(x, y);
  printf("x = %d, y = %d\n", x, y);
}
```
Output:
```
x = 20, y = 10
x = 10, y = 20 // Local variable vs. formal parameter
```

```
// Pass by reference
#include <stdio.h>

void swap(int * x, int * y) {
  int temp = * x;
  * x = * y;
  * y = temp;
}

int main() {
  int x = 10;
  int y = 20;

  swap(&x, &y);
  printf("x = %d, y = %d\n", x, y);
}
```
Output:
```
x = 20, y = 10
```

```
int main() {
  int * a = 1; // Pointing to address 1
  int * b = 2;
  print(&a);
```

```
// Passing a pointer by reference
void swap(int ** a, int ** b) { // Something that points to a pointer
  int * t = * a;
  * a = * b;
  * b = t;
  return;
}

int main() {
  int * a = 1;
  int * b = 2;
  swap(&a , &b); // Pointers are pass by value
  print("%p, %p", a, b); // 2, 1
```

```
void fun(int x[], int num) { // [] is equivalent to *
  for (int i = 0; i < num; i++) {
    x[i] = x[i] + 1; // equivalent to x[1] = *(x + 1)
  }
}

int main() {
  int x[2] = {1, 2};
  fun(x, 2); // address of x[0] is copied to x[] in fun
  printf("%d, %d\n", x[0], x[1]);
}
```
Output:
```
2, 3
```

```
void fun(int x[], int num) { // [] is equivalent to *
  for (int i = 0; i < num; i++) {
    x[i] = x[i] + 1; // equivalent to x[1] = *(x + 1)
  int y[2] = {5, 6};
  x = y;
  }
}

int main() {
  int x[2] = {1, 2};
  fun(x, 2); // address of x[0] is copied to x[] in fun
  printf("%d, %d\n", x[0], x[1]);
}
```
Output:
```
2, 3
```
