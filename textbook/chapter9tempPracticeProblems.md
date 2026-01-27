# Chapter 8

## 1. Type in and run the 11 programs presented in this chapter. Compare the output produced by each program with the output presented after each program in the text.

Program 9.1 Concatenating Character Arrays
```
// Function to concatenate two character arrays
#include <stdio.h>

void concat (char result[], const char str1[], int n1, const char str2[], int n2)
{
  int i, j;

  // copy str1 to result
  for ( i = 0; i < n1; ++i )
    result[i] = str1[i];

  // copy str2 to result
  for ( j = 0; j < n2; ++j )
    result[n1 + j] = str2[j];
}

int main (void)
{
void concat (char result[], const char str1[], int n1, const char str2[], int n2);

const char s1[5] = { 'T', 'e', 's', 't', ' '};
const char s2[6] = { 'w', 'o', 'r', 'k', 's', '.' };
char s3[11];
int i;

concat (s3, s1, 5, s2, 6);

for ( i = 0; i < 11; ++i )
  printf ("%c", s3[i]);

printf ("\n");

return 0;
}
```

Output:
```
Test works.

```

---

Program 9.2 Counting the Characters in a String
```
// Function to count the number of characters in a string
#include <stdio.h>

int stringLength (const char string[])
{
  int count = 0;

  while ( string[count] != '\0' )
    ++count;

  return count;
}

int main (void)
{
  int stringLength (const char string[]);

  const char word1[] = { 'a', 's', 't', 'e', 'r', '\0' };
  const char word2[] = { 'a', 't', '\0' };
  const char word3[] = { 'a', 'w', 'e', '\0' };

  printf ("%i %i %i\n", stringLength (word1),

  stringLength (word2), stringLength (word3));

  return 0;
}
```

Output:
```
5 2 3

```

---

Program 9.3 Concatenating Character Strings
```
#include <stdio.h>

int main (void)
{
  void concat (char result[], const char str1[], const char str2[]);

  const char s1[] = { "Test " };
  const char s2[] = { "works." };
  char s3[20];

  concat (s3, s1, s2);

  printf ("%s\n", s3);

  return 0;
}

// Function to concatenate two character strings
void concat (char result[], const char str1[], const char str2[])
{
  int i, j;

  // copy str1 to result
  for ( i = 0; str1[i] != '\0'; ++i )
    result[i] = str1[i];

  // copy str2 to result
  for ( j = 0; str2[j] != '\0'; ++j )
    result[i + j] = str2[j];

  // Terminate the concatenated string with a null character
  result [i + j] = '\0';
}
```

Output:
```
Test works.

```

---

Program 9.4 Testing Strings for Equality
```
// Function to determine if two strings are equal
#include <stdio.h>
#include <stdbool.h>

bool equalStrings (const char s1[], const char s2[])
{
  int i = 0;
  bool areEqual;

  while ( s1[i] == s2 [i] &&s1[i] != '\0' && s2[i] != '\0' )
    ++i;

  if ( s1[i] == '\0' && s2[i] == '\0' )
    areEqual = true;
  else
    areEqual = false;

return areEqual;
}

int main (void)
{
  bool equalStrings (const char s1[], const char s2[]);

  const char stra[] = "string compare test";
  const char strb[] = "string";

  printf ("%i\n", equalStrings (stra, strb));
  printf ("%i\n", equalStrings (stra, stra));
  printf ("%i\n", equalStrings (strb, "string"));

  return 0;
}
```

Output:
```
0
1
1

```

---

Program 9.5 Reading Strings with `scanf()`
```
// Program to illustrate the %s scanf format characters
#include <stdio.h>

int main (void)
{
  char s1[81], s2[81], s3[81];

  printf ("Enter text:\n");

  scanf ("%s%s%s", s1, s2, s3);

  printf ("\ns1 = %s\ns2 = %s\ns3 = %s\n", s1, s2, s3);

  return 0;
}
```

Output:
```
Enter text:
smart phone
apps

s1 = smart
s2 = phone
s3 = apps

```

---

