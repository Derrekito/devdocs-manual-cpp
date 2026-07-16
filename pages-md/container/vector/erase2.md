# std::erase, std::erase_if (std::vector)

```cpp
template< class T, class Alloc, class U >
constexpr typename std::vector<T, Alloc>::size_type
    erase( std::vector<T, Alloc>& c, const U& value );  // (1) (since C++20)
template< class T, class Alloc, class Pred >
constexpr typename std::vector<T, Alloc>::size_type
    erase_if( std::vector<T, Alloc>& c, Pred pred );  // (2) (since C++20)
```

1) Erases all elements that compare equal to `value` from the container.
   Equivalent to auto it = std::remove(c.begin(), c.end(), value); auto r =
   c.end() - it; c.erase(it, c.end()); return r;

2) Erases all elements that satisfy the predicate `pred` from the container.
   Equivalent to auto it = std::remove_if(c.begin(), c.end(), pred); auto r =
   c.end() - it; c.erase(it, c.end()); return r;

### Parameters

- **c** — container from which to erase
- **value** — value to be removed
- **pred** — unary predicate which returns ​`true` if the element should be
  erased. The expression `pred(v)` must be convertible to `bool` for every
  argument `v` of type (possibly const) `T`, regardless of value category, and
  must not modify `v`. Thus, a parameter type of `T&`is not allowed, nor is `T`
  unless for `T` a move is equivalent to a copy(since C++11). ​

### Return value

The number of erased elements.

### Complexity

Linear.

### Example

```cpp
#include <iostream>
#include <numeric>
#include <string_view>
#include <vector>

void print_container(std::string_view comment, const std::vector<char>& c)
{
    std::cout << comment << "{ ";
    for (char x : c)
        std::cout << x << ' ';
    std::cout << "}\n";
}

int main()
{
    std::vector<char> cnt(10);
    std::iota(cnt.begin(), cnt.end(), '0');
    print_container("Initially, cnt = ", cnt);

    std::erase(cnt, '3');
    print_container("After erase '3', cnt = ", cnt);

    auto erased = std::erase_if(cnt, [](char x) { return (x - '0') % 2 == 0; });
    print_container("After erase all even numbers, cnt = ", cnt);
    std::cout << "Erased even numbers: " << erased << '\n';
}
```

Output:

```text
Initially, cnt = { 0 1 2 3 4 5 6 7 8 9 }
After erase '3', cnt = { 0 1 2 4 5 6 7 8 9 }
After erase all even numbers, cnt = { 1 5 7 9 }
Erased even numbers: 5
```

### See also

- **removeremove_if** — removes elements satisfying specific criteria (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/erase2*
