---
layout: post
title: Logical operators don't return boolean values in Python
comments: true
published: true
image: https://homepages.inf.ed.ac.uk/rbf/HIPR2/figs/ttaband.gif
excerpt: Here's a quick question for you: if you "and" two values in Python, what is the type of the return? A boolean, right? Nope.
---


Here's a quick question for you: if you `and` two values in Python, what is the type of the return? A boolean, right? Nope. Not necessarily.

Here's an example:

```python
>>> "a" and "b"
'b'
```

What's going on here? The reason for this behavior is clearer (at least for me) when you consider how the `or` operator is commonly used in Python as a way to conditionally assign values in Python in a single line:

```python
name = user_value or "Ahmed" # If user_value is falsy, name becomes "Ahmed"
```


So when `or` is used in Python, the operands are evaluated one by one and the first non-falsy operand is returned. If all arguments are falsy, the last one is returned.

Conversely, when `and` is used, again the operands are evaluated one by one and first *falsy* one is returned. If all arguments are non-falsy, the last one is returned.

This is provides a nice symmetry and allows both `or` and `and` to be used for concise conditional assignment. 

In practice, I see `and` used much less frequently for this purpose. However there are some handy use cases -- for example, can you tell what's happening in this code snippet?

```python
config_port = get_port()  # Returns int, str, or None

port = config_port and int(config_port) or 8000
```

So to summarize:
* `or` returns the first non-falsy operand, or the last value
* `and` returns the first falsy operand, or the last value

--------

*Note*: When Python needs a boolean value (e.g. in an `if` statement), it converts arguments into a `boolean` --> all falsy values are treated as `False` and truthy values as `True`, which is why the `and` and `or` keywords can be overloaded in the manner above.

