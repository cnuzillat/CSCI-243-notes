# C: Variables, data types, control flow, operators, arrays, functions, basic I/O - `printf()`, `getchar()`, `putchar()`

## Data Types in C

`bool`
  - Must use #include <std.bool> to use

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
  - Data types that can hold digets after the decimal place
  - `float x = 2.3;`
  - Much slower than using just `int` or `char`
  - At least 6 digits of precision

`double`
  - Like float but more precise
  - At least 10 digit precision

`void`
  - Nothing is returned by a function of nothing is passed into a function

Pointers
  - Contents are not the data that you are looking for, but instead it tells you where that data can be found
  - Ex:
    ```
    char n = 4;
    char *ptr = &n;
    ```
      - * indicates that a variable is a pointer (dereferencing operator)
      - Contains the address of where n is being stored (ptr is pointing towards n)
  - Ex: Changing Values
    ```
    *ptr = 10;
    ```
      - Changes the value of n
  -`&ptr` is the address of the pointer
  - `*n = 5` takes the contents of n and goes to the address equal to the value and changes the value of that address
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

  for (i = 0; i <= 5; i++ ) {
    for (j = 1; j <= 5;) {
      printf("%d, %d\n", i, j);
      if (i * j == 12) {
        
      }
    }
  }
}
```
