# std::ranges::chunk_view<V>::*outer-iterator*

```cpp
class /*outer-iterator*/  // (since C++23) (exposition only*)
```

The return type of `chunk_view::begin` if `V` models `input_range`.

### Member types

- **`iterator_concept`** — `std::input_iterator_tag`
- **`difference_type`** — `ranges::range_difference_t<V>`

### Data members

- **`parent_` (private)** — A pointer to the "parent object" of type
  `ranges::chunk_view*` (exposition-only member object*)

### Member functions

- **(constructor) (C++23)** — constructs an iterator (public member function)
- **operator= (C++23)** — move assigns another iterator (public member function)
- **operator* (C++23)** — accesses the element (public member function)
- **operator++ (C++23)** — increments the iterator (public member function)

### Non-member functions

- **operator== (C++23)** — compares the iterator with default sentinel
  (function)
- **operator- (C++23)** — calculates the number of chunks remaining (function)

### Nested classes

- **value_type (C++23)** — the value type of `/*output-iterator*/` (public
  member class)

### Example

```cpp
#include <iostream>
#include <iterator>
#include <ranges>
#include <sstream>

int main()
{
    const std::string source{"ABCDEFGHIJ"};

    auto letters = std::istringstream{source};
    auto chunks = std::ranges::istream_view<char>(letters)
                | std::views::chunk(4);

    for (auto outer_iter = chunks.begin(); outer_iter != std::default_sentinel;
         ++outer_iter)
    {
        auto chunk = *outer_iter; // chunk is an object of type
                                  // chunk_view::outer_iterator::value_type
        std::cout << '[';
        for (auto inner_iter = chunk.begin(); inner_iter != std::default_sentinel;
             ++inner_iter)
            std::cout << *inner_iter;
        std::cout << "] ";
    }
    std::cout << '\n';

    // The same output using range-for loops
    auto letters2 = std::istringstream{source};
    auto chunks2 = std::ranges::istream_view<char>(letters2)
                 | std::views::chunk(4);
    for (auto chunk : chunks2)
    {
        std::cout << '[';
        for (auto ch : chunk)
            std::cout << ch;
        std::cout << "] ";
    }
    std::cout << '\n';
}
```

Output:

```text
[ABCD] [EFGH] [IJ]
[ABCD] [EFGH] [IJ]
```

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/outer_iterator*
