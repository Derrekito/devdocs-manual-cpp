# std::source_location::source_location

```cpp
constexpr source_location() noexcept;  // (1) (since C++20)
source_location( const source_location& other );  // (2) (since C++20)
source_location( source_location&& other ) noexcept;  // (3) (since C++20)
```

1) Constructs a `source_location` object of unspecified value.

2,3) Copy and move constructors. It is unspecified whether they are trivial
   and/or constexpr.

### Parameters

- **other** — another `source_location` to copy or move from

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <ranges>
#include <source_location>
#include <string_view>
#include <vector>

// GCC specific type name printer
#if (__GNUG__ >= 11)
template<typename T>
auto type_name_helper(const std::source_location s = std::source_location::current())
{
    using std::operator""sv;
    const std::string_view fun_name{s.function_name()};
    constexpr auto prefix{"[with T = "sv};
    const auto type_name_begin{fun_name.find(prefix)};
    if (""sv.npos == type_name_begin)
        return ""sv;
    const std::size_t first{type_name_begin + prefix.length()};
    return std::string_view{fun_name.cbegin() + first, fun_name.cend() - 1};
}

template<typename T>
auto type_name() { return type_name_helper<T>(); }
#endif

void print(std::string_view const comment, std::source_location const l)
{
    std::cout << comment << ":\n"
              << "  file_name     : " << std::quoted(l.file_name()) << '\n'
              << "  function_name : " << std::quoted(l.function_name()) << '\n'
              << "  line          : " << l.line() << '\n'
              << "  column        : " << l.column() << '\n';
}

int main()
{
    constexpr std::source_location default_constructed;
    print("default constructed", default_constructed);

    constexpr std::source_location current = std::source_location::current();
    print("current", current);

#if (__GNUG__ >= 11)
    const std::vector<std::vector<int>> v{{1,2}, {3,4,5}, {6}};
    auto jv = std::ranges::join_view(v);
    std::cout << '\n'
              << '[' << type_name<int>() << "]\n"
              << '[' << type_name<double*>() << "]\n"
              << '[' << type_name<decltype([](){})>() << "]\n"
              << '[' << type_name<decltype(type_name<int>())>() << "]\n"
              << '[' << type_name<decltype(jv)>() << "]\n";
#endif
}
```

Possible output:

```text
default constructed:
  file_name     : ""
  function_name : ""
  line          : 0
  column        : 0
current:
  file_name     : "main.cpp"
  function_name : "int main()"
  line          : 39
  column        : 75

[int]
[double*]
[main()::<lambda()>]
[std::basic_string_view<char>]
[std::ranges::join_view<std::ranges::ref_view<const std::vector<std::vector<int> > > >]
```

### See also

- **current [static]** — constructs a new `source_location` corresponding to the
  location of the call site (public static member function)
- **(constructor) (C++23)** — constructs a new `stacktrace_entry` (public member
  function of `std::stacktrace_entry`)

---
*Source: https://en.cppreference.com/w/cpp/utility/source_location/source_location*
