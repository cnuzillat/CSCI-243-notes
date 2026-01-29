# Chapter 13

## 1. Define a type `FunctionPtr()` (using `typedef`) that represents a pointer to a function that returns an `int` and that takes no arguments. Refer to Chapter 10, “Pointers,” for the details on how to declare a variable of this type.
```
typedef int (*FunctionPtr)();
```

## 2. Write a function called `monthName()` that takes as its argument a value of type `enum month` (as defined in this chapter) and returns a pointer to a character string containing the name of the month. In this way, you can display the value of an `enum month` variable with a statement such as: `printf ("%s\n", monthName (aMonth));`
```
char *monthName(enum month thisMonth)
{
    static char *names[] = {"Invalid month", "January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"};

    if (thisMonth < january || thisMonth > december)
        return names[0];

    return names[thisMonth];
}
```

## 3. Given the following variable declarations: `float f = 1.00;` `short int i = 100;` `long int l = 500L;` `double d = 15.00;` and the seven steps outlined in this chapter for conversion of operands in expressions, determine the type and value of the following expressions: `f + i` `l / d` `i / l + f` `l * i` `f / 2` `i / (d + f)` `l / (i * 2.0)` `l + i / (double) l`

f + i = float 101.0
l / d = double 33.33333333333333
i / l + f = float 1.0
l * i = long int 50000
f / 2 = float 0.5
i / (d + f) = double 6.25
l / (i * 2.0) = double 2.5
l + i / (double) l = double 500.2
