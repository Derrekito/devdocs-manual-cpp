# std::remove, std::remove_if

```cpp
template< class ForwardIt, class T >
ForwardIt remove( ForwardIt first, ForwardIt last, const T& value );  // (until C++20)
template< class ForwardIt, class T >
constexpr ForwardIt remove( ForwardIt first, ForwardIt last, const T& value );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class T >
ForwardIt remove( ExecutionPolicy&& policy,
                  ForwardIt first, ForwardIt last, const T& value );  // (2) (since C++17)
template< class ForwardIt, class UnaryPredicate >
ForwardIt remove_if( ForwardIt first, ForwardIt last, UnaryPredicate p );  // (until C++20)
template< class ForwardIt, class UnaryPredicate >
constexpr ForwardIt remove_if( ForwardIt first, ForwardIt last,
                               UnaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt, class UnaryPredicate >
ForwardIt remove_if( ExecutionPolicy&& policy,
                     ForwardIt first, ForwardIt last, UnaryPredicate p );  // (4) (since C++17)
```

Removes all elements satisfying specific criteria from the range
`[``first``,``last``)` and returns a past-the-end iterator for the new end of
the range.

1) Removes all elements that are equal to `value` (using `operator==`).

3) Removes all elements for which predicate `p` returns `true`.

2,4) Same as (1,3), but executed according to `policy`. These overloads do not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

If the value type of `ForwardIt` is not CopyAssignable, the behavior is
undefined.
*(until C++11)*

If the type of `*first` is not MoveAssignable, the behavior is undefined.
*(since C++11)*

Removing is done by shifting (by means of copy assignment(until C++11)move
assignment(since C++11)) the elements in the range in such a way that the
elements that are not to be removed appear in the beginning of the range.
Relative order of the elements that remain is preserved and the *physical* size
of the container is unchanged. Iterators pointing to an element between the new
*logical* end and the *physical* end of the range are still dereferenceable, but
the elements themselves have unspecified values (as per MoveAssignable
post-condition)(since C++11).

### Parameters

- **first, last** — the range of elements to process
- **value** — the value of elements to remove
- **policy** — the execution policy to use. See execution policy for details.
- **p** — unary predicate which returns ​`true` if the element should be
  removed. The expression `p(v)` must be convertible to `bool` for every
  argument `v` of type (possibly const) `VT`, where `VT` is the value type of
  `ForwardIt`, regardless of value category, and must not modify `v`. Thus, a
  parameter type of `VT&`is not allowed, nor is `VT` unless for `VT` a move is
  equivalent to a copy(since C++11). ​

**Type requirements**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-`UnaryPredicate` must meet the requirements of Predicate.**

### Return value

Past-the-end iterator for the new range of values (if this is not `end`, then it
points to an unspecified value, and so do iterators to any values between this
iterator and `end`).

### Complexity

Given `N` as `std::distance(first, last)`:

1,2) exactly `N` comparisons with `value` using `operator==`.

3,4) exactly `N` applications of the predicate `p`.

### Exceptions

The overloads with a template parameter named `ExecutionPolicy` report errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Notes

A call to `remove` is typically followed by a call to a container's `erase`
member function, which erases the unspecified values and reduces the *physical*
size of the container to match its new *logical* size. These two invocations
together constitute a so-called *Erase–remove* idiom, which can be achieved by
the free function `std::erase` that has overloads for all standard *sequence*
containers, or `std::erase_if` that has overloads for *all* standard
containers(since C++20).

The similarly-named container member functions `list::remove`,
`list::remove_if`, `forward_list::remove`, and `forward_list::remove_if` erase
the removed elements.

These algorithms cannot be used with associative containers such as `std::set`
and `std::map` because their iterator types do not dereference to MoveAssignable
types (the keys in these containers are not modifiable).

The standard library also defines an overload of `std::remove` in `<cstdio>`,
which takes a `const char*` and is used to delete files.

Because `std::remove` takes `value` by reference, it can have unexpected
behavior if it is a reference to an element of the range `[``first``,``last``)`.

### Possible implementation

```cpp
template<class ForwardIt, class T>
ForwardIt remove(ForwardIt first, ForwardIt last, const T& value)
{
    first = std::find(first, last, value);
    if (first != last)
        for (ForwardIt i = first; ++i != last;)
            if (!(*i == value))
                *first++ = std::move(*i);
    return first;
}
```

```cpp
template<class ForwardIt, class UnaryPredicate>
ForwardIt remove_if(ForwardIt first, ForwardIt last, UnaryPredicate p)
{
    first = std::find_if(first, last, p);
    if (first != last)
        for (ForwardIt i = first; ++i != last;)
            if (!p(*i))
                *first++ = std::move(*i);
    return first;
}
```

### Example

The following code removes all spaces from a string by shifting all non-space
characters to the left and then erasing the extra. This is an example of
Erase-remove idiom.

```cpp
#include <algorithm>
#include <cctype>
#include <iostream>
#include <string>
#include <string_view>

int main()
{
    std::string str1{"Text with some   spaces"};

    auto noSpaceEnd = std::remove(str1.begin(), str1.end(), ' ');

    // The spaces are removed from the string only logically.
    // Note, we use view, the original string is still not shrunk:
    std::cout << std::string_view(str1.begin(), noSpaceEnd)
              << " size: " << str1.size() << '\n';

    str1.erase(noSpaceEnd, str1.end());

    // The spaces are removed from the string physically.
    std::cout << str1 << " size: " << str1.size() << '\n';

    std::string str2 = "Text\n with\tsome \t  whitespaces\n\n";
    str2.erase(std::remove_if(str2.begin(),
                              str2.end(),
                              [](unsigned char x) { return std::isspace(x); }),
               str2.end());
    std::cout << str2 << '\n';
}
```

Output:

```text
Textwithsomespaces size: 23
Textwithsomespaces size: 18
Textwithsomewhitespaces
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 283 | C++98 | `T` was required to be EqualityComparable, but the value
      type of `ForwardIt` is not always `T` | required the value type of
      `ForwardIt` to be CopyAssignable instead

### See also

- **remove_copyremove_copy_if** — copies a range of elements omitting those that
  satisfy specific criteria (function template)
- **unique** — removes consecutive duplicate elements in a range (function
  template)
- **ranges::removeranges::remove_if (C++20)(C++20)** — removes elements
  satisfying specific criteria (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/remove*
