# C: Pointers and dynamic storage (continued) and General: Program maintenance

## Program Translation Process

How do you run a program on a machine written in a source language?

Compiler
  - source program -> compiler -> target program

Interpreter
  - source program, inputs -> interpreter -> outputs
  - Ex. Python

Hybrid Compiler
  - source program -> translator -> intermediate program, input -> virtual machine -> output
  - Ex. Java

C Program (HLL) -(preprocessor)-> Compiler -> Assembly Language -> Assembler -> Object Program (ML) -(libraries)-> Linker -> Executable Load Module (ML) -> Loader (in the OS) -> Program in Memory
  - Linking is much faster than compiling and assembling

```
position = initial + rate * 60
```
  - First step in compilation process is a lexical analyzer (scanner)
    - Reads the stream of characters making up the source program and groups characters into meaningful squences called lexemes
      - Lexeme is a meaningful group of characters
      - Produces output tokens of the meaningful groups: <id, 1> <=> <id, 2> <+> <id, 3> <*> <60>
  - Second step: Syntax Analyzer
    -Syntax Tree: = -> <id, 1>, (+ -> <id, 2>, (* -> <id, 3>, 60))
```
expression -> term | expression add_operation term
  - term -> factor | term multiplication_operation factor
    - factor -> identifier (not necessary) | number (not necessary) | -factor | (expression)
    - multiplicatiion_operation -> * | /
  - add_operation -> + | -

10 - 4 - 3
  - Tree:
    - expression
      - expression
        - expression
          - term
            - factor
              - number (10)
        - add_operation
          - -
        - term
          - factor
            - number (4)
      - add_operation
        - -
      - term
        - factor
          - number (3)

3 + 4 * 5
  - Tree:
    - expression (3 + 4 * 5)
      - expression
        - term
        - factor
        - number (3)
      - add_operation
        - +
      - term
        - term
          - factor
            - number (4)
        - multi_op
          - *
        - factor
          - number (5)
```
  - Semantic Analyzer
    - Certain statement can be true gramatically but still don't make sense
    ```
    =
      - <id, 1>
      - +
        - <id, 2>
        - *
          - <id, 3>
          - int to float
            - 60
    ```
    - Semantic Amalysis
      - The semantic analyzer uses the syntax tree and the information in the symbol table to check the source program for symantic consistency with the language definition
      - It also gathers type information and saves it in either the syntax tree or the symbol table for subsequent use
      - An imporant part of semantic analysis is type checking
        - Coercion
  - Intermediate Code Generator
    - Parses the tree using some kind of algorithm
    ```
    t1 = int to float (60)
    t2 = id3 * t1
    t3 = id2 + t2
    id1 = t3
    ```
    - Must be very similar to assembly language
    - Intermediate Code Generation:
      - After syntax and semantic analysis of the source program, many compilers generate an explicit low-level or machine-like intermediate representation
      - Which we can think of as a program for an abstract machine
      - The intermediate representation:
        - Easy to produce from the syntax tree
        - Easy to convert to target language
        - Three address code:
          - Each instruction has at most one operator on the right side of the assignment oeprator
          - These instructions fix the order in which operations are to be done
  - Code Optimizer
    ```
    t1 = id3 * 60.0
    id1 = id2 + t1
    ```
      - Machine-independent optimization
    - Code optimization:
      - The machine-independent code optimization phase attempts to improve the intermediate code so that "better" target code can be generated
      - "Better" usually means faster but other objectives may be desirable
        - Memory efficiency
        - Precision
        - Energy efficiency
        - Size of executable
        - Registers
  - Code Generator
    ```
    LDF R2, id3
    MULF R2, R2, #60.0
    LDF R1, id2
    ADDF R1, R1, R2
    STF id1, R1
    ```
    - Code Generation:
      - The code generator takes as input an intermediate representation of the source program and maps it to the target language 
