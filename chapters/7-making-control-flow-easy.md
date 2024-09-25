# Chapter 7: Making Control Flow Easy to Read

When it comes to programming, control flow—the way our code makes decisions and repeats actions—can quickly become complex. This chapter is all about making sure that your control flow is clear and easy to follow.

## The Order of Arguments in Conditionals

The readability of conditionals often depends on the order in which the arguments are presented. Consider the following two pieces of code:

```py
if length >= 10: pass
# versus
if 10 <= length: pass
```

Most programmers find the first version more readable. This preference is generally because:

* **Left-hand side**: The left-hand side usually represents the value that is more variable or subject to change.
* **Right-hand side**: The right-hand side typically represents a constant or reference value that remains relatively stable.

> **Tip**: Place the variable or changing value on the left side of the comparison and the constant on the right. This aligns with the reader’s natural thought process, making the code easier to follow.

## The Order of if/else blocks

When you’re writing `if/else` blocks, how you order them can really impact readability. Here’s how to decide:

### Positive Case First 

It’s often clearer to handle what’s true or expected before dealing with the negative case.

```cpp
if (url.hasQueryParameter("expand_all")) {
  for (int i = 0; i < items.size(); i++) {
    items[i].expand();
  }
} else {
  response.render(items);
}
```

This way, the reader sees the more likely or positive outcome first, making the code more intuitive.

### Simpler Case First

If one case is straightforward, handle it first. It makes the code easier to follow.

```py
def process_numbers(numbers):
  # Handle the simplest case first
  if not numbers:
    return

  total = 0
  for number in numbers:
    total += number

  average = total / len(numbers)
  print(f"Total: {total}")
  print(f"Average: {average}")
```

By handling the simpler cases up front, you keep the more complex logic less cluttered.

### Interesting Case First

If one case is particularly important or interesting, consider addressing it first.

```py
if file:
  # process the file
else:
  # log the error
```

## The ?: Conditional Expression 

The ternary operator `?:` can make code more concise, but its impact on readability can be controversial. Here’s an example:

```cpp
time_str += (hour >= 12) ? "pm" : "am";
```

For simple cases like this, it’s clear and efficient. But for more complex conditions, a full `if/else` statement might be easier to read:

```cpp
if (hour >= 12) {
  time_str += "pm";
} else {
  time_str += "am";
}```
```

> **Advice**: Use the ternary operator for simple, straightforward cases. For more complex conditions, stick with if/else to keep things clear.

## Returning Early from a Function

Sometimes, handling special cases right away can make your code easier to understand. Instead of having everything nested, you handle simple cases at the start and get them out of the way:

```java
public boolean contains(String str, String substr) {
  if (str == null || substr == null) {
    return false;
  }

  if (substr.equals("")) {
    return true;
  }

  // Additional logic for non-empty substrings
  return str.contains(substr);
}
```

By returning early, you keep the main logic clean and straightforward.

## Minimize Nesting

Deeply nested code can be confusing because it adds multiple layers of conditions that you have to keep track of. Here’s a way to reduce that complexity.

Original nested code:

```cpp
if (user_result == SUCCESS) {
  if (permission_result != SUCCESS) {
    reply.writeErrors("error reading permissions.");
    reply.done();
    return;
  }
  reply.writeErrors("");
} else {
  reply.writeErrors(user_result);
}

reply.done();
```

Refactored to reduce nesting:

```cpp
if (user_result != SUCCESS) {
  reply.writeErrors(user_result);
  reply.done();
  return;
}

if (permission_result != SUCCESS) {
  reply.writeErrors(permission_result);
  reply.done();
  return;
}

reply.writeErrors("");
reply.done();
```

This approach simplifies the logic and reduces the need to keep track of multiple nested conditions.

## Removing Nesting by Returning Early

When you’re working inside loops, you can use continue to skip over parts of the loop that don’t need to be processed, reducing nesting.

Original loop with nesting:

```cpp
for (int i = 0; i < results.size(); i++) {
  if (results[i] != NULL) {
    non_null_count++;

    if (results[i]->name != "") {
      cout << "Considering candidate..." << endl;
    }
  }
}
```

Refactored loop with continue:

```cpp
for (int i = 0; i < results.size(); i++) {
  if (results[i] == NULL) {
    continue;
  }

  non_null_count++;

  if (results[i]->name == "") {
    continue;
  }

  cout << "Considering candidate..." << endl;
}
```

## Summary

To enhance the readability of control flow in your code, consider the following practices:

* **Order of Comparisons**: Place the variable or fluctuating value on the left side of comparisons.
* **Order of if/else Blocks**: Handle positive, simpler, or more interesting cases first.
* **Conditional Expressions**: Use ternary operators sparingly and prefer `if/else` for more complex conditions.
* **Early Returns**: Simplify logic and reduce nesting by returning early from functions.
* **Minimize Nesting**: Reduce the depth of nesting to make code easier to follow.
* **Use Continue in Loops**: Employ `continue` to avoid deep nesting inside loops.
