# C: Pointers and dynamic storage

## Exam One

Basic concepts - Notes

Heavy pointers for code analysis on exam 1

20-30 lines of writing code

Everything before dynamically allocated memory for exam 1

## Pointers to Functions

Function Pointer
```
#include <stdio.h>

void fun(int a) {
  printf("Input was %d\n", a);
}

int main() {
  void (*fun_ptr)(int) = fun;
  (*fun_ptr)(10);
  fun_ptr(10);
  (******fun_ptr)(10); // Any amount of stars
}
```
Output:
```
Input was 10
Input was 10
Input was 10
```

cdecl

This is useful when passing a function as input to another function

## qsort

```
#include <stdio.h>
#inlcude <stdlib.h>
#include <string.h>

int lessthan_int(const void *a, const void *b) {
  return(*((int *)a) - *((int * b)));
}

int lessthan_str(const void *a, const void *b) {
  return(strcmp(*((char **)a), *((char **)b));
}

int data_int[] = {7, 299, 1, -3, 45, 22441, -88, 0, 24, 409};
int num_data_int = sizeof(data_int)/sizeof(int); // number of elements

char * data_str[] = {"ball", "dog", "cat", "apple"};
int num_data_str = sizeof(data_str)/sizeof(char *); // You could put any pointer type because all pointers are the same size

int main() {
  int i;

  printf("Originial integers: ");
  for (i = 0; i < num_data_int; i++) {
    printf("%d, ", data_int[i]);
  }
  printf("\n");

  qsort(data_int, num_data_int, sizeof(int), lessthan_int);
  printf("Sorted integers: ");
  for (i = 0; i < num_data_int; i++) {
    printf("%d, ", data_int[i]);
  }
  printf("\n");
  }

  printf("Originial strings: ");
  for (i = 0; i < num_data_str; i++) {
    printf("%s, ", data_str[i]);
  }
  printf("\n");

  qsort(data_str, num_data_str, sizeof(char *), lessthan_str);
  printf("Sorted strings: ");
  for (i = 0; i < num_data_str; i++) {
    printf("%s, ", data_str[i]);
  }
  printf("\n");
  }
}
```

## END OF EXAM 1 CONTENT
---

## Dynamically Allocated Memory

In C, a developer must manually handle allocation and deallocation of dynamically allocated memory

Heap memory is known as the dynamically allocated memory

Each allocation request reserves a contiguous area of reguested size amd returns a pointer to the heap block

A heap block should be deallocated once it is no longer needed 

```
int a; // Stack
int * ptr; // Stack
ptr = (int *) malloc(sizeof(int)); // Allocates 4 contiguous bytes (int) on the heap, malloc return void * so you have to cast to int *
*ptr = 10; // Go to ptr, look at the contents, go that memory location, change the value in that memory location
ptr = (int *) malloc(sizeof(int));
```
  - Issues with this is that there's no way to go to the previous memory address

Memory Leak
  - Occurs when a process allocates a heap block and loses address of that storage 
    - You can't use it and no one else can use it because the address is lost
    - Memory leak isn't just forgetting to deallocate memory
      
```
int a;
int * ptr; 
ptr = (int *) malloc(sizeof(int));
*ptr = 10; 
free(ptr); // Frees where that pointer was pointing towards
ptr = (int *) malloc(sizeof(int));
```

Dangling Pointers
  - Pointers pointing to deallocated heap memory blocks
  - Never dereference a dangling pointer

Allocate Scalar
```
#include <stdio.h>
#include <assert.h>
#include <stdlib.h>

int main() {
  int * jptr = (int *) malloc(sizeof(int)); // Implicit conversion between void star and int star warning

  assert(jptr); // Ensures that the input argument to it is true, if not, it produces an error

  *jptr = 10;

  printf("sizeof(int) = %lu\n", sizeof(int)); // 4
  printf("sizeof(jptr) = %lu\n", sizeof(jptr)); // 8

  printf(%p: %d\n", (void *)jptr, *jptr); // memory location: 10

  free(jptr); // jptr becomes a dangling pointer
  jptr = NULL; // fixs the issue with dangling pointers

  return 0;
}
```

