# Chapter 5: Knowing What to Comment

You might think the purpose of commenting is simply to **explain what the code does**, but that’s just a small part of it.

> The true purpose of commenting is to help the reader understand as much as the writer did when writing the code.

When you write code, you have a wealth of information in your head. When others read your code, that context is lost—all they have is the code in front of them.

## What Not to Comment

### Redundant Comments

Comments that simply restate what the code is doing are often redundant. For example:

```cpp
// The class definition for Account
class Account {
  public:
  // Constructor
  Account();

  // Set the profit member to a new value
  void setProfit(double profit);

 // Return the profit from this Account
  double getProfit();
};
```

These comments add no real value because the code itself is already clear about what each part does. 

### Commenting Just for the Sake of It

Some programmers feel compelled to comment on every detail of their code, even if the comments don't add value. For instance:

```cpp
// Find the Node in the given subtree, with the given name, using the given depth.
Node* findNodeInSubtree(Node* subtree, string name, int depth);
```

Instead of reiterating the function’s purpose, it’s more useful to explain important nuances:

```cpp
// Find a Node with the given 'name' or return NULL.
// If depth <= 0, only 'subtree' is inspected.
// If depth == N, only 'subtree' and N levels below are inspected.
Node* FindNodeInSubtree(Node* subtree, string name, int depth);
```

### Commenting Bad Names—Fix the Names Instead

Avoid using comments to explain poorly named functions or variables. Instead, use descriptive names that convey the function’s purpose. For example:

```cpp
// Enforce limits on the Reply as stated in the Request,
// such as the number of items returned, or total byte size, etc.
void cleanReply(Request request, Reply reply);
```

Instead, use a more self-explanatory name:

```cpp
// Make sure 'reply' meets the count/byte/etc. limits from the 'request'
void enforceLimitsFromRequest(Request request, Reply reply);
```

Similarly:

```cpp
// Releases the handle for this key. This doesn't modify the actual registry.
void deleteRegistry(RegistryKey* key);
```

A more descriptive name might be:

```cpp
void releaseRegistryHandle(RegistryKey* key);
```

Good code should ideally minimize the need for comments, adhering to the principle: **good code > bad code + good comments**.

## What to Comment

### Record Your Thoughts

Comments should capture valuable insights that aren’t immediately obvious from the code. For example:

```java
// Surprisingly, a binary tree was 40% faster than a hash table for this data.
// The cost of computing a hash was more than the left/right comparisons.
```

This comment provides useful context for why a particular data structure was chosen.

Similarly:

```cpp
// This heuristic might miss a few words. That's OK; solving this 100% is hard.
```

This comment prevents confusion by explaining the trade-offs involved.

You can also use comments to suggest improvements:

```cpp
// This class is getting messy. Maybe we should create a 'ResourceNode' subclass to help organize things.
```

This acknowledges the code’s shortcomings and suggests a way forward.

### Comment on Constants

When defining constants, provide context or rationale for their values:

```py
NUM_THREADS = 8
```

A comment can explain why 8 was chosen:

```py
# as long as it's >= 2 * num_processors, that's good enough.
NUM_THREADS = 8
```

Or explain the purpose of the limit:

```c
// Impose a reasonable limit - no human can read that much anyway.
const int MAX_RSS_SUBSCRIPTIONS = 1000;
```

Sometimes, constants are tuned based on experience:

```py
# users thought 0.72 gave the best size/quality tradeoff
image_quality = 0.72
```

These comments help others understand the reasoning behind the chosen values.

### Put Yourself in the Reader’s Shoes

Anticipate which parts of your code might be confusing or require additional explanation. For example:

```cpp
struct Recorder {
  vector<float> data;
  ...
  void clear() {
    vector<float>().swap(data);  // Huh? Why not just data.clear()?
  }
};
```

This comment explains a non-obvious optimization detail:

```cpp
// Force vector to relinquish its memory (look up "STL swap trick")
vector<float>().swap(data);
```

Additionally, use comments to provide an overview of the code:

* **File/Class Level Comments**: Explain the high-level purpose and how different pieces fit together.
* **Block-Level Comments**: Summarize the purpose of code blocks to avoid readers getting lost in details.

## Summary

The purpose of comments is to help readers understand the context and nuances of your code that may not be immediately clear from the code alone. Focus on:

* What Not to Comment:
  * Avoid commenting on self-explanatory code.
  * Don’t use comments to compensate for poor code design; improve the code itself.
* What to Comment:
  * Share valuable insights and reasoning behind decisions.
  * Explain constants and their values.
  * Anticipate confusion and address it with comments.
  * Provide high-level overviews and summaries to guide readers through complex code.