# Lecture 1

## Origin of C Programming Language

Need for ways of making machine instruction easier

Informaion in a computer is some physical quantity such as voltages

Digital systems or machines have 2 level of this physical quantity
  -Labeled as `0` and `1`

In order to tell a machine to do something (ex. add a constant to a number in memory) 
  - Where the number is stored in memory
  - Where to store the result
  - Value of the constant
  - Tell addition needs to happen

Memory is seperated by different instances, each of these instances habe their own memory address

Machine Language (example): `0010 1010 0011 0111 0011 0010 1111`
  - First four bits might tell the machine what to do (ex. addition)
  - Next eight bits might be where to store the result (memory address)
  - Next eight might be where the number is
  - Next eight might be the value of the constant
    - Assembley Language (turning 0's and 1's into human words): `addi $1, $2, 350`

An assembler converts assembly language into maching langauge

Suppose you wanted to do `a+b*c+d`
  - Would require you to program something to produce assembly languge to make it easier (high-level language)

C is an example of a high-level language

Compiler converts high-level language to assembly language

## Language Paradigms

Object-oriented
  - Program is a collection of classes
  - Encapsulated data types
  - Bundle information and oeprations
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
  - clear: clears the screen
  - ls: see files and folders on the machine
    - -l: more information
  - cd: go into a directory
