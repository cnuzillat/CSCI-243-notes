# General: Program translation

Output of the compiler is assembly language

## Assembler

Translates Assembly language to machine language (1's and 0's)

Produced as a relocatable object module (ROM)

Example ROM format is Executable and linkable format

## Linker (Link Editor)

Linkers combine ROMs to create load modules

For each ROM, connect global symbol references and definitions (functions, variables, etc.)

Adjust relocatable values

Why do we need to adjust things?
  - The assemblers assume each ROM starts at location 0
  - All internal addresses are relative to that point
  - When combining, only one can be the first (i.e. at location 0)

## Loader (Within the OS Kernel)

Part of the OS

Copies Load Modules into memory and runs them

Steps: 
  - Allocate memory for the program (space for text region, data region, ...)
  - Copies segments into the new address space
  - Sets up the runtime stack, register (where the top of the stack, program counter (keeps track of what instruction will run next), etc.)
  - Jumps to the entry point:
    - Startup routine, which calls main()
    - If your main() returns, the startup machine calls exit()

# Preprocessor

Outputs of your preprocessor go to the compiler

Kind of like a text processing software

Filter before the compiler sees the code

CPP (C PreProcessor) directives
  - \# directive [operands]
  - Four kinds of directives:
    - File inclusion
    - Macro definition
    - Conditional compilation
    - Other miscellaneous
  - Examples
    file_inc_ex.c
    ```
    #include <stdio.h> // Takes this text that is present and pastes it on top of this file

    int main() {
      printf("My name is Abeer\n");
      return 0;
    }
    ```
    
    macro_def_ex1
    ```
    #define LOOP for(;;){
    #define ENDLOOP }
    #define MY_NAME "Abeer"

    LOOP
    printf("%s\n", MY_NAME);
    ENDLOOP
    ```

    macro_def_ex2
    ```
    #include <stdio.h>

    #define f(x) x*x

    int main() {
      int y = 4;
      printf("%d\n", f(y)); // Prints 16

      return 0;
    }
    ```

    macro_def_ex3
    ```
    #include <stdio.h>

    #define MAX(a, b) ((a) > (b) ? (a) : (b))

    int main() {
      int a = 50;
      int b = 100;
      printf("%d\n", MAX(a,b));

      int i = 0;
      int j = 0;
      printf("%d\n", MAX(i++, ++j)); // 2
    }
    ```

    macro_def_ex4
    ```
    #include <stdio.h>

    #define PRT(n) printf("The vlaue of " #n " is %d\n", n) // # says to put "" quote around it

    int main() {
      for (int i = 0; i < 10; i++) {
        PRT(i+1); // The value of i+1 is ...
      }
    }
    ```

    macro_def_ex5
    ```
    #include <stdio.h>

    #define VAR(x) var##x // token pasting operator

    int main(void) {
      int varA = 10;
      int varB = 20;

      printf("%d\n", VAR(A)); // 10
      printf("%d\n", VAR(A)); // 20
    }
    ```

    cond_comp_ex1
    ```
    #ifdef name
    name is defined // if name is defined name is defined gets put into the final output
    #else
    name is not defined
    #endif
    ```

    cond_comp_ex2
    ```
    #include <stdio.h>

    int main() {
      int num = 1;
      #if num // # if 1 - preprocessor knows it's true
        printf("%d\n", num) // wouldn't print anything
      #elif

      #else
      #endif
    }
    ```

    cond_comp_ex3
    ```
    #if defined(A) && defined(B) && !defined(C)

    some text
    #endif
    ```

    cond_comp_ex4
    ```
    #include <stdio.h>
    #define DEBUG

    int main() {
      #ifdef DEBUG
        printf("Debugging\n");
      #else
        printf("Not debugging\n");
      #endif

      #ifdef DEBUG
        #undef DEBUG
        #ifndef DEBUG
          printf("Debugging turned off\n");
        #endif
      #endif

      return(0);
    }
    ```
      - You could define a macro with -DDEBUG when compiling the program

    cond_comp_ex5
    ```
    #define OPTION_A
    
    #if defined(OPTION_A)
    option a
    #elif defined(OPTION_B)
    option b
    #else
      #error No options was selected! Do better stupid!
    #endif
    ```

    misc_ex
    ```
    #include <stdio.h>

    int main() {
      // #line 10 "weird.c"
      printf("%s, %d, %s, %s\n", __FILE__, __LINE__, __DATE__, __TIME__); // misc_ex.c, 4, Feb 24 2026, 14:19:02
    ```

Can be run as a stand alone filter on non-C code (e.g. to preprocess assembly language)
