# Chapter 8: Breaking Down Giant Expressions

The giant squid is an incredible creature, but its nearly perfect body design has a critical flaw: its donut-shaped brain wraps around its esophagus. If it swallows too much food at once, it can suffer brain damage.

How does this relate to coding? Just like the giant squid's brain, complex chunks of code can cause confusion and errors. Research shows that most people can only focus on three or four concepts at a time. The more complex an expression is, the harder it becomes to understand.

## Explaining Variables

One effective way to simplify a complex expression is to use an extra variable to capture a smaller subexpression. This "explaining variable" helps clarify what the subexpression represents.

For example:

```py
if line.split(":")[0].strip() == "root":
  pass
```

This can be rewritten with an explaining variable:

```py
user_name = line.split(":")[0].strip()

if user_name == 'root':
  pass
```

## Summary Variables

Even if an expression is clear, it can still be beneficial to assign it to a new variable. This "summary variable" replaces a larger chunk of code with a smaller, more manageable name.

Consider the following code:

```py
if request.user.id == document.owner_id:
  # user can edit

if request.user.id != document.owner_id:
  # user can't edit
```

The main concept here is, “Does the user own the document?” This can be stated more clearly by introducing a summary variable:

```py
user_owns_document = (request.user.id == document.owner_id)

if user_owns_document:
  # user can edit

if not user_owns_document:
  # user can't edit
```

## Using De Morgan’s Laws

De Morgan’s laws help rewrite Boolean expressions in simpler forms. 

These laws state:

* not (a or b or c) ⇔ (not a) and (not b) and (not c)
* not (a and b and c) ⇔ (not a) or (not b) or (not c)

Using these laws can make Boolean expressions more readable.

For example:

```cpp
if (!(file_exists && !is_protected)){
  raise Error("Sorry, could not read file.");
}
```

This can be rewritten as:

```cpp
if (!file_exists || is_protected) {
  raise Error("Sorry, could not read file.");
}
```

### Abusing Short-Circuit Logic

Short-circuit logic can be powerful, but it's easy to overuse and make code harder to understand.

```cpp
assert((!(bucket = findBucket(key))) || !bucket->isOccupied());
```

In plain English, this code is saying, “Get the bucket for this key. If the bucket is not null, then make sure it isn’t occupied.”

```cpp
bucket = findBucket(key);

if (bucket != NULL) {
  assert(!bucket->isOccupied());
}
```

While this version is longer but easier to understand.

> Be cautious with "clever" code — it can be confusing to others who read your code later.

## Breaking Down Giant Statements

Consider this complex function:

```jsx
let update_highlight = function (message_num) {
  if ($("#vote_value" + message_num).html() === "Up") {
    $("#thumbs_up" + message_num).addClass("highlighted");
    $("#thumbs_down" + message_num).removeClass("highlighted");
  } else if ($("#vote_value" + message_num).html() === "Down") {
    $("#thumbs_up" + message_num).removeClass("highlighted");
    $("#thumbs_down" + message_num).addClass("highlighted");
  } else {
    $("#thumbs_up" + message_num).removeClass("highlighted"); 
    $("#thumbs_down" + message_num).removeClass("highlighted");
  }
}
```

We can simplify this by using summary variables to avoid repeating code:

```jsx
let update_highlight = function (message_num) {
  let vote_value = $("#vote_value" + message_num).html();
  let thumbs_up = $("#thumbs_up" + message_num);
  let thumbs_down = $("#thumbs_down" + message_num);
  let hi = "highlighted";

  if (vote_value === "Up") {
    thumbs_up.addClass(hi);
    thumbs_down.removeClass(hi);
  } else if (vote_value === "Down") {
    thumbs_up.removeClass(hi);
    thumbs_down.addClass(hi);
  } else {
    thumbs_up.removeClass(hi);
    thumbs_down.removeClass(hi);
  }
}
```

### Summary

Giant expressions can be overwhelming. This chapter covered several techniques for breaking them down into more digestible pieces:

1. **Explaining Variables**: These capture subexpressions and make the code easier to understand.
2. **Summary Variables**: They simplify and clarify the main concepts of your code.
3. **De Morgan’s Laws**: Use these laws to rewrite Boolean expressions in a clearer way.
4. **Avoid Clever Short-Circuit Logic**: Simpler, more straightforward code is often easier to read and maintain.
