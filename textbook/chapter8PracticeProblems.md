# Chapter 8

## 1. Type in and run the seven programs presented in this chapter. Compare the output produced by each program with the output presented after each program in the text.

Program 8.1 Illustrating a Structure
```
// Program to illustrate a structure
#include <stdio.h>

int main (void)
  {
    struct date
    {
      int month;
      int day;
      int year;
    };

struct date today;
today.month = 9;
today.day = 25;
today.year = 2015;

printf ("Today's date is %i/%i/%.2i.\n", today.month, today.day, today.year % 100);

return 0;
}
```

Output:
```
Today's date is 9/25/15.

```

---

Program 8.2 Determining Tomorrow’s Date
```
// Program to determine tomorrow's date
#include <stdio.h>

int main (void)
{
  struct date
  {
    int month;
    int day;
    int year;
  };

  struct date today, tomorrow;

  const int daysPerMonth[12] = { 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

  printf ("Enter today's date (mm dd yyyy): ");
  scanf ("%i%i%i", &today.month, &today.day, &today.year);

  if ( today.day != daysPerMonth[today.month - 1] ) {
    tomorrow.day = today.day + 1;
    tomorrow.month = today.month;
    tomorrow.year = today.year;
  }
  else if ( today.month == 12 ) { // end of year
    tomorrow.day = 1;
    tomorrow.month = 1;
    tomorrow.year = today.year + 1;
  }
  else { // end of month
    tomorrow.day = 1;
    tomorrow.month = today.month + 1;
    tomorrow.year = today.year;
  }

  printf ("Tomorrow's date is %i/%i/%.2i.\n", tomorrow.month, tomorrow.day, tomorrow.year % 100);

  return 0;
}
```

Output:
```
Enter today's date (mm dd yyyy): 12 17 2013
Tomorrow's date is 12/18/13.

```

Output:
```
Enter today's date (mm dd yyyy): 12 31 2014
Tomorrow's date is 1/1/15.

```

Output:
```
Enter today's date (mm dd yyyy): 2 28 2012
Tomorrow's date is 3/1/12.

```

---

Program 8.3 Revising the Program to Determine Tomorrow’s Date
```
// Program to determine tomorrow's date
#include <stdio.h>
#include <stdbool.h>

struct date
{
  int month;
  int day;
  int year;
};

int main (void)
{
  struct date today, tomorrow;
  int numberOfDays (struct date d);

  printf ("Enter today's date (mm dd yyyy): ");
  scanf ("%i%i%i", &today.month, &today.day, &today.year);

  if ( today.day != numberOfDays (today) ) {
    tomorrow.day = today.day + 1;
    tomorrow.month = today.month;
    tomorrow.year = today.year;
  }
  else if ( today.month == 12 ) { // end of year
    tomorrow.day = 1;
    tomorrow.month = 1;
    tomorrow.year = today.year + 1;
  }
  else { // end of month
    tomorrow.day = 1;
    tomorrow.month = today.month + 1;
    tomorrow.year = today.year;
  }

  printf ("Tomorrow's date is %i/%i/%.2i.\n",tomorrow.month, tomorrow.day, tomorrow.year % 100);

  return 0;
}

// Function to find the number of days in a month
int numberOfDays (struct date d)
{
  int days;
  bool isLeapYear (struct date d);
  const int daysPerMonth[12] = { 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };

  if ( isLeapYear (d) == true && d.month == 2 )
    days = 29;
  else
    days = daysPerMonth[d.month - 1];

  return days;
}

// Function to determine if it's a leap year
bool isLeapYear (struct date d)
{
  bool leapYearFlag;

  if ( (d.year % 4 == 0 && d.year % 100 != 0) || d.year % 400 == 0 )
    leapYearFlag = true; // It's a leap year
  else
    leapYearFlag = false; // Not a leap year

  return leapYearFlag;
}
```

Output:
```
Enter today's date (mm dd yyyy): 2 28 2016
Tomorrow's date is 2/29/16.
```

Output:
```
Enter today's date (mm dd yyyy): 2 28 2014
Tomorrow's date is 3/1/14.
```

---

Program 8.4 Revising the Program to Determine Tomorrow’s Date, Version 2
```
// Program to determine tomorrow's date
#include <stdio.h>
#include <stdbool.h>

struct date
{
  int month;
  int day;
  int year;
};

// Function to calculate tomorrow's date
struct date dateUpdate (struct date today)
{
  struct date tomorrow;
  int numberOfDays (struct date d);

  if ( today.day != numberOfDays (today) ) {
    tomorrow.day = today.day + 1;
    tomorrow.month = today.month;
    tomorrow.year = today.year;
  }
  else if ( today.month == 12 ) { // end of year
    tomorrow.day = 1;
    tomorrow.month = 1;
    tomorrow.year = today.year + 1;
  }
  else { // end of month
    tomorrow.day = 1;
    tomorrow.month = today.month + 1;
    tomorrow.year = today.year;
  }

  return tomorrow;
}

// Function to find the number of days in a month
int numberOfDays (struct date d)
{
  int days;
  bool isLeapYear (struct date d);
  const int daysPerMonth[12] = { 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };

  if ( isLeapYear (d) && d.month == 2 )
    days = 29;
  else
    days = daysPerMonth[d.month - 1];

  return days;
}

// Function to determine if it's a leap year
bool isLeapYear (struct date d)
{
  bool leapYearFlag;

  if ( (d.year % 4 == 0 && d.year % 100 != 0) || d.year % 400 == 0 )
    leapYearFlag = true; // It's a leap year
  else
    leapYearFlag = false; // Not a leap year

  return leapYearFlag;
}

int main (void)
{
  struct date dateUpdate (struct date today);
  struct date thisDay, nextDay;

  printf ("Enter today's date (mm dd yyyy): ");
  scanf ("%i%i%i", &thisDay.month, &thisDay.day, &thisDay.year);

  nextDay = dateUpdate (thisDay);

  printf ("Tomorrow's date is %i/%i/%.2i.\n",nextDay.month, nextDay.day, nextDay.year % 100);

  return 0;
}
```

Output:
```
Enter today's date (mm dd yyyy): 2 28 2016
Tomorrow's date is 2/29/16.
```

Output:
```
Enter today's date (mm dd yyyy): 2 22 2015
Tomorrow's date is 2/23/15.
```

---

## 5. Write a function called `clockKeeper()` that takes as its argument a `dateAndTime` structure as defined in this chapter. The function should call the `timeUpdate()` function, and if the time reaches midnight, the function should call the `dateUpdate` function to switch over to the next day. Have the function return the updated `dateAndTime` structure.

```
clockKeeper(dateAndTime time) {
  time.time = timeUpdate(time.time);

  if (time.time.hour == 0 && time.time.minute = 0) {
    time.date = dateUpdate(time.date);
  }

  return time;
}
```

## 6. Replace the `dateUpdate()` function from Program 8.4 with the modified one that uses compound literals as presented in the text. Run the program to verify its proper operation.
```
// Function to calculate tomorrow's date
struct date dateUpdate (struct date today)
{
  struct date tomorrow;
  int numberOfDays (struct date d);

  if ( today.day != numberOfDays (today) ) {
    tomorrow = (struct date) {today.day + 1, today.month, today.year};
  }
  else if ( today.month == 12 ) { // end of year
    tomorrow  = (struct date) {1, 1, today.year + 1};
  }
  else { // end of month
    tomorrow = (struct date) {1, today.month + 1, today.year};
  }

  return tomorrow;
}
```
