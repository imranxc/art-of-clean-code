# Chapter 6: Making Comments Precise and Compact

Effective commenting is crucial in programming, but it requires balancing detail with brevity. Comments should be precise, clear, and compact to maximize their usefulness without overwhelming the reader. This chapter delves into strategies for achieving this balance.

## Keep Comments Compact

Comments should convey their message in as few words as possible. For example, consider the following C++ type definition comment:

```cpp
// The int is the CategoryType.
// The first float in the inner pair is the 'score',
// The second is the 'weight'.
typedef hash_map<int, pair<float, float> > ScoreMap;
```

This can be condensed into a single, more informative line:

```cpp
// CategoryType -> (score, weight)
typedef hash_map<int, pair<float, float> > ScoreMap;
```

## Avoid Ambiguous Pronouns

Ambiguous pronouns can lead to confusion. It’s crucial to be specific about what "it" or "this" refers to. For instance:

```cpp
// Insert the data into the cache, but check if it's too big first.
```

Here, "it" is unclear—it could refer to either "data" or "cache." A clearer comment would be:

```cpp
// Insert the data into the cache, but check if the data is too big first.
```

Alternatively, you could rephrase it for clarity:

```cpp
// If the data is small enough, insert it into the cache.
```

## Polish Sloppy Sentences

Improving the clarity of comments involves making them more direct and specific. For example:

```py
# Depending on whether we've already crawled this URL before, 
# give it a different priority.
```

It can be rewritten for clarity:

```py
# Give higher priority to URLs we've never crawled before.
```

This revised comment is more direct and easier to understand.

## Describe Function Behavior Precisely

When documenting functions, be precise about their behavior. For instance, a function that counts lines in a file might be described as:

```cpp
// Return the number of lines in this file.
int countLines(string filename) {
  ...
}
```

This comment is vague because it doesn’t specify what constitutes a line. A more precise comment could be:

```cpp
// Count the number of newline characters ('\n') in the file.
int countLines(string filename) { 
  ... 
}
```

This comment clearly defines the criteria used for counting lines, helping to avoid misunderstandings about the function's behavior.

## State the Intent of Your Code

Rather than merely describing what the code does, it’s often more useful to comment on its purpose or intent. For example:

```cpp
void displayProducts(list<Product> products) {
  products.sort(compareProductByPrice);

  // Iterate through the list in reverse order
  for (list<Product>::reverse_iterator it = products.rbegin();
    it != products.rend(); ++it) {
    displayPrice(it->price);
  }
  ...
}
```

This comment could be improved to:

```cpp
// Display each product's price, from highest to lowest
for (list<Product>::reverse_iterator it = products.rbegin(); ... )
```

The revised comment explains the overall purpose of the code, providing a clearer understanding of what it aims to accomplish.

## “Named Function Parameter” Comments

In languages that don’t support named parameters (like C++ and Java), use inline comments to clarify function arguments. For example:

```c
void connect(int timeout, bool use_encryption) { ... }

// Call the function with commented parameters
connect(/* timeout_ms = */ 10, /* use_encryption = */ false);
```

Avoid ambiguous or redundant comments:

```cpp
// Avoid using vague comments
connect( ... , false /* use_encryption */);

// Avoid using misleading comments
connect( ... , false /* = use_encryption */);
```

Inline comments should be clear and concise to avoid confusion about the purpose of each argument.

### Use Information-Dense Words

Effective comments often use terminology that condenses complex ideas into concise phrases. For example:

```cpp
// This class contains some members that store the same information as in the
// database, but are stored here for speed. When this class is read from later, 
// those members are checked first to see if they exist, and if so are returned; 
// otherwise the database is read from and that data is stored in those fields for next time.
```

This can be simplified to:

```php
// This class acts as a "caching layer" to the database.
```

Similarly, replace verbose descriptions with industry-standard terms:

```cpp
// Remove excess whitespace from the street address, and do lots of other cleanup
// like turn "Avenue" into "Ave." This way, if there are two different street addresses
// that are typed in slightly differently will have the same cleaned-up version and
// We can detect that these are equal.
```

With:

```cpp
// Canonicalize the street address (remove extra spaces, "Avenue" -> "Ave.", etc.)
```

Using concise, industry-standard terms helps in conveying the intended meaning more efficiently.

## Summary

Writing comments that are both precise and compact enhances code readability and maintainability. Here are the specific tips:

* **Avoid pronouns**: Use specific references instead of ambiguous pronouns.
* **Be precise**: Define function behavior clearly, accounting for edge cases.
* **State intent**: Comment on the high-level purpose of code rather than the specifics of its implementation.
* **Clarify function parameters**: Use inline comments to explain function arguments in languages without named parameters.
* **Use concise terms**: Employ information-dense words and phrases to make comments more efficient and informative.
