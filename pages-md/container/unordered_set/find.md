# std::unordered_set<Key,Hash,KeyEqual,Allocator>::find

```cpp
iterator find( const Key& key );  // (1)
const_iterator find( const Key& key ) const;  // (2)
template< class K >
iterator find( const K& x );  // (3) (since C++20)
template< class K >
const_iterator find( const K& x ) const;  // (4) (since C++20)
```

1,2) Finds an element with key equivalent to `key`.

3,4) Finds an element with key that compares *equivalent* to the value `x`. This
   overload participates in overload resolution only if `Hash::is_transparent`
   and `KeyEqual::is_transparent` are valid and each denotes a type. This
   assumes that such `Hash` is callable with both `K` and `Key` type, and that
   the `KeyEqual` is transparent, which, together, allows calling this function
   without constructing an instance of `Key`.

### Parameters

- **key** — key value of the element to search for
- **x** — a value of any type that can be transparently compared with a key

### Return value

Iterator to an element with key equivalent to `key`. If no such element is
found, past-the-end (see `end()`) iterator is returned.

### Complexity

Constant on average, worst case linear in the size of the container.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_generic_unordered_lookup` | 201811L | (C++20) | Heterogeneous
      comparison lookup in unordered associative containers; overloads (3,4)

### Example

```cpp
#include <cstddef>
#include <functional>
#include <iostream>
#include <source_location>
#include <string>
#include <string_view>
#include <unordered_set>

using namespace std::literals;

namespace logger { bool enabled{false}; }

inline void who(const std::source_location sloc = std::source_location::current())
{
    if (logger::enabled)
        std::cout << sloc.function_name() << '\n';
}

struct string_hash // C++20's transparent hashing
{
    using hash_type = std::hash<std::string_view>;
    using is_transparent = void;

    std::size_t operator()(const char* str) const        { who(); return hash_type{}(str); }
    std::size_t operator()(std::string_view str) const   { who(); return hash_type{}(str); }
    std::size_t operator()(std::string const& str) const { who(); return hash_type{}(str); }
};

int main()
{
    std::unordered_set<int> example = {1, 2, -10};

    std::cout << "Simple comparison demo:\n" << std::boolalpha;
    if (auto search = example.find(2); search != example.end())
        std::cout << "Found " << *search << '\n';
    else
        std::cout << "Not found\n";

    std::unordered_set<std::string, string_hash, std::equal_to<>> set{"one"s, "two"s};

    logger::enabled = true;
    std::cout << "Heterogeneous lookup for unordered containers (transparent hashing):\n"
              << (set.find("one")   != set.end()) << '\n'
              << (set.find("one"s)  != set.end()) << '\n'
              << (set.find("one"sv) != set.end()) << '\n';
}
```

Possible output:

```text
Simple comparison demo:
Found 2
Heterogeneous lookup for unordered containers (transparent hashing):
std::size_t string_hash::operator()(const char*) const
true
std::size_t string_hash::operator()(const std::string&) const
true
std::size_t string_hash::operator()(std::string_view) const
true
```

### See also

- **count** — returns the number of elements matching specific key (public
  member function)
- **equal_range** — returns range of elements matching a specific key (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_set/find*
