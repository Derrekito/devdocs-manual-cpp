# std::remove, std::remove_if

`remove` does **not** shrink the container. It shifts the elements
you're keeping to the front, preserving their relative order, and
returns an iterator to the new logical end — everything from there to
the old `last` is left with unspecified values, and the container's
size is unchanged. You must follow it with a call to `erase` to
actually drop the tail (the *erase–remove idiom*):
`v.erase(std::remove(v.begin(), v.end(), value), v.end());`. Since
C++20, `std::erase`/`std::erase_if` do both steps in one call for
standard sequence (`erase`) or any (`erase_if`) container.

```cpp skip
std::remove(first, last, value);               // erase-remove: drop == value
std::remove_if(first, last, pred);              // erase-remove: drop matching
std::remove(policy, first, last, value);        // parallel        (since C++17)
std::remove_if(policy, first, last, pred);       // parallel + pred (since C++17)
std::erase(container, value);                    // does both steps (since C++20)
std::erase_if(container, pred);                  // does both steps (since C++20)
```

`constexpr` since C++20.

### What you provide

- **first, last** — forward iterators bounding the range.
- **value** — elements equal to this (via `operator==`) are dropped.
- **pred** — `remove_if` only: a callable returning `true` for
  elements to drop; must not modify what it's given.
- **policy** — an execution policy (C++17) for parallel execution.

### Guarantees and costs

- `remove`: exactly `N` comparisons with `value`, where
  `N = std::distance(first, last)`.
- `remove_if`: exactly `N` predicate applications.
- Relative order of kept elements is preserved.
- Iterators between the returned iterator and the old `last` stay
  dereferenceable but hold unspecified values.
- Parallel overloads: if a predicate throws, `std::terminate` is
  called; allocation failure throws `std::bad_alloc`.

### Gotchas

- Forgetting the `erase` call is the classic bug: the container's
  `size()` doesn't change, so "removed" elements are still iterated
  and counted until you erase the tail.
- Won't work on `std::set`/`std::map` — their iterators don't
  dereference to mutable (MoveAssignable) types, since keys can't be
  reordered in place.
- `std::list`/`std::forward_list` have their own member
  `remove`/`remove_if` that erase directly — prefer those over the
  free function on those containers.
- `value` is taken by reference: passing a reference to an element
  inside `[first, last)` (e.g. `std::remove(v.begin(), v.end(),
  v[0])`) can misbehave once that element gets overwritten mid-shift.
- `<cstdio>` also declares a `std::remove` that deletes a file by
  path — an unrelated overload pulled in by `using namespace std`.

### Example

```cpp c++20
#include <algorithm>
#include <cctype>
#include <iostream>
#include <string>
#include <string_view>

int main()
{
    std::string str1{"Text with some   spaces"};

    auto noSpaceEnd = std::remove(str1.begin(), str1.end(), ' ');

    // Spaces are gone only logically; the string isn't shrunk yet.
    std::cout << std::string_view(str1.begin(), noSpaceEnd)
              << " size: " << str1.size() << '\n';

    str1.erase(noSpaceEnd, str1.end());

    // Now physically shrunk.
    std::cout << str1 << " size: " << str1.size() << '\n';
}
```

```text
Textwithsomespaces size: 23
Textwithsomespaces size: 18
```

### Reference

Full declarations:

```cpp skip
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

`ForwardIt` must meet LegacyForwardIterator; the value type must be
MoveAssignable (CopyAssignable until C++11) or behavior is undefined —
shifting is implemented with move-assignment. LWG 283 (applied
retroactively to C++98): `T` was originally required to be
EqualityComparable with the value type required CopyAssignable; the
corrected requirement is that the value type of `ForwardIt` be
CopyAssignable/MoveAssignable, full stop.

### See also

- **remove_copy** — copies a range, omitting elements matching criteria
- **unique** — removes consecutive duplicate elements
- **erase** — free function combining remove + erase (C++20)
- **erase_if** — free function combining remove_if + erase, any container (C++20)
- **ranges::remove** — constrained version (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/remove*
