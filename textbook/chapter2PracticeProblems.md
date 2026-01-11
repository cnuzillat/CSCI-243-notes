# Chapter 2

## 1. Type in and run the six programs presented in this chapter. Compare the output produced by each program with the output presented after each program in the text.

Program 2.1 Writing Your First C Program
```
#include <stdio.h>

int main (void)
{
     printf ("Programming is fun.\n");

     return 0;
}
```

Output:
```
Programming is fun.

```

---

Program 2.2
```
#include <stdio.h>

int main (void)
{
     printf ("Programming is fun.\n");
     printf ("And programming in C is even more fun.\n");

     return 0;
}
```

Output:
```
Programming is fun.
And programming in C is even more fun.

```

---

Program 2.3 Displaying Multiple Lines of Output
```
#include <stdio.h>

int main (void)
{
     printf ("Testing...\n..1\n...2\n....3\n");

     return 0;
}
```

Output:
```
Testing...
..1
...2
....3

```

---

Program 2.4 Displaying Variables
```
#include <stdio.h>

int main (void)
{
     int sum;

     sum = 50 + 25;
     printf ("The sum of 50 and 25 is %i\n", sum);

     return 0;
}
```

Output:
```
The sum of 50 and 25 is 75

```

---

Program 2.5 Displaying Multiple Values
```
#include <stdio.h>

int main (void)
{
     int  value1, value2, sum;

     value1 = 50;
     value2 = 25;
     sum = value1 + value2;
     printf ("The sum of %i and %i is %i\n", value1, value2, sum);

     return 0;
}
```

Output:
```
The sum of 50 and 25 is 75

```

---

Program 2.6 Using Comments in a Program
```
/* This program adds two integer values
   and displays the results             */

#include <stdio.h>

int main (void)
{
    // Declare variables
    int  value1, value2, sum;

    // Assign values and calculate their sum
    value1 = 50;
    value2 = 25;
    sum = value1 + value2;

    // Display the result
    printf ("The sum of %i and %i is %i\n", value1, value2, sum);

    return 0;
}
```

Output:
```
The sum of 50 and 25 is 75

```

## 2. Write a program that prints the following text.

1. In C, lowercase letters are significant.

2. main() is where program execution begins.

3. Opening and closing braces enclose program statements in a routine.

4. All program statements must be terminated by a semicolon.

---

```
#include <stdio.h>

int main (void)
{
    printf("In C, lowercase letters are significant.\n");
    
    printf("main() is where program execution begins.\n");
    
    printf("Opening and closing braces enclose program statements in a routine.\n");
    
    printf("All program statements must be terminated by a semicolon.\n");

    return 0;
}
```

Output:
```
In C, lowercase letters are significant.
main() is where program execution begins.
Opening and closing braces enclose program statements in a routine.
All program statements must be terminated by a semicolon.

```

## 3. What output would you expect from the following program?
```
#include <stdio.h>

int main (void)
{
    printf ("Testing...");
    printf ("....1");
    printf ("...2");
    printf ("..3");
    printf ("\n");

    return 0;
}
```

---

Output:
```
Testing.......1...2..3

```

## 4. Write a program that subtracts the value 15 from 87 and displays the result, together with an appropriate message, at the terminal.
```
# include <stdio.h>

int main(void) {
  int value1 = 87;
  int value2 = 15;

  int sum = value1 - value2;

  printf("%i - %i = %i", value1, value2, sum);

  return 0;
}
```

Output:
```
87 - 15 = 72
```

##  Identify the syntactic errors in the following program. Then type in and run the corrected program to ensure you have correctly identified all the mistakes.
```
#include <stdio.h>

int main (Void)
(
        INT  sum;
        /* COMPUTE RESULT
        sum = 25 + 37 - 19
        /* DISPLAY RESULTS //
        printf ("The answer is %i\n" sum);
        return 0;
}
```

---

Corrected:
```
#include <stdio.h>

int main (void)
{
        int  sum;
        // COMPUTE RESULT
        sum = 25 + 37 - 19;
        // DISPLAY RESULTS
        printf ("The answer is %i\n", sum);
        return 0;
}
```

Output:
```
The answer is 43

```

##  What output might you expect from the following program?
```
#include <stdio.h>

int main (void)
{
      int answer, result;

      answer = 100;
      result = answer - 10;
      printf ("The result is %i\n", result + 5);

      return 0;
}
```

---

Output:
```
The result is 95

```
