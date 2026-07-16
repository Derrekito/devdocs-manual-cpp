# std::basic_istringstream<CharT,Traits,Allocator>::str

```cpp
std::basic_string<CharT, Traits, Allocator> str() const;  // (until C++20)
std::basic_string<CharT, Traits, Allocator> str() const&;  // (since C++20)
template< class SAlloc >
std::basic_string<CharT, Traits, SAlloc> str( const SAlloc& a ) const;  // (2) (since C++20)
std::basic_string<CharT, Traits, Allocator> str() &&;  // (3) (since C++20)
void str( const std::basic_string<CharT, Traits, Allocator>& s );  // (4)
template< class SAlloc >
void str( const std::basic_string<CharT, Traits, SAlloc>& s );  // (5) (since C++20)
void str( std::basic_string<CharT, Traits, Allocator>&& s );  // (6) (since C++20)
template< class StringViewLike >
void str( const StringViewLike& t );  // (7) (since C++26)
```

Manages the contents of the underlying string object.

1) Returns a copy of the underlying string. Equivalent to `return
   rdbuf()->str();`.

2) Returns a copy of the underlying string, using `a` as allocator. Equivalent
   to `return rdbuf()->str(a);`.

3) Returns a string move-constructed from the underlying string. Equivalent to
   `return std::move(*rdbuf()).str();`.

4,5) Replaces the contents of the underlying string. Equivalent to
   `rdbuf()->str(s);`.

6) Replaces the contents of the underlying string. Equivalent to
   `rdbuf()->str(std::move(s));`.

7) Replaces the contents of the underlying string. Equivalent to
   `rdbuf()->str(t);`.

This overload participates in overload resolution only if
   `is_convertible_v<const T&, basic_string_view<charT, traits>>` is `true`.

### Parameters

- **s** — new contents of the underlying string
- **t** — an object (convertible to `std::basic_string_view`) to use as the new
  contents of the underlying string
- **a** — allocator used to construct the returned string

### Return value

1,2) A copy of the underlying string object.

3) A string move-constructed from the underlying string object.

4-7) (none)

### Notes

The copy of the underlying string returned by `str` is a temporary object that
will be destructed at the end of the expression, so directly calling `c_str()`
on the result of `str()` (for example in `auto *ptr = out.str().c_str();`)
results in a dangling pointer.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_sstream_from_string_view` | 202306L | (C++26) | Interfacing
      `std::stringstream`s with `std::string_view`, (7)

### Example

```cpp
#include <iostream>
#include <sstream>

int main()
{
    int n;

    std::istringstream in; // could also use in("1 2")
    in.str("1 2");
    in >> n;
    std::cout << "After reading the first int from \"1 2\", the int is "
              << n << ", str() = \"" << in.str() << "\"\n";

    std::ostringstream out("1 2");
    out << 3;
    std::cout << "After writing the int '3' to output stream \"1 2\""
              << ", str() = \"" << out.str() << "\"\n";

    std::ostringstream ate("1 2", std::ios_base::ate);
    ate << 3;
    std::cout << "After writing the int '3' to append stream \"1 2\""
              << ", str() = \"" << ate.str() << "\"\n";
}
```

Output:

```text
After reading the first int from "1 2", the int is 1, str() = "1 2"
After writing the int '3' to output stream "1 2", str() = "3 2"
After writing the int '3' to append stream "1 2", str() = "1 23"
```

### See also

- **rdbuf** — returns the underlying raw string device object (public member
  function)
- **str** — replaces or obtains a copy of the associated character string
  (public member function of `std::basic_stringbuf<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_istringstream/str*
