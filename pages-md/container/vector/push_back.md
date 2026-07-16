# std::vector<T,Allocator>::push_back

Appends `value` to the end of the vector (copying or moving it in).
Amortized O(1). The one thing to know before you call it: if this push
makes `size()` exceed the old `capacity()`, the vector reallocates and
**every** iterator, pointer, and reference into it is invalidated;
otherwise only the `end()` iterator is.

```cpp skip
v.push_back(value);             // copies value
v.push_back(std::move(value));  // moves value          (since C++11)
```

### What you provide

- **value** — the element to append. The copy overload needs `T` to be
  CopyInsertable; the move overload needs `T` to be MoveInsertable.

### Guarantees and costs

- Amortized constant time; an individual call may reallocate and
  copy/move every existing element.
- Reallocation (new `size()` > old `capacity()`): all iterators,
  pointers, and references to the vector's elements are invalidated,
  `end()` included.
- No reallocation: only the `end()` iterator is invalidated; iterators,
  pointers, and references to existing elements stay valid.
- Strong exception guarantee: if `push_back` throws, the vector is
  unchanged. Exception *(since C++11)*: if `T`'s move constructor isn't
  `noexcept` and `T` isn't CopyInsertable, vector falls back to the
  throwing move constructor — if that throws, the guarantee is waived
  and the effects are unspecified.
- Some implementations throw `std::length_error` if the reallocation
  this call implies would exceed `max_size()`.

### Gotchas

- Don't assume an iterator survives a `push_back` — it only does when
  the call didn't reallocate, and you usually can't be sure of that
  without checking `capacity()` yourself.
- A reference or pointer taken before a reallocating `push_back` is
  dangling afterward, even though nothing looks wrong until you use it.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <string>
#include <vector>

int main()
{
    std::vector<std::string> letters;

    letters.push_back("abc");
    std::string s{"def"};
    letters.push_back(std::move(s));

    std::cout << "letters holds: ";
    for (auto&& e : letters)
        std::cout << std::quoted(e) << ' ';

    std::cout << "\nmoved-from s holds: " << std::quoted(s) << '\n';
}
```

```text
letters holds: "abc" "def" 
moved-from s holds: ""
```

### Reference

```cpp skip
void push_back( const T& value );  // (until C++20)
constexpr void push_back( const T& value );  // (since C++20)
void push_back( T&& value );  // (since C++11) (until C++20)
constexpr void push_back( T&& value );  // (since C++20)
```

No return value.

### See also

- **emplace_back** (C++11) — constructs the new element in place, no
  separate copy or move
- **pop_back** — removes the last element
- **back_inserter** — output iterator adaptor that calls `push_back`

---
*Source: https://en.cppreference.com/w/cpp/container/vector/push_back*
