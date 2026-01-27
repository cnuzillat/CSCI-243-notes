# Chapter 8

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
