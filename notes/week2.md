# C: Variables, data types, control flow, operators, arrays, functions, basic I/O - `printf()`, `getchar()`, `putchar()`

## Data Types in C

`bool`
  - Must use `#include <std.bool>` to use

`char`
  - `char c = 'a';`
  - Not ok : `char c = "a";`
  - `char c = 100;`
  - `char c = 'a' - 5;`
    - All characters in C have a numerical value
    - Even storing something like, `'a'`, is still storing a numerical value

`int`
  - `int x = 5;`
  - `int x = 2.3;`
    - Works but would be stored as just 2
  - Numerical values that don't have a decimal point
  - `int` existes instead of just `char` because only 0-205 values could be represented
  - 4 bytes associated with them, 32 bits (CS server) - can represent about 4 billion different values

`float`
  - Data types that can hold digits after the decimal place
  - `float x = 2.3;`
  - Much slower than using just `int` or `char`
  - At least 6 digits of precision

`double`
  - Like float but more precise
  - At least 10 digit precision

`void`
  - Nothing is returned by a function or nothing is passed into a function

Pointers
  - Contents are not the data that you are looking for, but instead it tells you where that data can be found
  - Ex:
    ```
    char n = 4;
    char *ptr = &n;
    ```
      - `*` indicates that a variable is a pointer (dereferencing operator)
      - Contains the address of where n is being stored (ptr is pointing towards n)
  - Ex: Changing Values
    ```
    *ptr = 10;
    ```
      - Changes the value of n
  - `&ptr` is the address of the pointer
  - Ex:
    ```
    ptr = 4;
    *ptr = 5;
    ```
      - Changes the address that `ptr` is pointing to and changes the value of that address to 5
  - Ex:
    ```
    char n = 4
    char *ptr
    ptr = &n
    *ptr = 10
    ```

Arrays
  - Ex:
    ```
    char arr[7] = {'a', 'b', 'c', 'd'};
    ```
  - A collection of elements of the same datatype
  - The memory addresses of elements in arrays are next to each other (successive and contiguously)
  - In an array you can directly jump to the next value of the array (simply by arithmetic)
  - The value of an array in memory is the index of the first element
  - `arr[1]` or `1[arr]`
  - `*(arr + 1)`  or `*(1 + arr)` get you different memory allocations

## Examples

Pointer
```
#include <stdio.h>

int main() {
  int n;
  int *ptr;

  n = 42;
  ptr = &n;

  printf("%d, %d\n", n, *ptr);

  *ptr = 99; // Assigns the value in n to 99

  printf("%d, %d\n", n, *ptr);
}
```
Output:
```
42, 42
99, 99
```

For
```
#include <stdio.h>

int main() {
  for (int i = 1; i <= 10; i++) {
    printf("%d\n", i);
    if (i == 5) {
      printf("Found 5!\n");
      break;
    }
  }
  return 0;
}
```
Output:
```
1
2
3
4
5
Found 5!

```

While
```
#input <stdio.h>

int main() {
  int i = 1;

  while(i <= 5) {
    printf("%d\n", i);
    i++;
  }
}
```
Output:
```
1
2
3
4
5

```

If Else-If
```
#input <stdio.h>

int main() {
  int number;

  printf("Enter an integer: ");
  scanf("%d", &number); // Gives the address of the variable - task by reference

  if (number > 0) {
    printf("The number is positive!\n");
  }
  else if (numer < 0) {
    printf("The number is negative\n");
  }
  else {
    printf("The number is zero\n");
  }
}
```

Switch
```
#include <stdio.h>

int main() {
  int choice = 3;

  switch(choice) {
    case 1:
      printf("You selected option 1\n");
      break;
    case 2:
      printf("You selected option 2\n");
      break;
    case 3:
      printf("You selected option 3\n");
      break;
    case 4:
      printf("You selected option 4\n");
      break;
    defualt:
      printf("Invalid option");
  }
  return 0;
}
```
  - Switch cases can be better for performance
    - You don't evaluate every statement, instead you just jump to the correct case
  - Not everything you can do with switch statements, you can do with if/else statements

```
#inlcude <stdio.h>

int main() {
  int num = 0;

  if (num == 0) {printf("1\n");} 
  if (num){printf("2\n");}
  if (num + 1){printf("3\n");}
  if (num - 1){printf("4\n");} // Would print
  if('0'){printf("5\n");}
  if('a'){printf("6\n);}
  if(' ' - 32){printf("7\n");} // Would not print
}
```

Nested Loops
```
#include <stdio.h>

int main() {
  int i, j;
  int flag = 0;

  for (i = 0; i <= 5; i++ ) {
    for (j = 1; j <= 5;) {
      printf("%d, %d\n", i, j);
      if (i * j == 12) {
        flag = 1;
        break;
      }
    }
    if (flag) {
      break;
    }
  }
  printf("Found 12. Outisde of loops!\n");
}
```

