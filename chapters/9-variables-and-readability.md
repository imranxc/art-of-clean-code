## Chapter 9: Variables and Readability

## Eliminating Variables

In this section, we will focus on eliminating variables that do not enhance readability. By removing such variables, we can achieve more concise code that remains easy to understand.

### Useless Temporary Variables

Consider the following Python code:

```py
import datetime

now = datetime.datetime.now()
root_message.last_view_time = now
```

Is the variable `now` worth keeping? No, and here are the reasons:

* It does not break down a complex expression.
* It does not add clarification; the expression `datetime.datetime.now()` is clear enough.
* It is used only once, meaning it does not help compress redundant code.

Without the variable `now`, the code is just as easy to understand:

```py
import datetime

root_message.last_view_time = datetime.datetime.now()
```

### Eliminating Intermediate Results

Here’s an example of a JavaScript function that removes a value from an array:

```jsx
var remove_one = function (array, value_to_remove) {
  var index_to_remove = null;

  for (var i = 0; i < array.length; i += 1) {
    if (array[i] === value_to_remove) {
      index_to_remove = i;
      break;
    }
  }

  if (index_to_remove !== null) {
    array.splice(index_to_remove, 1);
  }
}
```

The variable `index_to_remove` merely holds an intermediate result. We can eliminate this variable by handling the result as soon as we obtain it:

```jsx
var remove_one = function (array, value_to_remove) {
  for (var i = 0; i < array.length; i += 1) {
    if (array[i] === value_to_remove) {
      array.splice(i, 1);
      return;
    }
  }
}
```

By allowing the function to `return` early, we have removed `index_to_remove` altogether and simplified the code.

### Eliminating Control Flow Variables

Control flow variables, such as `done` in the following Java example, are often unnecessary:

```java
boolean done = false;

while (/* condition */ && !done) {
  ...
  if (...) {
    done = true;
    continue;
  }
}
```

These variables are used solely to steer the program’s execution and do not contain any actual program data. We can often eliminate control flow variables by making better use of structured programming:

```java
while (/* condition */) {
  ...
  if (...) {
    break;
  }
}
```

## Shrink the Scope of Your Variables

We’ve all heard the advice to "avoid global variables." This is good advice because it’s hard to track where and how all those global variables are being used.

It’s advisable to "shrink the scope" of all your variables, not just global ones.

> Make your variable visible by as few lines of code as possible.

Many programming languages offer multiple scope/access levels, including module, class, function, and block scope. Using more restricted access is generally better because it means the variable can be "seen" by fewer lines of code.

Why is this important? Because it effectively reduces the number of variables the reader has to consider at one time. If you shrink the scope of all your variables by a factor of two, on average, there will be half as many variables in scope at any given time.

For example, consider a very large class with a member variable that is used by only two methods:

```cpp
#include <string>

class LargeClass {
  std::string str_;

  void method1() {
    str_ = ...;
    method2();
  }

  void method2() {
    // Uses str_
  }
  // Lots of other methods that don't use str_ ...
};
```

In this case, it may make sense to "demote" `str_` to a local variable:

```cpp
class LargeClass {
  void method1() {
    std::string str = ...;
    method2(str);
  }

  void method2(std::string str) {
    // Uses str
  }

  // Now other methods can't see str.
};
```

Consider the following C++ code:

```cpp
PaymentInfo* info = database.readPaymentInfo();

if (info) {
  cout << "User paid: " << info->amount() << endl;
}

// Many more lines of code below ...
```

The variable `info` remains in scope for the rest of the function, which may lead the reader to wonder how it will be used again. However, since `info` is only used inside the `if` statement, we can define it in the conditional expression:

```cpp
if (PaymentInfo* info = database.ReadPaymentInfo()) {
  cout << "User paid: " << info->amount() << endl;
}
```

This way, the reader can easily forget about `info` after it goes out of scope.

Now, consider a persistent variable that is used by only one function:

```jsx
submitted = false;  // Note: global variable

var submit_form = function (form_name) {
  if (submitted) {
    return;  // don't double-submit the form
  }
  ...
  submitted = true;
};
```

Global variables like `submitted` can create confusion for the reader. It appears that `submit_form()` is the only function using `submitted`, but another JavaScript file might be using a global variable named `submitted` for a different purpose.

We can prevent this issue by wrapping submitted inside a closure:

