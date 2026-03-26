# C: Advanced features (bits, type qualifiers, etc.)

## Bit Fields

Supposed we want to represent 5 variables. Assume 3 are boolean flags: f1, f2, f3. Assume 1 is an unsigned integer in the range 0 - 255: type. Assume 1 is an unsigned integer in the range 0 - 100000: index.
  - 3 bits necessary for the boolean flags
  - 8 bits necessary for type
  - 17 bits necessary (log base 2 of 100001 and take the ceiling) for index
  - Total of 28 bits necessary
  ```
  struct info {
    char f1, f2, f3; // 3 bytes
    unsigned char type; // 1 byte
    unsigned int index; // 4 bytes
    // 8 bytes or 64 bits
  };
  ```

```
#include <stdio.h>

struct packed_data {
  unsigned int :4;
  unsigned int f1:1;
  unsigned int f2:1;
  unsigned int f3:1;
  unsigned int type :8;
  unsigned int index :17;
};

union testing {
  unsigned int full;
  struct packed_data bits;
};

int main() {
  union testing data;

  data.full = 0;

  data.bits.f1 = 1;
  printf("%#010x\n", data.full);

  data.bits.index = 1;
  printf("%#010x\n", data.full);
  
}
```

## Enumerated Types

enum_ex1.c
```
#include <stdio.h>

enum days {
  Sunday,
  Monday = 17,
  Tuesday = 17,
  Wednesday, // 18
  Thursday, // 19
  Friday,
  Saturday
};

#define PRT(x) printf(#x " is %d\n", x);

int main() {
  for (int today = Sunday; today <= Saturday; today++) {
    if (today < Monday || today > Friday) {
      print("It's the weekend\n");
    }
    else {
      printf("Back to the daily grind\n);
    }
  }

  PRT(Sunday);
  PRT(Monday);
  PRT(Tuesday);
  PRT(Wednesday);

  return 0;
}
```

## More Type Specifiers

Register variables:
  - Syntax: register type var_name
  - Optimization hint - frequency of use
  - Doesn't change the mean of anything inherently
  - A compiler is free to ignore this
  - This variable will be used a lot
  - Keeping it in a CPU register will improve performance

Volatile variables:
  - Syntax: volatile type var_name
  - When not to optimize
  - This variable may change between access
  - No point in keeping it in the resistor
  - Have to load it from a different part of the memory anyways

Restrict variables:
  - Syntax: type * restrict var;
  - Promises that two pointers will not be aliases (never point to the same place as another pointer variable) within the same scope
  - Can be more efficient
```
void updatepointers(int * p1, int * p2, int * value) {
  * p1 += * value;
  * p2 += * value;
}
```
  - load R1 from * value
  - load R2 from * p1
  - compute R2 += R1
  - store R2 in * p1
  - load R1 from * value
  - load R2 from * p2
  - computer R2 += R1
  - store R2 in * p2
```
void updatepointers(int restrict * p1, int restrict * p2, int restrict * value) {
  * p1 += * value;
  * p2 += * value;
}

int a;
update pointers(&a, &a, &a) // result is undefined, depends on compiler, compiler settings, etc.
```
  - load R1 from * value
  - load R2 from * p1
  - compute R2 += R1
  - store R2 in * p1
  - load R2 from * p2
  - computer R2 += R1
  - store R2 in * p2
  - Programmer is responsible for restrict safety

## END OF EXAM 2

## Threads

Program: set of instructions and some data

Process: program in execution
  - Historical view: 
    - Process is a single execution path through a block of code
  - Current view:
    - Process is a container for one or more execution paths
    - Each path is called a thread
    - These threads execute in the environment of the process that created them

Why interest in threads?
  - Improve performance by multiplexing computing
  - Multi core hardware -> can run a thread on each core in parallel

Threads have several advantagers over processes
  - Creating a process takes longer than creating a thread
  - Context switching between threads is faster
  - Processes take more memory within the operating system

Threads within a process share:
  - Executable code
  - OS-level resources
  - Global data
  - Heap

Threads have their own:
  - Stack region
  - Context (registers, etc.)
  - Scheduling priority
  - Private data areas

Shared data areas pose a danger:
  - Any thread can change them arbitrarily

Concept: thread-sage code (reentrant code)
  - Code that can be executed simultaneously by multiple threads
  - Avoids use of global data structures
  - Historically, each operating system had its own propritary version
    - Significant variability between versions
    - Difficult to port programs between systems
    - POSIX threads (pthreads) became standard

Multiple threads have no relationship to each other
  - No parent/child relationship (all equal)
  - Any thread may create new threads

Two main types of threads:
  - Joinable (default behavior)
    - "Linger" after exiting so that status can be picked up by another thread (via join operation)
  - Detached
    - Disappear when they exit
    - Exit status is discarded

`int pthread_create(pthread_t * tid, const pthread_attr_t * attr, void * (* start_routine)(void *), void * arg);`
  - Returns 0 on success, else an error number
  - New thread executes by calling `start_rountine(arg)`
  - Function must have this prototype:
    - `void * name(void * arg);`

Termination: 
  - `void pthread_exit(void * retval)`
  - `return (...)`

th_ex1.c
```
#include <stdlib.h>
#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

int count = 0;

void * routine(void * input) {
  for (int i = 0; i < 10000; i++) {
    count++;
  }
}

int main() {
  pthread_t t1, t2, t3, t4;

  if (pthread_create(&t1, NULL, &routine, NULL) != 0) {
    return 1;
  }
  if (pthread_create(&t2, NULL, &routine, NULL) != 0) {
    return 1;
  }
  if (pthread_create(&t3, NULL, &routine, NULL) != 0) {
    return 1;
  }
  if (pthread_create(&t4, NULL, &routine, NULL) != 0) {
    return 1;
  }

  if (pthread_join(t1, NULL) != 0) {
    return 2;
  }
  if (pthread_join(t2, NULL) != 0) {
    return 2;
  }
  if (pthread_join(t3, NULL) != 0) {
    return 2;
  }
  if (pthread_join(t4, NULL) != 0) {
    return 2;
  }

  // sleep(1);

  printf("The value of count is %d\n", count);
}
```
