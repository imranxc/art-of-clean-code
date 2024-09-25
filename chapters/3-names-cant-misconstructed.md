# Chapter 3: Names That Can’t Be Misconstrued

In software development, naming is a critical aspect that can greatly impact code clarity and maintainability. One of the key challenges is choosing names that convey their intended meaning unambiguously. This chapter focuses on how to avoid names that might be misunderstood and how to choose names that are clear and precise.

## Scrutinizing Names for Ambiguity

When naming functions, variables, or constants, it’s crucial to actively consider how they might be misinterpreted. A name that seems clear at first glance can sometimes be misleading or ambiguous. To avoid this, ask yourself: **What other meanings could someone interpret from this name?** This step involves creatively thinking through possible misinterpretations to identify and correct any ambiguities.

### Examples of Ambiguous Names

#### Example 1: filter()

In programming, the `filter` function is commonly used to extract a subset of items that meet certain criteria. However, the term `filter` can be ambiguous:

```py
results = Model.objects.filter("year__lte=2011")
```

Here’s the potential confusion:

* Objects whose year is `<= 2011`? (i.e., pick objects with year less than or equal to 2011)
* Objects whose year is not `<= 2011`? (i.e., exclude objects with year less than or equal to 2011)

To clarify intent:

* If you mean “to pick out,” use `select()`.
* If you mean “to exclude,” use `exclude()`.

#### Example 2: clip(text, length)

Consider a function designed to clip the contents of a text to a specified length:

```py
# Cuts off the end of the text, and appends "..."
def clip(text, length):
  ...
```

There are two possible interpretations:

* It removes length characters from the end of the text.
* It truncates the text to a maximum of length characters.

The most intuitive interpretation is truncation. To avoid ambiguity, rename the function to `truncate(text, max_chars)`:

* Use `truncate` to indicate the text is cut off.
* Specify `max_chars` to clarify that the length is measured in characters.

#### Example 3: CART_TOO_BIG_LIMIT

In a shopping cart application:

```py
CART_TOO_BIG_LIMIT = 10

if shopping_cart.num_items() >= CART_TOO_BIG_LIMIT:
  raise Error("Too many items in cart.")
```

This name is ambiguous because it's unclear whether the limit is inclusive or exclusive:

* **Inclusive**: 10 items allowed, 11 items trigger an error.
* **Exclusive**: 10 items trigger an error.

To avoid this confusion, use `MAX_ITEMS_IN_CART = 10`:

* The name `MAX_ITEMS_IN_CART` implies a maximum limit.

## Inclusive vs. Exclusive Ranges

When dealing with ranges, clarity on inclusivity is important.

### Inclusive Ranges

For ranges where both endpoints are included, use `first` and `last`:

```py
set.printKeys(first="Bart", last="Maggie")
```

Alternatively, `min` and `max` can work if contextually appropriate.

### Exclusive Ranges

For ranges where one endpoint is exclusive, use `begin` and `end`:

```py
printEventsInRange("OCT 16 12:00am", "OCT 17 12:00am")
```

Using `begin` and `end` clearly distinguishes between inclusive and exclusive ranges.

## Naming Booleans

When naming boolean variables, clarity is paramount to avoid confusion between different states.

Ambiguous Example:

```cpp
bool read_password = true;
```

This can be confusing:

* **Read the password** or **password has already been read**?

Instead, use names that indicate state or action:

* `need_password` (indicates whether a password is needed)
* `user_is_authenticated` (indicates the authentication status)

Avoid using negated terms:

```cpp
bool use_ssl = true;
```

Rather than `disable_ssl = false`, which might be more confusing.

## Method Names and User Expectations

Be aware of common conventions to avoid misleading users:

Misleading Example:

```java
public class StatisticsCollector {
  public void addSample(double x) { ... }
  
  public double getMean() {
    // Iterate through all samples and return total / num_samples
    }
  ...
}
```

The term `get` often implies a lightweight accessor, but `getMean()` involves expensive computation. Rename it to `computeMean()` to signal that it's a potentially costly operation.

## Summary

Choosing clear and unambiguous names is crucial for writing maintainable and understandable code. Names should convey their exact purpose and avoid any potential for misinterpretation. Consider the following guidelines:

* **Be specific**: Use descriptive names that convey exactly what a function or variable does.
* **Avoid ambiguity**: Think about possible alternative interpretations and address them.
* **Use clear prefixes**: For limits, use `max` and `min`. For ranges, use `first`, `last`, `begin`, and `end`.
* **Name booleans clearly**: Use `is`, `has`, `can`, or `should`, and avoid negations.
* **Follow conventions**: Be mindful of common expectations around method names like `get`.
