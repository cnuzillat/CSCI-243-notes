# General: Abstract Data Types (ADTs)

## Type Checking

Process of enforcing languages typing rules

Strongly typed:
  - Prohibit (in an enforceable way) using any operation on a type that isn't supposed to support that operation
  - e.g. Ada, Pascal, ...

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
  - To deal with the challenges: Code a .c file that the user doesn't have access to (Stack ADT), an interface (Stacl ADT .h), and a user of stack (driver.c)

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
#include <>

#include "stackADT.h"

#define STACK_SIZE 5
struct stackStruct{
  int contents[STACK_SIZE]
  size_t num;
}

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
  stack -> num = 0;
}
```