Allocate Vector
```
#include <stdio.h>
#include <stdlib.h>
#include <assert.h>

#define DIM 7

int main() {
  int * values = (int *) malloc(DIM * sizeof(int));
  assert(values);

  printf("sizeof(int) = %lu\n", sizeof(int))); // 4
  printf("sizeof(values) = %lu\n", sizeof(values))); // 8 - size of pointer, not array

  for (int i = 0; i < DIM; i++) {
    values[i] = (int) random();
    printf("%p: %d\n", (void *) (values + i), values[i]);
  }

  free(values);
  values = NULL;

  return 0;
}
```

Allocate Structures
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <assert.h>

#define NAMELEN 30
#define MAJORLEN 30

struct Student_S {
  char name[NAMELEN};
  char major[MAJORLEN];
  float gpa;
};

int main(int argc, char * argv[]) {
  if(argc != 4) {
    return 1;
  }

  struct Student_S * sptr = (struct Student_S *) malloc(sizeof(struct Student_S);
  assert(sptr)

  printf("sizeof(sptr) = %lu\n", sizeof(sptr));
  printf("sizeof(struct Student_S) = %lu\n", sizeof(Student_S));

  strncpy(sptr -> name, argv[1], NAMELEN - 1);
  strncpy(sptr -> major, argv[2], MAJORLEN - 1);
  sptr -> gpa = strtof(argv[3], NULL);

  printf("%s, %s, %f\n", sptr -> name, sptr -> major, sptr -> gpa);

  free(sptr);
  sptr = NULL;

  return 0;
}
```

```
#include <stdio.h>

void fun(int * a) {
  a = (int *) malloc(sizeof(int));
}

int main() {
  int * p;
  fun(p);
  *p = 6;
  printf("%d\n", *p);
  
}
```
  - This would error because p doesn't have a pointee
  - There's a memory leak

```
#include <stdio.h>

void fun(int ** a) {
  *a = (int *) malloc(sizeof(int));
}

int main() {
  int * p;
  fun(&p);
  *p = 6;
  printf("%d\n", *p);
}
```

## Exam 1 Practice Problems

```
#include <stdio.h>

int main() {
  char * ptr = "Awesome";
  printf("%c", *&*&*ptr); // A
}
```

```
#include <stdio.h>

int main() {
  float arr[5] = {12.5, 10, 13.5, 90.5, 0.5};
  float *ptr1 = &arr[0];
  float *ptr2 = ptr1 + 3;
  printf("%f\n", *ptr2); // 90.5
  printf("%lu\n", ptr2-ptr1); // 3
}
```

```
#include <stdio.h>

int main() {
  float arr[5] = {12.5, 10, 13.5, 90.5, 0.5};
  float *ptr1 = &arr[1] - 1;
  float *ptr2 = ptr1 + 3;
  printf("%f\n", *ptr2); // 90.5
  printf("%lu\n", ptr2-ptr1); // 3
}
```

```
#include <stdio.h>

int fun(int x, int y) {
  return (x - (x == y));
}

int main() {
  int a = 25, b = 24 + 1, c;
  printf("%d\n", fun(a, b)); // 24
  return 0;
}
```

```
#include <stdio.h>

int main() {
  char array[5] = "Knot", *ptr, i, *ptr1;

  ptr = &array[1];
  ptr1 = ptr + 3;
  *ptr = 'e';
  for (int i = 0; i < 4; i++) {
    prtinf("%c", *(ptr++))); // note
  }
  return 0;
}
```

```
#include <stdio.h>

#define print(x) printf("%d, ", x)

int x = 0;

void Q(int z) {
  z += x;
  print(z);
}

void P(int *y) {
  int x = *y + 2;
  Q(x);
  *y = x - 1;
  print(x);
}

int main() {
  x = 5;
  P(&x);
  print(x); 
}
```
Output:
```
12, 7, 6
```

```
#include <stdio.h>

char *c[] = {"Easiest", "Question", "of_my", "Life");
char ** cp[] = {c+3, c+2, c+1, c};
char ***cpp = cp;

int main() {
  printf("%s\n", *(*(++cpp))); // of_my
  printf("%s\n", *(--(*(++cpp))) + 3); // iest
  printf("%s\n", *(cpp[-2]) + 3); // e
  printf("%s\n", cpp[-1][-1] + 1); // uestion
}
```
