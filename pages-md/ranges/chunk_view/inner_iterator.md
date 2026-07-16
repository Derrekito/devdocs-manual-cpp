# std::ranges::chunk_view<V>::*inner-iterator*

```cpp
class /*inner-iterator*/  // (since C++23) (exposition only*)
```

The return type of `chunk_view::outer-iterator::value_type::begin` if `V` models
`input_range`.

### Member types

- **`iterator_concept`** — `std::input_iterator_tag`
- **`difference_type`** — `ranges::range_difference_t<V>`
- **`value_type`** — `ranges::range_value_t<V>`

### Data members

- **`parent_` (private)** — A pointer to the "parent object" of type
  `ranges::chunk_view*`. (exposition-only member object*)

### Member functions

- **(constructor) (C++23)** — constructs an iterator (public member function)
- **operator= (C++23)** — move assigns another iterator (public member function)
- **base (C++23)** — returns an iterator to the current element (public member
  function)
- **operator* (C++23)** — accesses the element (public member function)
- **operator++ (C++23)** — increments the iterator (public member function)

### Non-member functions

- **operator== (C++23)** — compares the iterator with default sentinel
  (function)
- **operator- (C++23)** — calculates the remained number of elements (function)
- **iter_move (C++23)** — casts the result of dereferencing the underlying
  iterator to its associated rvalue reference type (function)
- **iter_swap (C++23)** — swaps the objects pointed to by two underlying
  iterators (function)

### Example

```cpp
#include <iostream>
#include <iterator>
#include <ranges>
#include <sstream>

int main()
{
    auto letters = std::istringstream{"ABCDEFGHIJK"};

    auto chunks = std::ranges::istream_view<char>(letters)
                | std::views::chunk(4);

    for (auto chunk : chunks)
    {
        // chunk is an object of type chunk_view::outer_iterator::value_type
        std::cout << '[';
        for (auto inner_iter = chunk.begin(); inner_iter != std::default_sentinel;
             ++inner_iter)
            std::cout << *inner_iter;
        std::cout << "] ";
    }
    std::cout << '\n';
}
```

Output:

```text
[ABCD] [EFGH] [IJK]
```

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/inner_iterator*