Program 9.6 Reading Lines of Data
```
#include <stdio.h>

int main (void)
{
  int i;
  char line[81];

  void readLine (char buffer[]);

  for ( i = 0; i < 3; ++i )
  {
    readLine (line);
    printf ("%s\n\n", line);
  }
  return 0;
}

// Function to read a line of text from the terminal
void readLine (char buffer[])
{
  char character;
  int i = 0;

  do {
    character = getchar ();
    buffer[i] = character;
    ++i;
  } while ( character != '\n' );

  buffer[i - 1] = '\0';
}
```

Output:
```
This is a sample line of text.
This is a sample line of text.

abcdefghijklmnopqrstuvwxyz
abcdefghijklmnopqrstuvwxyz

runtime library routines
runtime library routines
```

---

Program 9.7 Counting Words
```
// Function to determine if a character is alphabetic
#include <stdio.h>
#include <stdbool.h>

bool alphabetic (const char c)
{
  if ( (c >= 'a' && c <= 'z') || (c >= 'A' && c <='Z') )
    return true;
  else
    return false;
}

/* Function to count the number of words in a string */
int countWords (const char string[])
{
  int i, wordCount = 0;
  bool lookingForWord = true, alphabetic (const char c);

  for ( i = 0; string[i] != '\0'; ++i )
    if ( alphabetic(string[i]) )
    {
      if ( lookingForWord )
      {
        ++wordCount;
        lookingForWord = false;
      }
    }
    else
      lookingForWord = true;

  return wordCount;
}

int main (void)
{
  const char text1[] = "Well, here goes.";
  const char text2[] = "And here we go... again.";
  int countWords (const char string[]);

  printf ("%s - words = %i\n", text1, countWords (text1));
  printf ("%s - words = %i\n", text2, countWords (text2));

  return 0;
}
```

Output:
```
Well, here goes. - words = 3
And here we go... again. - words = 5

```

---

Program 9.8 Counting Words in a Piece of Text
```
#include <stdio.h>
#include <stdbool.h>

bool alphabetic (const char c)
{
  if ( (c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z') )
    return true;
  else
    return false;
}

void readLine (char buffer[])
{
  char character;
  int i = 0;

  do {
    character = getchar ();
    buffer[i] = character;
    ++i;
  } while ( character != '\n' );

  buffer[i - 1] = '\0';
}

int countWords (const char string[])
{
  int i, wordCount = 0;
  bool lookingForWord = true, alphabetic (const char c);

  for ( i = 0; string[i] != '\0'; ++i )
    if ( alphabetic(string[i]) )
    {
      if ( lookingForWord )
      {
        ++wordCount;
        lookingForWord = false;
      }
    }
    else
      lookingForWord = true;

  return wordCount;
}

int main (void)
{
  char text[81];
  int totalWords = 0;
  int countWords (const char string[]);
  void readLine (char buffer[]);
  bool endOfText = false;

  printf ("Type in your text.\n");
  printf ("When you are done, press 'RETURN'.\n\n");

  while ( ! endOfText )
  {
    readLine (text);

    if ( text[0] == '\0' )
      endOfText = true;
    else
      totalWords += countWords (text);
  }

  printf ("\nThere are %i words in the above text.\n", totalWords);

  return 0;
}
```

Output:
```
Type in your text.
When you are done, press 'RETURN'.

Wendy glanced up at the ceiling where the mound of lasagna
loomed
like a mottled mountain range. Within seconds, she was
crowned with
ricotta ringlets and a tomato sauce tiara. Bits of beef
formed meaty
moles on her forehead. After the second thud, her culinary
coronation
was complete.


There are 48 words in the above text.

```

