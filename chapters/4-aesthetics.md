# Chapter 4: Aesthetics

Aesthetic principles for writing clear, maintainable code include:

* **Use a Consistent Layout**: Adhere to layout patterns that readers can familiarize themselves with.
* **Make Similar Code Look Similar**: Ensure that similar code structures are visually similar.
* **Group Related Lines of Code into Blocks**: Organize code into logical blocks to enhance readability.

## Consistent Layout

**Principle**: Maintain a consistent layout so that readers can easily follow your code structure.

### Before:

```cpp
class StatsKeeper {
  public:
    // A class for keeping track of a series of doubles
    void add(double d);  // and methods for quick statistics about them
  private: 
    int count;  /* how many so far  */ 
  public:
    double average();

  private:   
    double minimum;
    list<double> past_items;
    double maximum;
};
```

### After:

```cpp
// A class for keeping track of a series of doubles and methods for quick statistics about them.
class StatsKeeper {
  public:
    void add(double d);
    double average();

  private:
    list<double> past_items;
    int count; // how many so far
    double minimum;
    double maximum;
};
```

## Rearranging Line Breaks

**Principle**: Introduce line breaks for consistency and compactness to enhance readability.

### Before:

```java
public class PerformanceTester {
  public static final TcpConnectionSimulator wifi = new TcpConnectionSimulator(
    500, /* Kbps */
    80, /* millisecs latency */
    200, /* jitter */
    1 /* packet loss % */);

  public static final TcpConnectionSimulator t3_fiber = new TcpConnectionSimulator(
    45000, /* Kbps */
    10, /* millisecs latency */
    0, /* jitter */
    0 /* packet loss % */);

  public static final TcpConnectionSimulator cell = new TcpConnectionSimulator(
    100, /* Kbps */
    400, /* millisecs latency */
    250, /* jitter */
    5 /* packet loss % */);
}
```

### After:

```java
public class PerformanceTester {
  public static final TcpConnectionSimulator wifi =
    new TcpConnectionSimulator(
      500,   /* Kbps */
      80,    /* millisecs latency */
      200,   /* jitter */
      1      /* packet loss % */);

  public static final TcpConnectionSimulator t3_fiber =
    new TcpConnectionSimulator(
      45000, /* Kbps */
      10,    /* millisecs latency */
      0,     /* jitter */
      0      /* packet loss % */);

  public static final TcpConnectionSimulator cell =
    new TcpConnectionSimulator(
      100,   /* Kbps */
      400,   /* millisecs latency */
      250,   /* jitter */
      5      /* packet loss % */);
}
```

**More Compact**:

```java
public class PerformanceTester {
  // TcpConnectionSimulator(throughput, latency, jitter, packet_loss)
  //                           [Kbps]   [ms]    [ms]    [percent]

  public static final TcpConnectionSimulator wifi =
    new TcpConnectionSimulator(500,    80,     200,    1);

  public static final TcpConnectionSimulator t3_fiber =
    new TcpConnectionSimulator(45000,  10,     0,      0);

  public static final TcpConnectionSimulator cell =
    new TcpConnectionSimulator(100,    400,    250,    5);
}
```

## Use Methods to Clean Up Irregularity

**Principle**: Abstract repetitive or complex testing into helper methods to improve clarity.

### Before:

```cpp
DatabaseConnection database_connection;
string error;

assert(expandFullName(database_connection, "Doug Adams", &error)
  == "Mr. Douglas Adams");
assert(error == "");

assert(expandFullName(database_connection, " Jake  Brown ", &error)
  == "Mr. Jacob Brown III");
assert(error == "");

assert(expandFullName(database_connection, "No Such Guy", &error) == "");
assert(error == "no match found");

assert(expandFullName(database_connection, "John", &error) == "");
assert(error == "more than one result");
```

### After:

```cpp
void CheckFullName(
  string partial_name,
  string expected_full_name,
  string expected_error
) {
  string error;
  string full_name = ExpandFullName(database_connection, partial_name, &error);
  
  assert(error == expected_error);
  assert(full_name == expected_full_name);
}

CheckFullName("Doug Adams", "Mr. Douglas Adams", "");
CheckFullName(" Jake  Brown ", "Mr. Jacob Brown III", "");
CheckFullName("No Such Guy", "", "no match found");
CheckFullName("John", "", "more than one result");
```

## Use Column Alignment When Helpful

**Principle**: Align code into columns to improve readability and ease of understanding.

You could space out and line up the arguments to `checkFullName()`.

```cpp
checkFullName("Doug Adams"   , "Mr. Douglas Adams" , "");
checkFullName(" Jake  Brown ", "Mr. Jake Brown III", "");
checkFullName("No Such Guy"  , ""                  , "no match found");
checkFullName("John"         , ""                  , "more than one result");
```

It’s easier to distinguish the second and third arguments to `checkFullName()`.

## Pick a Meaningful Order, and Use It Consistently

**Principle**: Order elements in a meaningful and consistent manner to avoid confusion.

### Before:

```py
details  = request.POST.get('details')
location = request.POST.get('location')
phone    = request.POST.get('phone')
email    = request.POST.get('email')
url      = request.POST.get('url')
```

### After:

```py
# Order by the HTML form input fields
email    = request.POST.get('email')
phone    = request.POST.get('phone')
location = request.POST.get('location')
url      = request.POST.get('url')
details  = request.POST.get('details')
```

## Break Code into “Paragraphs”

**Principle**: Break large blocks of code into smaller, logical sections for better readability and structure.

### Before:

No one likes to read a giant lump of code like this:

```py
# Import the user's email contacts, and match them to users in our system.
# Then display a list of those users that he/she isn't already friends with.
def suggest_new_friends(user, email_password):
  friends = user.friends()
  friend_emails = set(f.email for f in friends)
  contacts = import_contacts(user.email, email_password)
  contact_emails = set(c.email for c in contacts)
  non_friend_emails = contact_emails - friend_emails
  suggested_friends = User.objects.select(email__in=non_friend_emails)
  display['user'] = user
  display['friends'] = friends
  display['suggested_friends'] = suggested_friends
  return render("suggested_friends.html", display)
```

### After:

So it would be especially useful to break up those lines of code into paragraphs:

```py
def suggest_new_friends(user, email_password):
  # Get the user's friends' email addresses.
  friends = user.friends()
  friend_emails = set(friend.email for friend in friends)

  # Import all email addresses from this user's email account.
  contacts = import_contacts(user.email, email_password)
  contact_emails = set(contact.email for contact in contacts)

  # Find matching users that they aren't already friends with.
  non_friend_emails = contact_emails - friend_emails
  suggested_friends =  User.objects.select(email__in=non_friend_emails)

  # Display these lists on the page.
  display['user'] = user
  display['friends'] = friends
  display['suggested_friends'] = suggested_friends

  return render("suggested_friends.html", display)
```

> **Tip**: Consistent style is more important than the `right` style.

## Summary

Adhering to aesthetic principles in code formatting improves both readability and maintainability. By consistently applying these principles—using a uniform layout, making similar code visually similar, grouping related lines, aligning columns, and breaking code into logical paragraphs—you enhance the clarity and structure of your code.

### Key Techniques:

* **Consistent Layout**: Standardize your code layout to foster familiarity.
* **Similarity in Code**: Ensure similar code structures look alike.
* **Logical Grouping**: Organize related code into cohesive blocks.
* **Column Alignment**: Use alignment for better visual separation.
* **Meaningful Order**: Maintain a logical and consistent order for elements.
* **Paragraphs in Code**: Break down large blocks into manageable sections.

