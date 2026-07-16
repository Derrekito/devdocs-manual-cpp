# std::multiset<Key,Compare,Allocator>::rend, std::multiset<Key,Compare,Allocator>::crend

```cpp
reverse_iterator rend();  // (until C++11)
reverse_iterator rend() noexcept;  // (since C++11)
const_reverse_iterator rend() const;  // (until C++11)
const_reverse_iterator rend() const noexcept;  // (since C++11)
const_reverse_iterator crend() const noexcept;  // (3) (since C++11)
```

Returns a reverse iterator to the element following the last element of the
reversed `multiset`. It corresponds to the element preceding the first element
of the non-reversed `multiset`. This element acts as a placeholder, attempting
to access it results in undefined behavior.

### Parameters

(none)

### Return value

Reverse iterator to the element following the last element.

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
    std::multiset<unsigned> rep{1, 2, 3, 4, 1, 2, 3, 4};

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
⏼ ⏼ ⏼ ⏼
⏼ ⏼ ⏼
⏼ ⏼ ⏼
⏼ ⏼
⏼ ⏼
⏼
⏼
```

### See also

- **rbegincrbegin (C++11)** — returns a reverse iterator to the beginning
  (public member function)
- **rendcrend (C++14)** — returns a reverse end iterator for a container or
  array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/multiset/rend*
