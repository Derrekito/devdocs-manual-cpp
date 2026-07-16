# std::source_location::column

```cpp
constexpr std::uint_least32_t column() const noexcept;  // (since C++20)
```

Returns an implementation-defined value representing some offset from the start
of the line represented by this object (i.e., the column number). Column numbers
are presumed to be 1-indexed.

### Parameters

(none)

### Return value

An implementation-defined value representing some offset from the start of the
line represented by this object (i.e., the column number).

An implementation is encouraged to use `0` when the column number is unknown.

### Example

```cpp
#include <iostream>
#include <source_location>

template<typename T = std::source_location>
inline void pos(const T& location = T::current())
{
    std::cout
        << "("
        << location.line()
        << ':'
        << location.column()
        << ") ";
}

int main()
{
//      ↓: column #9
    pos(); std::cout << "Proxima\n";    // row #18
      pos(); std::cout << "Centauri\n"; // row #19
//        ↑: column #11
}
```

Possible output:

```text
(18:9) Proxima
(19:11) Centauri
```

### See also

- **line** — return the line number represented by this object (public member
  function)
- **file_name** — return the file name represented by this object (public member
  function)
- **function_name** — return the name of the function represented by this
  object, if any (public member function)

**Filename and line information**

---
*Source: https://en.cppreference.com/w/cpp/utility/source_location/column*
