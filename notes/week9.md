# General: ADTs (continued)

## ADT Example

stackADTV3.h
```
#ifndef STACKADT_H_
#define STACKADT_H_

#include <stdbool.h>

#ifndef STACK_IMPLEMENTATION
typedef struct {} * StackADT; // StackADT is a pointer to an anonymous struct - can't dereference it
#endif

StackADT stk_create(void); // driver calls this whenever an instance of a stack is requested
void stk_destroy(StackADT stack); // when the user is done with their stack, they can call this and the allocated memory is freed
void stk_clear(StackADT stack); // removes all the elements from the stack
void stk_push(StackADT stack, void * data);
void * stk_pop(StackADT stack);
void * stk_top(StackADT stack);
bool stk_empty(stackADT stack);
bool stk_full(StackADT stack);

#endif STACKADT_H_
```

stackADTV2.c
```
#include <stdlib.h>
#include <assert.h>
#include <unistd.h>
#include <stdbool.h>

typdef struct stackStruct * StackADT;

#define STACK_IMPLEMENTATION
#include "stackADT.h"

#define STACK_ALLOC_UNIT 5

struct stackStruct{
  void ** contents;
  size_t capacity;
  size_t num;
};

StackADT stk_create(void) {
  StackADT new;
  new = (StackADT) malloc(sizeof(struct stackStruct));

  if (new != 0) {
    new -> contents = 0;
    new -> capacity = 0;
    new -> num = 0;
  }

  return(new);
}

void stk_destroy(StackADT stack) {
  assert(stack != 0);
  free(stack -> contents);
  free(stack);
}

void stk_clear(StackADT stack) {
  assert(stack != 0);

  free(stack -> contents);
  stack -> contents = 0;
  stack -> capacity = 0;
  stack -> num = 0;
}

void stk_push(StackADT stack, void *  data) {
  assert(stack != 0);

  if (stack -> contents == 0) {
    stack -> contents = (void **) malloc(sizeof(void *) * STACK_ALLOC_UNIT);
    assert(stack -> contents != 0);
    stack -> capacity = STACK_ALLOC_UNIT;
  }

  if (stack -> num == stack -> capacity) {
    int * tmp = (void **) realloc(stack -> contents, sizeof(void *) * (stack -> capacity + STACK_ALLOC_UNIT));
    assert(tmp != 0);
    stack -> contents = tmp;
    stack -> capacity += STACK_ALLOC_UNIT;
  }

  stack -> contents[stack -> num] = data;
  stack -> num += 1;
}

void * stk_pop(StackADT stack) {
  assert(stack != 0);
  int n = stack -> num -= 1;
  void * data = stack -> contents[n];
  return data;
}

void * stk_pop(StackADT stack) {
  assert(stack != 0);
  int n = stack -> num - 1;
  void * data = stack -> contents[n];
  return data;
}

bool stk_empty(StackADT stack) {
  return stack -> num == 0;
}

bool stk_full(StackADT stack) {
  (void) stack;
  return false;
}
```

driver3.c
```
#include <stdio.h>

#include "stackADT.h"

void try_to_push(StackADT stack, int n) {
  if (stk_full(stack)) {
    printf("Stack is full!!. DO better stupid\n");
  }
  else {
    stk_push(stack, n);
    printf("Pushed %d\n", n);
  }
}

void test_and_pop(StackADT stack) {
  if (stk_empty(stack)) {
    printf("Stack is empty!!. DO better stupid\n");
  }
  else {
    printf("Top element is %#18lx\n", (unsigend long) stk_top(stack));
    data = stk_pop(stack);
    printf("Remove %#18lx\n", (unsigned long) data);
  }    
}

void pop_all(StackADT stack) {
  while (!stk_empty(stack)) {
    int n = stk_pop(stack);
    printf("Popped %d\n", n);
}

int main() {
  StackADT mystack = stk_create();

  if (mystack == 0) {
    printf("Stack gave up on us\n");
    return(1);
  }

  printf("Initial stack is %s\n", stk_empty(mystack) ? "empty" : "not empty");

  try_to_push(mystack, (void *) 42);
  try_to_push(mystack, (void *) 17);
  try_to_push(mystack, (void *) 12);
  try_to_push(mystack, (void *) -99);
  try_to_push(mystack, (void *) 347);
  try_to_push(mystack, (void *) -224);

  pop_all(mystack);

  printf("Final stack is %s\n", stk_empty(mystack) ? "empty" : "not empty");

  return 0;
}
```