Program 9.9 Using the Dictionary Lookup Program
```
// Program to use the dictionary lookup program
#include <stdio.h>
#include <stdbool.h>

struct entry
{
  char word[15];
  char definition[50];
};

bool equalStrings (const char s1[], const char s2[])
{
  int i = 0;
  bool areEqual;

  while ( s1[i] == s2 [i] && s1[i] != '\0' && s2[i] != '\0' )
    ++i;

  if ( s1[i] == '\0' && s2[i] == '\0' )
    areEqual = true;
  else
    areEqual = false;

  return areEqual;
}

// function to look up a word inside a dictionary
int lookup (const struct entry dictionary[], const char search[], const int entries)
{
  int i;
  bool equalStrings (const char s1[], const char s2[]);

  for ( i = 0; i < entries; ++i )
    if ( equalStrings (search, dictionary[i].word) )
      return i;

  return -1;
}

int main (void)
{
  const struct entry dictionary[100] =
  { { "aardvark", "a burrowing African mammal" },
  { "abyss", "a bottomless pit" },
  { "acumen", "mentally sharp; keen" },
  { "addle", "to become confused" },
  { "aerie", "a high nest" },
  { "affix", "to append; attach" },
  { "agar", "a jelly made from seaweed" },
  { "ahoy", "a nautical call of greeting" },
  { "aigrette", "an ornamental cluster of feathers" },
  { "ajar", "partially opened" }};

  char word[10];
  int entries = 10;
  int entry;
  int lookup (const struct entry dictionary[], const char search[], const int entries);

  printf ("Enter word: ");
  scanf ("%14s", word);

  entry = lookup (dictionary, word, entries);
  if ( entry != -1 )
    printf ("%s\n", dictionary[entry].definition);
  else
    printf ("Sorry, the word %s is not in my dictionary.\n", word);

  return 0;
}
```

Output:
```
Enter word: agar
a jelly made from seaweed

```

Output:
```
Enter word: accede
Sorry, the word accede is not in my dictionary.

```

---

Program 9.10 Modifying the Dictionary Lookup Using Binary Search
```
// Dictionary lookup program
#include <stdio.h>

struct entry
{
  char word[15];
  char definition[50];
};

// Function to compare two character strings
int compareStrings (const char s1[], const char s2[])
{
  int i = 0, answer;

  while ( s1[i] == s2[i] && s1[i] != '\0'&& s2[i] != '\0')
    ++i;

  if ( s1[i] < s2[i] )
    answer = -1; /* s1 < s2 */
  else if ( s1[i] == s2[i] )
    answer = 0; /* s1 == s2 */
  else
    answer = 1; /* s1 > s2 */

  return answer;
}

// Function to look up a word inside a dictionary
int lookup (const struct entry dictionary[], const char search[], const int entries)
{
  int low = 0;
  int high = entries - 1;
  int mid, result;
  int compareStrings (const char s1[], const char s2[]);

  while ( low <= high )
  {
    mid = (low + high) / 2;
    result = compareStrings (dictionary[mid].word,search);

    if ( result == -1 )
      low = mid + 1;
    else if ( result == 1 )
      high = mid - 1;
    else
      return mid; /* found it */
  }

  return -1; /* not found */
}

int main (void)
{
  const struct entry dictionary[100] =
  { { "aardvark", "a burrowing African mammal" },
  { "abyss", "a bottomless pit" },
  { "acumen", "mentally sharp; keen" },
  { "addle", "to become confused" },
  { "aerie", "a high nest" },
  { "affix", "to append; attach" },
  { "agar", "a jelly made from seaweed" },
  { "ahoy", "a nautical call of greeting" },
  { "aigrette", "an ornamental cluster of feathers"},
  { "ajar", "partially opened" }};

  int entries = 10;
  char word[15];
  int entry;
  int lookup (const struct entry dictionary[], const char search[], const int entries);

  printf ("Enter word: ");
  scanf ("%14s", word);

  entry = lookup (dictionary, word, entries);
  if ( entry != -1 )
    printf ("%s\n", dictionary[entry].definition);
  else
    printf ("Sorry, the word %s is not in my dictionary.\n", word);

  return 0;
}
```

Output:
```
Enter word: aigrette
an ornamental cluster of feathers

```

Output:
```
Enter word: acerb
Sorry, the word acerb is not in my dictionary.

```

---

Program 9.11 Converting a String to its Integer Equivalent
```
// Function to convert a string to an integer
#include <stdio.h>

int strToInt (const char string[])
{
  int i, intValue, result = 0;

  for ( i = 0; string[i] >= '0' && string[i] <= '9'; ++i)
  {
    intValue = string[i] - '0';
    result = result * 10 + intValue;
  }

  return result;
}

int main (void)
{
  int strToInt (const char string[]);

  printf ("%i\n", strToInt("245"));
  printf ("%i\n", strToInt("100") + 25);
  printf ("%i\n", strToInt("13x5"));

  return 0;
}
```

Output:
```
245
125
13

```