Goto
```
#include <stdio.h>

int main() {
  int i, j;
  int flag = 0;

  for (i = 0; i <= 5; i++ ) {
    for (j = 1; j <= 5;) {
      printf("%d, %d\n", i, j);
      if (i * j == 12) {
        goto exit_loops
        break;
      }
    }
  }
  exit_loops:
  printf("Found 12. Outisde of loops!\n");
}
```

Continue
```
#include <stdio.h>

int main() {
  for (int i = 1; i < 20; i++) {
    if (i % 2 == 0) {
      continue;
    }
    printf("%d\n", i);
  }
}
```

Global Variables
```
#include <stdio.h>

int gblVar = 10;

void fun() {
  gblVar += 5;
  printf("%d\n", glbVar);
}

int main() {
  printf("%d\n", glbVar);
  fun();
  printf("%d\n", glbVar);
}
```
Output:
```
10
15
15
```

Global Variables 2
```
#include <stdio.h>

int x = 10;

void fun(int x) {
  x += 5;
  printf("%d\n", x);
}

void fun2() {
  printf("%d\n", x);
}

int main() {
  printf("%d\n", x); //Global
  int x = 5; //Local
  printf("%d\n", x);
  fun(x);
  printf("%d\n", x);
  fun2();
}
```
Output:
```
10
5
10
5
10
```

## Type Specifiers

You can use to alter type characteristic

May use more than one type specifier to compound effects

Four Categories:
  - Sign
  - Storage
  - Modifiability
  - Access/Scope

Sign
  - `unsigned` and `signed`
  - Use them with characters or integers
  - Ex. `unsigned int x = 5;`
  - Ex. `unsigned char c;`
  - If you have a signed integer, half are held for negatives and half are held for positives
  - An unsigned integer allows you to extend your range

Storage
  - `short`, `long`, `long long`
  - `char <= short int <= int <= long int <= long long int`
  - Ex. `long int i = 0;`

Modifiability
  - `const`
  - Ex. `const int num = 42;`

Access/Scope
  - `static`
  - Applied to global variables
    - Changes the accessibility
      - Limited to code in this source file
  - Applied to local variable
    - Changes persistance
      - Still local, but keeps its value
  - Ex.
    ```
    #include <stdio.h>

    void incVar() {
      int count = 0;
      count++;

      printf("count = %d\n", count);
    }

    int main() {
      incVar();
      incVar();
      incVar();
      incVar();
    }
    ```
    Output:
    ```
    count = 1
    count = 2
    count = 3
    count = 4
    ```

## Literal Constants

Integer type
  - `23`, `47`, `63`, etc.
  - [s][b]d[d*][u]
    - s - sign
    - b - base prefix (Ex. hexadecimal - `0x2a`)
    - d - digit
    - s - suffix (Ex. `long int 1l;`)
    - [] - means it's optional
    - `*` - means you can have more than one

Floating points (ex. 2.3)

String ("abc")

## Implicit Type Conversion

Assignments

Operations

Ex.
```
# include <stdio.h>
int main() {
  char myChar = 1;
  int myInt = myChar;
  long int myLongInt = myInt;

  printf("sizeof(myChar) = %lu\n", sizeof(myChar));
  printf("sizeof(myChar) = %lu\n", sizeof(myInt));
  printf("sizeof(myChar) = %lu\n", msizeof(yLongInt));
}
```
Output
```
1
4
8
```

Ex.
```
#include <stdio.h>

int main() {
  char c1 = -1; // -1
  unsigned char c2 = c1; // 255
  unsigned char c3 = -127; // 129
  char c4 = c3; // 127
}
```

Ex.
```
#include <stdio.h>

int main() {
  int i = 0;
  printf(%lu\n", sizeof(i)):
  printf(%lu\n", sizeof(2.33 + i)):
}
```
Output:
```
4
8
```

## Explicit Type Conversion

```
#include <stdio.h>

int main() {
  double i = 2.45;
  printf("no casting\n %lu\n", sizeof(i)); // 8
  printf("with casting\n %lu\n", sizeof( (int) i)); // 4
}
```

```
#include <stdio.h>

int main() {
  char arr[15] = "hello world"; // and \0 - null byte, marks end of a string
  int * ptr; // since this is an integer pointer and ints are stored in 4 bit and char are stored in 1, it goes 4 bits ahead
  ptr = arr;

  printf("%c", ptr[0]); // h
  printf("%c", ptr[1]); // o
  printf("%c", ptr[2]); // r
}
```