## Unions

```
// Memory is stored in the exact same memory location, regardless of variable or type
// Changing another value in the same memory address can change another vlaue
union stackData {
  char c[8];
  float f[2];
  int i[2]
  short s[4];
  unsigned int u[2];
  double d;
  void * v;
};
```

uni.c
```
#include <stdio.h>
#include <ctype.h>

union myData {
  char c[8];
  float f[2];
  int i[2];
  short s[4];
  unsigned int u[2];
  double d;
  void * v;
};

void printch(int index, char data) {
  int ch = data & 0xff;
  printf("c[%d] = %#04x\n", index, ch);

  if(isprint(ch)) {
    printf(" %c \n", ch);
  }
  else {
    printf(" ? "\n);
  }
}

void printall(const char * msg, union myData data) {
  pritnf("\n****Data changed was %s ****\n", msg);

  printf("v = %#18lx", (long unsigned int) data.v); // print whole union as a hexadecimal number

  for (int i = 0; i < 8; i++) {
    printch(i, data.c[i]);
  }
  printf("f[0] = %12f f[1] = %12f\n", data.f[0], data.f[1]);
  printf("i[0] = %d i[1] = %d\n", data.i[0], data.i[1]);
  printf("s[0] = %d s[1] = %d s[2] = %d s[3] = %d\n", data.s[0], data.s[1], data.s[2], data.s[3]);
}

int main() {
  union myData x;

  x.c[0] = 'a';
  x.c[1] = 'b';
  x.c[2] = 'c';
  x.c[3] = 'd';
  x.c[4] = 'e';
  x.c[5] = 'f';
  x.c[6] = 'g';
  x.c[7] = 'h';

  printall("character", x);

  x.i[0] = -5;
  x.i[1] = 3;

  printall("integer", x);

  x.f[0] = 4.25;
  x.f[1] = 6.75;

  printall("float", x);

  x.s[0] = 8;
  x.s[1] = 5;
  x.s[2] = 3;
  x.s[3] = 9;

  printall("short", x);
}
```

## Comma Operator

comma.c
```
#include <stdio.h>

int f(void) {
  return 12;
}

int g(void) {
  return 42;
}

int main() {
  int i, j, x;

  for (int i = 0, j = 12; i < 4; ++i, --j) {
    printf("%d, %d\n", i, j);
  }
  x = (f(), g()); // 42
  x = f(), g(); // 12
}
```

## Binary Numbers

1's and 0's

0, 1, 10, 11, 100, 101, 110, 111, etc.

To get value of binary number do the value of the number multiplied by its 2 to the power of its place

To make a number + or - just add a bit assigning that value

Bit Patterns:
  - 0 - 0000
  - 1 - 0001
  - 2 - 0010
  - 3 - 0011
  - 4 - 0100
  - 5 - 0101
  - 6 - 0110
  - 7 - 0111

  - -1 - 1111
  - -2 - 1110
  - -3 - 1101
  - -4 - 1100
  - -5 - 1011
  - -6 - 1010
  - -7 - 1001
  - -8 - 1000

One's complement

Two's complement - shifting range by one
  - Flip everything after the right leading 1 (0101 -> 1011)
  - Always leads to the negative of a number

Adding 1's (-) or 0's (+) to left of a number doesn't change the meaning of the number

Subtraction comes from the bit falling off

This shows why overflowing addition can lead to a negative number

## Bitwise Operators

Operate at the bit level

Two forms:
  - Bitwise logical
  - Shift

Bitwise Logical:
  A | B | A & B | A | B | A ^ B | ~A
  0   0     0       0       0     1
  0   1     0       1       1     1
  1   0     0       1       1     0
  1   1     1       1       0     0

