# General: Abstract Data Types (ADTs)

## Type Checking

Process of enforcing languages typing rules

Strongly typed:
  - Prohibit (in an enforceable way) using any operation on a type that isn't supposed to support that operation
  - e.g. Ada, Pascal, etc.

Weakly typed: 
  - Can get around typing restrictions explicitly or implicitly
  - e.g. C

Can be done at different times:
  - Static typing:
    - AKA early binding of type information
    - Done entirely at compile time
    - e.g. C
  - Dynamic typing:
    - AKA late binding of type information
    - Done at run time during execution
    - e.g. Lisp

## Abstraction

High level view of a sequence of low level activities

Control abstraction -> looping/decision constructs, function calls, etc.

Data abstraction -> seperate implementation from usage
  - e.g. "integer" vs. 32-bit binary value

Easier to think about things without gory low-level details

Easier to control access to things you shouldn't see

Simplifies modular design
  - Change the implementation without changing the interface

## Abstract Data Types

AKA module types

Conceptual entity that consists of:
  - Data
  - Operations on that data

Data is encapsulated
  - No way to access or modify data except via defined operations
  - "Black box" module

Seperation of implementation from usage

Easy in object-oriented languages
  - Define a class with private data and public operations
  - You can only access or modify data via defined operations

Harder in other languages:
  - e.g. C
  - May not be able to completely hide the internals
  - To deal with the challenges: Code a .c file that the user doesn't have access to (StackADT.c - implementation), an interface (StackADT.h), and a user of stack (driver.c)

## Examples

stackADT.h
```
#ifndef STACKADT_H_
#define STACKADT_H_

#include <stdbool.h>

// makes it impossible for the driver.c to access the data in stackStruct since there is no information about stackStruct in the file
typedef struct stackStruct * StackADT; // StackADT is a pointer to stackStruct

StackADT stk_create(void); // driver calls this whenever an instance of a stack is requested
void stk_destroy(StackADT stack); // when the user is done with their stack, they can call this and the allocated memory is freed
void stk_clear(StackADT stack); // removes all the elements from the stack
void stk_push(StackADT stack, int n);
int stk_pop(StackADT stack);
int stk_top(StackADT stack);
bool stk_empty(stackADT stack);
bool stk_full(StackADT stack);

#endif STACKADT_H_
```

stackADT.c
```
#include <stdlib.h>
#include <assert.h>
#include <unistd.h>

#include "stackADT.h"

#define STACK_SIZE 5

struct stackStruct{
  int contents[STACK_SIZE];
  size_t num;
};

StackADT stk_create(void) {
  StackADT new;
  new = (StackADT) malloc(sizeof(struct stackStruct));

  if (new != 0) {
    new -> num = 0;
  }

  return(new);
}

void stk_destroy(StackADT stack) {
  free(stack);
}

void stk_clear(StackADT stack) {
  assert(stack != 0);
  stack -> num = 0;
}

void stk_push(StackADT stack, int n) {
  assert(stack != 0);
  assert(stack -> num < STACKSIZE);

  stack -> contents[stack -> num] = n;
  stack -> num += 1;
}

int stk_pop(StackADT stack) {
  assert(stack != 0);
  int n = stack -> num -= 1;
  int data = stack -> contents[n];
  return data;
}

int stk_pop(StackADT stack) {
  assert(stack != 0);
  int n = stack -> num - 1;
  int data = stack -> contents[n];
  return data;
}

bool stk_empty(StackADT stack) {
  return stack -> num == 0;
}

bool stk_full(StackADT stack) {
  return stack -> num == STACKSIZE;
}
```

driver1.c
```
#include <stdio.h>

#include <stackADT.h"

void test_and_pop(StackADT stack) {
  if (stk_empty(stack)) {
    printf("Stack is empty!!. DO better stupid\n");
  }
  else {
    printf("Top element is %d\n", stk_top(stack));
    int n = stk_pop(stack);
    printf("Remove %d\n", n);
  }    
}

int main() {
  StackADT mystack = stk_create();

  if (mystack == 0) {
    printf("Stack gave up on us\n");
    return(1);
  }

  printf("Initial stack is %s\n", stk_empty(mystack) ? "empty" : "not empty");

  stk_push(mystack, 42);
  stk_push(mystack, 17);
  stk_push(mystack, 12);

  test_and_pop(mystack);
  test_and_pop(mystack);
  test_and_pop(mystack);
  test_and_pop(mystack);

  printf("Final stack is %s\n", stk_empty(mystack) ? "empty" : "not empty");

  return 0;
}
```

driver2.c
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
    printf("Top element is %d\n", stk_top(stack));
    int n = stk_pop(stack);
    printf("Remove %d\n", n);
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

  try_to_push(mystack, 42);
  try_to_push(mystack, 17);
  try_to_push(mystack, 12);
  try_to_push(mystack, -99);
  try_to_push(mystack, 347);
  try_to_push(mystack, -224);

  pop_all(mystack);

  printf("Final stack is %s\n", stk_empty(mystack) ? "empty" : "not empty");

  return 0;
}
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
  int * contents;
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

void stk_push(StackADT stack, int n) {
  assert(stack != 0);

  if (stack -> contents == 0) {
    stack -> contents = (int *) malloc(sizeof(int) * STACK_ALLOC_UNIT);
    assert(stack -> contents != 0);
    stack -> capacity = STACK_ALLOC_UNIT;
  }

  if (stack -> num == stack -> capacity) {
    int * tmp = (int *) realloc(stack -> contents, sizeof(int) * (stack -> capacity + STACK_ALLOC_UNIT));
    assert(tmp != 0);
    stack -> contents = tmp;
    stack -> capacity += STACK_ALLOC_UNIT;
  }

  stack -> contents[stack -> num] = n;
  stack -> num += 1;
}

int stk_pop(StackADT stack) {
  assert(stack != 0);
  int n = stack -> num -= 1;
  int data = stack -> contents[n];
  return data;
}

int stk_pop(StackADT stack) {
  assert(stack != 0);
  int n = stack -> num - 1;
  int data = stack -> contents[n];
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

stackADTV2.h
```
#ifndef STACKADT_H_
#define STACKADT_H_

#include <stdbool.h>

#ifndef STACK_IMPLEMENTATION
typedef struct {} * StackADT; // StackADT is a pointer to stackStruct
#endif

StackADT stk_create(void); // driver calls this whenever an instance of a stack is requested
void stk_destroy(StackADT stack); // when the user is done with their stack, they can call this and the allocated memory is freed
void stk_clear(StackADT stack); // removes all the elements from the stack
void stk_push(StackADT stack, int n);
int stk_pop(StackADT stack);
int stk_top(StackADT stack);
bool stk_empty(stackADT stack);
bool stk_full(StackADT stack);

#endif STACKADT_H_
```