```jsx
var submit_form = (function () {
  var submitted = false;  // Note: can only be accessed by the function below

  return function (form_name) {
    if (submitted) {
      return;  // don't double-submit the form
    }
    ...
    submitted = true;
  };
}());
```

Note the parentheses on the last line—the anonymous outer function is immediately executed, returning the inner function.

Languages like C++ and Java have block scope, where variables defined inside an `if`, `for`, `try`, or similar structure are confined to that block's nested scope.

```cpp
if (...) {
  int x = 1;
}

x++;  // Compile-error! 'x' is undefined.
```

In contrast, Python and JavaScript allow variables defined in a block to "spill out" to the entire function. Consider this potentially problematic Python code:

```py
# No use of example_value up to this point.
if request:
  for value in request.values:
    if value > 0:
      example_value = value
      break

for logger in debug.loggers:
  logger.log("Example:", example_value)
```

If `example_value` is not set in the first part of the code, the second part will raise a `NameError: 'example_value' is not defined`. We can fix this and improve readability by defining `example_value` at the "closest common ancestor" in terms of nesting:

```py
example_value = None

if request:
  for value in request.values:
    if value > 0:
      example_value = value
      break

if example_value:
  for logger in debug.loggers:
    logger.log("Example:", example_value)
```

However, `example_value` can be eliminated entirely, as it merely holds an intermediate result. Here’s how we can refactor the code:

```py
def LogExample(value):
  for logger in debug.loggers:
    logger.log("Example:", value)

if request:
  for value in request.values:
    if value > 0:
      LogExample(value)  # deal with 'value' immediately
      break
```

### Prefer Write-Once Variables

We’ve discussed how too many variables can make code harder to understand. It becomes even more challenging to track variables that change frequently. To mitigate this, we recommend a practice that may sound unusual: *prefer write-once variables*.

Variables that are a “permanent fixture” are easier to understand. Constants, such as:

```cpp
static const int NUM_THREADS = 10;
```

do not require much thought from the reader. This is why using `const` in C++ (and `final` in Java) is highly encouraged.

> The more locations a variable is manipulated, the harder it is to track its current value.

For example, suppose you have a web page with several input text fields structured as follows:

```html
<input type="text" id="input1" value="Dustin">
<input type="text" id="input2" value="Trevor">
<input type="text" id="input3" value="">
<input type="text" id="input4" value="Melissa">
...
```

Your task is to write a function named `setFirstEmptyInput()` that takes a string and inserts it into the first empty `<input>` on the page. The function should return the DOM element that was updated (or `null` if there are no empty inputs left). Here’s how the code might look, not adhering to the principles discussed in this chapter:

```jsx
var setFirstEmptyInput = function (new_value) {
  var found = false;
  var i = 1;
  var ele = document.getElementById('input' + i);

  while (ele !== null) {
    if (ele.value === '') {
      found = true;
      break;
    }

    i++;
    ele = document.getElementById('input' + i);
  }

  if (found) {
    ele.value = new_value;
  } 

  return ele;
};
```

In this code, the intermediate variable `found` can be eliminated by returning early. Here’s an improved version:

```jsx
var setFirstEmptyInput = function (new_value) {
  var i = 1;
  var ele = document.getElementById('input' + i);

  while (ele !== null) {
    if (ele.value === '') {
      ele.value = new_value;
      return ele;
    }

    i++;
    ele = document.getElementById('input' + i);
  }
  return null;
};
```

Next, we should consider `ele`. It is used multiple times throughout the code, leading to ambiguity regarding its value. We can restructure the `while` loop into a `for` loop that iterates over `i`:

```jsx
var setFirstEmptyInput = function (new_value) {
  for (var i = 1; true; i++) {
    var ele = document.getElementById('input' + i);
    if (ele === null)
      return null;  // Search Failed. No empty input was found.

    if (ele.value === '') {
      ele.value = new_value;
      return ele;
    }
  }
};
```

### Summary

This chapter discusses how the accumulation of variables can complicate understanding a program. To enhance code readability, consider the following strategies:

* Eliminate variables that add no value. We highlighted how to eliminate "intermediate result" variables by handling results immediately.
* Reduce the scope of each variable. Limit each variable's visibility to the smallest context possible so that fewer lines of code can see it. Out of sight means out of mind.
* Prefer write-once variables. Variables that are assigned only once are easier to comprehend.
