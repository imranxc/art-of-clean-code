# Chapter 2: Packing Information into Names

Effective naming is crucial in coding because names often serve as a mini-comment explaining what a piece of code does. Here's how to pack more information into names to make your code clearer and more maintainable.

## Choose Specific Words

Instead of generic words like `get`, use more descriptive names:

* `get_page(url)` could become `fetch_page(url)` or `download_page(url)`.

For methods in a class:

```cpp
class BinaryTree() {
    int size(){
        ...
    }
};
```

`size()` in a `BinaryTree` class might be better named `height()`, `numNodes()`, or `memoryBytes()`, depending on what it measures.

For actions like stopping a thread:

```cpp
class Thread {
    void stop() {
    ...
    };
    ...
};
```

Use `stop()` if it's a normal stop, but consider `kill()` for an irreversible action or `pause()` if it can be resumed.

## Finding More Descriptive Words

Instead of generic verbs, opt for more descriptive alternatives:

* send → `deliver`, `dispatch`, `route`
* find → `search`, `locate`, `retrieve`
* start → `launch`, `initiate`
* make → `create`, `generate`

> Tip: Avoid being overly creative with names. Aim for clarity.

### Avoid Generic Names Like `tmp` and `retval`

Generic names often mean a lack of thought. Instead of `retval`, use:

* `sumSquares` for accumulating squared values in a norm calculation.

### When to Use Generic Names

`tmp` is acceptable for short-lived variables used only within a small scope, like in a swap operation:

```jsx
let tmp = right;
right = left;
left = tmp;
```

But if the temporary variable holds significant user information:

* Use `user_info` instead of `tmp`.

> The name `tmp` should be used only in cases when being short-lived and temporary is the most important fact about that variable.

## Loop Iterators

Names like `i`, `j`, `iter`, and `it` are commonly used as indices and loop iterators. Even though these names are generic.

But sometimes there are better iterator names than `i`, `j`, and `k`. For instance, the following loops find which users belong to which clubs.

```cpp
for (int i = 0; i < clubs.size(); i++)
  for (int j = 0; j < clubs[i].members.size(); j++)
    for (int k = 0; k < users.size(); k++)
      if (clubs[i].members[k] == users[j]) {
         cout << "user[" << j << "] is in club[" << i << "]" << endl;
      }
```

In the `if` statement, `members[]` and `users[]` are using the wrong index. Bugs like these are hard to spot.

In this case, using more precise names may have helped. Replace `i`, `j`, `k` with `clubIndex`, `memberIndex`, `userIndex` if it helps understanding. This approach would help the bug stand out more.

## Prefer Concrete Names over Abstract Names

Names should describe functionality directly.

For example, suppose you have an internal method named `serverCanStart()`, which tests whether the server can  listen on a given TCP/IP port. The name `serverCanStart()` is somewhat abstract, though. 

A more concrete name would be `canListenOnPort()`. This name directly describes what the method will do.

## Attaching Extra Information to a Name

If there’s something very important about a variable that the reader must know, it’s worth attaching an extra “word” to the name. For example:

* `hex_id` for hexadecimal identifiers.
* `user_info` instead of `tmp` when more context is needed.

### Values with Units

Encode units in variable names if variable is a measurement.

JavaScript code that measures the load time of a web page:

```jsx
// top of the page
let start = new Date().getTime(); // returns milliseconds, not seconds.
...
// bottom of the page
let elapsed = new Date().getTime() - start;
document.writeln("Load time was: " + elapsed + " seconds");
```

By appending `_ms` to our variables, we can make everything more explicit:

```jsx 
// top of the page
let start_ms = new Date().getTime();
...
// bottom of the page
let elapsed_ms = new Date().getTime() - start_ms;
document.writeln("Load time was: " + elapsed_ms / 1000 + " seconds");
```

Examples of better naming:

* `delay_secs` instead of `delay`.
* `size_mb` instead of `size`.
* `max_kbps` instead of `limit`.
* `degrees_cw` instead of `angle`.

## Encoding Other Important Attributes

Many security exploits come from not realizing that some data your program receives is not yet in a safe state. 

For this, you might want to use variable names like `unTrustedUrl` or `unSafeMessageBody`. After calling functions that cleanse the unsafe input, the resulting variables might be `trustedUrl` or `safeMessageBody`.

## How Long Should a Name Be?

* **Short Names**: Suitable for variables with small scope, e.g., `m` in a small function.
* **Longer Names**: Necessary for variables with larger scope, e.g., `days_since_last_update`.

This decision is a judgment call whose best answer depends on exactly how that variable is being used.

### Throwing Out Unneeded Words

Trim names by removing redundant words:

* `toString()` instead of `convertToString()`.
* `serveLoop()` instead of `doServeLoop()`.

## Use Name Formatting to Convey Meaning

Consistent formatting helps with readability:

* `kMaxOpenFiles` for constants.
* `offset_` for class member variables.

```cpp
static const int kMaxOpenFiles = 100;

class LogReader {
  public:
    void openFile(string local_file);

  private:
    int offset_;
    DISALLOW_COPY_AND_ASSIGN(LogReader);
};
```

For instance, constant values are of the form `kConstantName` instead of `CONSTANT_NAME`. This style has the benefit of being easily distinguished from `#define` macros, which are `MACRO_NAME` by convention.

Class member variables are like normal variables, but must end with an underscore, like `offset_`. At first, this convention may seem strange, but being able to instantly distinguish members from other variables is very handy.

## Summary

* **Be Specific**: Use precise terms like `fetch` or `download` instead of generic ones like `get`.
* **Avoid Generic Names**: Steer clear of vague names such as `tmp` or `retval`.
* **Be Concrete**: Use descriptive names, e.g., `CanListenOnPort()` instead of `ServerCanStart()`.
* **Include Details**: Append suffixes like `_ms` for milliseconds or `raw_` for unprocessed data.
* **Scope Matters**: Use longer, more descriptive names for variables with larger scopes; shorter names are fine for local, short-lived variables.
* **Use Naming Conventions**: Employ capitalization, underscores, etc., meaningfully to differentiate between variables and class members.
