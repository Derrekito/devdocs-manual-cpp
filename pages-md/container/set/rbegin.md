# std::set<Key,Compare,Allocator>::rbegin, std::set<Key,Compare,Allocator>::crbegin

```cpp
reverse_iterator rbegin();  // (until C++11)
reverse_iterator rbegin() noexcept;  // (since C++11)
const_reverse_iterator rbegin() const;  // (until C++11)
const_reverse_iterator rbegin() const noexcept;  // (since C++11)
const_reverse_iterator crbegin() const noexcept;  // (3) (since C++11)
```

Returns a reverse iterator to the first element of the reversed `set`. It
corresponds to the last element of the non-reversed `set`. If the `set` is
empty, the returned iterator is equal to `rend()`.

### Parameters

(none)

### Return value

Reverse iterator to the first element.

### Complexity

Constant.

### Notes

Because both `iterator` and `const_iterator` are constant iterators (and may in
fact be the same type), it is not possible to mutate the elements of the
container through an iterator returned by any of these member functions.

### Example

```cpp
#include <iostream>
#include <set>

int main()
{
    std::set<unsigned> rep{1, 2, 3, 4, 1, 2, 3, 4};

    for (auto it = rep.crbegin(); it != rep.crend(); ++it)
    {
        for (auto n = *it; n > 0; --n)
            std::cout << "⏼" << ' ';
        std::cout << '\n';
    }
}
```

Output:

```text
⏼ ⏼ ⏼ ⏼
⏼ ⏼ ⏼
⏼ ⏼
⏼
```

### See also

- **rendcrend (C++11)** — returns a reverse iterator to the end (public member
  function)
- **rbegincrbegin (C++14)** — returns a reverse iterator to the beginning of a
  container or array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/set/rbegin*
