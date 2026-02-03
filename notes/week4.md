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

int lessthan_int(consdt void *a, const void *b) {
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

END OF EXAM 1 CONTENT
---

## Dynamically Allocated Memory

In C, a developer must manually handle allocation and deallocation of dynamically allocated memory

Heap memory is known as the dynamically allocated memory

Each allocation request reserves a contiguous area of reguested size amd returns a pointer to the heap block

A heap block should be deallocated once it is no longer needed 

```
int a; // Stack
int * ptr; // Stack
ptr = (int *) malloc(sizeof(int)); // Allocates 4 bytes (int) on the heap, malloc return void * so you have to cast to int *
*ptr = 10; // Go to ptr, look at the contents, go that memory location, change the value in that memory location
```
