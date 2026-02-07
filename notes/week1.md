# General: Programming paradigms; Environments; Modular design & development and C: Language history & overview

## Origin of C Programming Language

Need for ways of making machine instruction easier

Information in a computer is some physical quantity such as voltages

Digital systems or machines have 2 levels of this physical quantity
  - Labeled as `0` and `1`

In order to tell a machine to do something (ex. add a constant to a number in memory), you need to know:
  - Where the number is stored in memory
  - Where to store the result
  - Value of the constant
  - Tell the machine that addition needs to happen

Memory is seperated by different instances, each of these instances have their own memory address

Machine Language (example): `0010 1010 0011 0111 0011 0010 1111`
  - First four bits might tell the machine what to do (ex. addition)
  - Next eight bits might be where to store the result (memory address)
  - Next eight might be where the number is
  - Next eight might be the value of the constant
    - Assembley Language (turning 0's and 1's into human words): `addi $1, $2, 350`

An assembler converts assembly language into machine langauge

Suppose you wanted to do `a+b*c+d`
  - Would require you to program something to produce assembly languge to make it easier (high-level language)

C is an example of a high-level language

Compiler converts high-level language to assembly language

## Language Paradigms

Object-oriented
  - Program is a collection of classes
  - Encapsulated data types
  - Bundle information and operations
  - Ex. Java, C#, Small Talk
  - Example:
    ```
    Person:
      - age
      - name
      - dead/alive
      - change_name(...)
      - kill(...)

    Faculty:
      - salary
     ```

Imperative
  - Statements are commands that change the state (memory) of the program
  - Program is a sequence of statements
  - Flat collection of modules (no hierarchical structure)
  - Ex. C, Fortran, Pascal, Modula-2, Basic

Functional
  - Computation is a series of function calls
  - "Function" in the mathematical sense
  - No state
  - Produce reliable output from input
  - Easy to write proofs for

## Writing Code

SSH into CS machines: `ssh csn1296@machine-name.cs.rit.edu`

Commands
  - `clear`: clears the screen
  - `ls`: see files and folders on the machine
    - `-l`: more information
  - `cd`: go into a directory
    - `..`: go up one directory
  - `mkdir`: create a directory
  - `cp`: copy
    - `-r`: copy directory
  - `mv`: move
  - `rm`: remove
    - `-r`: remove folder
    
Tab will autocomplete for you

Hello World
```
#include <stdio.h>

int main(void) {
  printf("Hello world!\n"); // string - "hello", char - 'h'

  return 0;
}
```
  - You must compile .c programs into .o: `gcc -c hello.c`
  - `gcc -o hello hello.o` creates your executable
  - `gcc hello.c` does both in one step
  - `./` to run a compiled program
  - `gcc hello.c && ./.out` does both in one step

Hello Input
```
#include <stdio.h>

// this is a comment
int main(int argc, char * argv[]) { // argc is the # of input line arguments, argv[] array of strings
  printf("Hello %s\n", argv[1]);

  return 0;
}
```

Putting quotes around an argument ensures that it is taken as one argument
  - "abeer is awesome" vs. abeer is awesome

Print Integers
```
#include <stdio.h>

int main() {
  int x = 10;
  int y = 10;

  printf("x = %d, y = %d\n", x, y);

  return 0;
}
```

`gmakemake > Makefile` product of gmakemake is stored in the Makefile

`make` compiles is for you

`make realclean` removes old files for you (temp files)

`./executable > output.txt` puts output in a .txt file

CONSTANTs
```
#include <stdio.h>

#define CONSTANT 5

int main() {
  int x = 10 * CONSTANT;
  int y = 20 - CONSTANT;

  printf("x = %d, y = %d, CONSTANT = %d\n", x, y, CONSTANT);
}
```

Create Header File
```
#ifndef CONSTANT_H
#define CONSTANT_H

#define INCHES_PER_CENTIMETER 0.394
#define CENTIMETERS_PER_INCH (1 / INCHES_PER_CENTIMETER)

#endif
```

Using Custom Header File
```
#include <stdio.h>
#include "constants.h" // Put in quotes to tell the compiler that it's in that current directory

void convert(int inches) {
  printf("%d inches = %f centimeters\n", inches, inches * CENTIMERTERS_PER_INCH);
}

int main() {
  convert(10);
  convert(163);
  convert(5971);
}
```
