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
      - Produces output tokens of the meaningful groups: <id, 1> <=> <id, 2> <+> <id, 3> <60>
