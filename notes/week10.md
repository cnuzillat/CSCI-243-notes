# C: Advanced features (bits, type qualifiers, etc.)

## Bit Fields

Supposed we want to represent 5 variables. Assume 3 are boolean flags: f1, f2, f3. Assume 1 is an unsigned integer in the range 0 - 255: type. Assume 1 is an unsigned integer in the range 0 - 100000: index.
  - 3 bits necessary for the boolean flags
  - 8 bits necessary for type
  - 17 bits necessary (log base 2 of 100001 and take the ceiling) for index
  - Total of 28 bits necessary
  ```
  struct info {
    char f1, f2, f3; // 3 bytes
    unsigned char type; // 1 byte
    unsigned int index; // 4 bytes
    // 8 bytes or 64 bits
  };
  ```

```
#include <stdio.h>

struct packed_data {
  unsigned int :4;
  unsigned int f1:1;
  unsigned int f2:1;
  unsigned int f3:1;
  unsigned int type :8;
  unsigned int index :17;
};

union testing {
  unsigned int full;
  struct packed_data bits;
};

int main() {
  union testing data;

  data.full = 0;

  data.bits.f1 = 1;
  printf("%#010x\n", data.full);

  data.bits.index = 1;
  printf("%#010x\n", data.full);
  
}
```

## Enumerated Types

enum_ex1.c
```
#include <stdio.h>

enum days {
  Sunday,
  Monday = 17,
  Tuesday = 17,
  Wednesday, // 18
  Thursday, // 19
  Friday,
  Saturday
};

#define PRT(x) printf(#x " is %d\n", x);

int main() {
  for (int today = Sunday; today <= Saturday; today++) {
    if (today < Monday || today > Friday) {
      print("It's the weekend\n");
    }
    else {
      printf("Back to the daily grind\n);
    }
  }

  PRT(Sunday);
  PRT(Monday);
  PRT(Tuesday);
  PRT(Wednesday);

  return 0;
}
```

## More Type Specifiers

Register variables:
  - Syntax: register type var_name
  - Optimization hint - frequency of use
  - Doesn't change the mean of anything inherently
  - A compiler is free to ignore this
  - This variable will be used a lot
  - Keeping it in a CPU register will improve performance

Volatile variables:
  - Syntax: volatile type var_name
  - When not to optimize
  - This variable may change between access
  - No point in keeping it in the resistor