Shift:
  - Shift left (<<)
    - 0101 -> 1010 -> 0100 -> 0000
  - Shift right (>>)
    - Ex. 0101 -> 0010 -> 0001 -> 0000
  - Equivalent to multiplying or dividing by 2

bitwise.c
```

#include <stdio.h>

void binprint(unsigned int n, unsigned int bits) {
  char ch[bits + 1];
  for (int i = bits; i > 0; i--) {
    ch[i - 1] = n & 1 ? '1' : '0';
    n >>= 1;
  }
  ch[bits] = '\0';
  printf("%s", ch);
}

void testing(unsigned int v1, unsigned int v2, unsigned int res, const char * op, unsigned int bits) {
  binprint(v1, bits);
  printf(" %s ", op);
  binprint(v2, bits);
  printf(" = ");
  binprint(res, bits);
  printf("\n");

int main() {
  testing(0xb3, 0x2e, 0xb3 "&" 0x2e, "&", 8):
  testing(0xb3, 0x2e, 0xb3 "|" 0x2e, "&", 8):
  testing(0xb3, 0x2e, 0xb3 "^" 0x2e, "&", 8):

  printf(" ~ ");
  binprint(0x2e, 8);
  printf(" = ");
  binprint(~0x2e, 8);
  printf("\n");

  printf("\nBoolean:\n");
  testing(0xb, 0x6, 0xb && 0x6, "&&", 4);
  printf("\nBitwise\n");
  testing(0xb, 0x6, 0xb && 0x6, "&", 4);

  printf("\nBoolean:\n");
  testing(0xb, 0x4, 0xb && 0x4, "&&", 4);
  printf("\nBitwise\n");
  testing(0xb, 0x4, 0xb && 0x4, "&", 4);
}
```

count_set_bits.c
```
#include <stdio.h>

unsigned int countSetBits(unsigned int n) {
  unisgned int count = 0;
  while (n) {
    count += n & 1;
    n >>= 1;
  }
  return count;
}

int main() {
  unsigned int i = 11;
  printf("%d\n", countSetBits(i));
  return 0;

}
```

count_set_bits2.c
```
#include <stdio.h>

unsigned int countSetBits(unsigned int n) {
  unisgned int count = 0;
  while (n) {
    n &= (n - 1);
    count++;
  }
  return count;
}

int main() {
  unsigned int i = 11;
  printf("%d\n", countSetBits(i));
  return 0;

}
```

shift.c
```
#include <stdio.h>

void testing(unsigned int v1, int v2, unsigned int res, const char * op) {
  printf("%#010x", v1);
  printf(" %s ", op);
  printf("%d = ", v2);
  printf("%#010x", res);
  printf("\n");
}

int main() {
  unsigned int v1 = 0x12345678; // left-most bit is 0 -- positive
  unsigned int v2 = 0x87654321; // negative -- 1000 (8)

  int v3 = 0x12345678;
  int v4 = 0x87654321;

  printf("\n unsigned, positive number: \n");
  testing(v1, 4, v1 << 4, "<<"); // shifts one number to the left and drops the 1 (int is 4 bits)
  testing(v1, 4, v1 >> 4, ">>");

  printf("\n unsigned, negative number: \n");
  testing(v2, 4, v2 << 4, "<<");
  testing(v2, 4, v2 >> 4, ">>");

  printf("\n signed, positive number: \n");
  testing(v3, 4, v3 << 4, "<<");
  testing(v3, 4, v3 >> 4, ">>");

  printf("\n signed, negative number: \n");
  testing(v4, 4, v4 << 4, "<<");
  testing(v4, 4, v4 >> 4, ">>"); // not 0 - 1111 - keeps the negative value
}
```

Use ~&|^:
  - To check if a positive number is even or odd
    - n & 1
      - ex. 101 & 001 = 1
  - Detect if two integers have opposite signs
    - (x ^ y) < 0
      - ex. 0101 ^ 1011 = 1110
  - Add one to an integer
    - ~x
      - ~x = x + 1
  - Supposed you want to encrypt a message x, you can encrypt it with a key y, x ^ y is the encrypted text
    - x ^ y ^ y decrypts your message
  - Swap two integers without using a third variable
    - x = x ^ y -> y = y ^ x -> x = x ^ y