## 2. Why could you have replaced the `while` statement of the `equalStrings()` function of Program 9.4 with the statement `while ( s1[i] == s2[i] && s1[i] != '\0' )` to achieve the same results?

Say for example you had the strings `abcde` and `abcd`. If you were to set `s1 = "abcde"` and `s2 = "abcd"`, once you got to index `4`, the loop would exit because `s1[4] = e` and `s2[4] = '\0'`.

## 3. The `countWords()` function from Programs 9.7 and 9.8 incorrectly counts a word that contains an apostrophe as two separate words. Modify this function to correctly handle this situation. Also, extend the function to count a sequence of positive or negative numbers, including any embedded commas and periods, as a single word.

```
int countWords (const char string[])
{
    int i, wordCount = 0;
    bool lookingForWord = true, alphabetic (const char c);

    for (i = 0; string[i] != '\0'; ++i)
    {
        if (
            // alphabetic character
            alphabetic(string[i]) ||

            // apostrophe between alphabetic characters
            (string[i] == '\'' &&
             i > 0 &&
             alphabetic(string[i - 1]) &&
             alphabetic(string[i + 1])) ||

            // digit
            (string[i] >= '0' && string[i] <= '9') ||

            // leading + or - for numbers
            ((string[i] == '+' || string[i] == '-') &&
             string[i + 1] >= '0' && string[i + 1] <= '9') ||

            // comma or period inside a number
            ((string[i] == ',' || string[i] == '.') &&
             i > 0 &&
             string[i - 1] >= '0' && string[i - 1] <= '9' &&
             string[i + 1] >= '0' && string[i + 1] <= '9')
           )
        {
            if (lookingForWord)
            {
                ++wordCount;
                lookingForWord = false;
            }
        }
        else
        {
            lookingForWord = true;
        }
    }

    return wordCount;
}
```

## 4. Write a function called `substring()` to extract a portion of a character string. The function should be called as follows: `substring (source, start, count, result);` where `source` is the character string from which you are extracting the substring, `start` is an index number into `source` indicating the first character of the `substring, count` is the number of characters to be extracted from the `source` string, and `result` is an array of characters that is to contain the extracted substring. For example, the call `substring ("character", 4, 3, result);` extracts the substring `"act"` (three characters starting with character number 4) from the string `"character"` and places the result in `result`. Be certain the function inserts a null character at the end of the substring in the result array. Also, have the function check that the requested number of characters does, in fact, exist in the string. If this is not the case, have the function end the substring when it reaches the end of the source string. So, for example, a call such as `substring ("two words", 4, 20, result);` should just place the string “words” inside the result array, even though 20 characters were requested by the call.

```
void substring(char source[], int start, int count, char result[]) {
    int resultCounter = 0;

    for (int i = start; i < start + count && source[i] != '\0'; i++) {
        result[resultCounter] = source[i];
        resultCounter++;
    }

    result[resultCounter] = '\0';
}
```

## 5. Write a function called `findString()` to determine if one character string exists inside another string. The first argument to the function should be the character string that is to be searched and the second argument is the string you are interested in finding. If the function finds the specified string, have it return the location in the source string where the string was found. If the function does not find the string, have it return −1. So, for example, the call `index = findString ("a chatterbox", "hat");` searches the string `"a chatterbox"` for the string `"hat"`. Because `"hat"` does exist inside the source string, the function returns 3 to indicate the starting position inside the source string where `"hat"` was found.

```
int findString(char s1[], char s2[])
{
    int s2Len = 0;

    while (s2[s2Len] != '\0')
        s2Len++;

    for (int i = 0; s1[i] != '\0'; i++) {
        int j = s2Len - 1;

        while (j >= 0 && s1[i + j] == s2[j])
            j--;

        if (j < 0)
            return i;
    }

    return -1;
}
```

## 6. Write a function called `removeString()` to remove a specified number of characters from a character string. The function should take three arguments: the source string, the starting index number in the source string, and the number of characters to remove. So, if the character array `text` contains the string `"the wrong son"`, the call `removeString (text, 4, 6);` has the effect of removing the characters “wrong” (the word “wrong” plus the space that follows) from the array `text`. The resulting string inside `text` is then `"the son"`.
