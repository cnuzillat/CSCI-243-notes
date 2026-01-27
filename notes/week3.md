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

`{'h', 'e', 'l', 'p', '\0'}` // you can have more characters after `'\0'`, but it's not part of the string

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

Rectangle to showcase memory
  - Stack region is automatically managed (has local variables)
  - Heap region within memory to represent dynamicallly allocated (while the code is running the memory requirments of something might change, programmer is responsible for managing) memory
  - BSS region within memory to represent zero initialized or un-initialized global variables
  - Data region within memory to represent non-zero inititalized global variables
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
