# C++ named requirements: Predicate

The **Predicate** requirements describe a callable that returns a value testable
as a `bool`.

Predicate is typically used with algorithms that take input data (individual
objects/containers) and a predicate, which is then called on input data to
decide on further course of action. Some examples of predicate usage in C++
standard library are:

- `std::all_of`, `std::any_of`, `std::none_of` Take an array of elements and a
  predicate as an input. Call predicate on individual input elements, and return
  true if for all/any/none elements, predicate returns true.
- `std::find_if` Take sequence of elements, and a predicate. Return first
  element in the sequence, for which predicate returns value equal to `true`.

Description of algorithm facilities, given above, is crude and intended to
explain Predicate in simple terms. For detailed info, refer to individual pages.

In other words, if an algorithm takes a Predicate `pred` and an iterator
`first`, it should be able to test the object of the type pointed to by the
iterator `first` using the given predicate via a construct like
`if(pred(*first)) {...}`.

The function object `pred` shall not apply any non-constant function through the
dereferenced iterator and must accept a `const` object argument, with the same
behavior regardless of whether its argument is `const` or non-`const`(since
C++20). This function object may be a pointer to function or an object of a type
with an appropriate function call operator.

### Requirements

- FunctionObject

### See also

- **predicate (C++20)** — specifies that a callable type is a Boolean predicate
  (concept)

---
*Source: https://en.cppreference.com/w/cpp/named_req/Predicate*
