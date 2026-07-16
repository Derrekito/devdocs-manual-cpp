# C++ attribute: nodiscard (since C++17)

If a function declared `nodiscard` or a function returning an enumeration or
class declared `nodiscard` by value is called from a discarded-value expression
other than a cast to `void`, the compiler is encouraged to issue a warning.

### Syntax

```text
[[nodiscard]]    (1)
[[nodiscard( string-literal )]]    (2)    (since C++20)
```

- **string-literal** — an unevaluated string literal that could be used to
  explain the rationale for why the result should not be discarded

### Explanation

Appears in a function declaration, enumeration declaration, or class
declaration.

If, from a discarded-value expression other than a cast to `void`,

- a function declared `nodiscard` is called, or
- a function returning an enumeration or class declared `nodiscard` by value is
  called, or
- a constructor declared `nodiscard` is called by explicit type conversion or
  `static_cast`, or
- an object of an enumeration or class type declared `nodiscard` is initialized
  by explicit type conversion or `static_cast`,

the compiler is encouraged to issue a warning.

The string-literal, if specified, is usually included in the warnings.
*(since C++20)*

### Example

```cpp
struct [[nodiscard]] error_info { /*...*/ };
error_info enable_missile_safety_mode() { /*...*/ return {}; }
void launch_missiles() { /*...*/ }
void test_missiles()
{
    enable_missile_safety_mode(); // compiler may warn on discarding a nodiscard value
    launch_missiles();
}
error_info& foo() { static error_info e; /*...*/ return e; }
void f1() { foo(); } // nodiscard type is not returned by value, no warning
// nodiscard( string-literal ) (since C++20):
[[nodiscard("PURE FUN")]] int strategic_value(int x, int y) { return x ^ y; }

int main()
{
    strategic_value(4, 2); // compiler may warn on discarding a nodiscard value
    auto z = strategic_value(0, 0); // ok: return value is not discarded
    return z;
}
```

Possible output:

```text
game.cpp:5:4: warning: ignoring return value of function declared with 'nodiscard' attribute
game.cpp:17:5: warning: ignoring return value of function declared with 'nodiscard' attribute: PURE FUN
```

### Standard library

The following standard functions are declared with `nodiscard` attribute:

**Allocation functions**

- **operator newoperator new[]** — allocation functions (function)
- **allocate** — allocates uninitialized storage (public member function of
  `std::allocator<T>`)
- **allocate [static]** — allocates uninitialized storage using the allocator
  (public static member function of `std::allocator_traits<Alloc>`)
- **allocate** — allocates memory (public member function of
  `std::pmr::memory_resource`)
- **allocate** — allocate memory (public member function of
  `std::pmr::polymorphic_allocator<T>`)
- **allocate** — allocates uninitialized storage using the outer allocator
  (public member function of
  `std::scoped_allocator_adaptor<OuterAlloc,InnerAlloc...>`)

**Indirect access**

- **launder (C++17)** — pointer optimization barrier (function template)
- **assume_aligned (C++20)** — informs the compiler that a pointer is aligned
  (function template)

**Emptiness-checking functions**

- **empty (C++17)** — checks whether the container is empty (function template)
- **empty** — checks whether the node handle is empty (public member function of
  `node handle`)
- **empty** — checks whether the container is empty (public member function of
  `std::array<T,N>`)
- **empty** — checks whether the string is empty (public member function of
  `std::basic_string<CharT,Traits,Allocator>`)
- **empty** — checks whether the view is empty (public member function of
  `std::basic_string_view<CharT,Traits>`)
- **empty** — checks whether the container is empty (public member function of
  `std::deque<T,Allocator>`)
- **empty** — checks whether the container is empty (public member function of
  `std::forward_list<T,Allocator>`)
- **empty** — checks whether the container is empty (public member function of
  `std::list<T,Allocator>`)
- **empty** — checks whether the container is empty (public member function of
  `std::map<Key,T,Compare,Allocator>`)
- **empty** — checks whether the match was successful (public member function of
  `std::match_results<BidirIt,Alloc>`)
- **empty** — checks whether the container is empty (public member function of
  `std::multimap<Key,T,Compare,Allocator>`)
- **empty** — checks whether the container is empty (public member function of
  `std::multiset<Key,Compare,Allocator>`)
- **empty** — checks whether the container adaptor is empty (public member
  function of `std::priority_queue<T,Container,Compare>`)
- **empty** — checks whether the container adaptor is empty (public member
  function of `std::queue<T,Container>`)
- **empty** — checks whether the container is empty (public member function of
  `std::set<Key,Compare,Allocator>`)
- **empty** — checks if the sequence is empty (public member function of
  `std::span<T,Extent>`)
- **empty** — checks whether the container adaptor is empty (public member
  function of `std::stack<T,Container>`)
- **empty** — checks whether the container is empty (public member function of
  `std::unordered_map<Key,T,Hash,KeyEqual,Allocator>`)
- **empty** — checks whether the container is empty (public member function of
  `std::unordered_multimap<Key,T,Hash,KeyEqual,Allocator>`)
- **empty** — checks whether the container is empty (public member function of
  `std::unordered_multiset<Key,Hash,KeyEqual,Allocator>`)
- **empty** — checks whether the container is empty (public member function of
  `std::unordered_set<Key,Hash,KeyEqual,Allocator>`)
- **empty** — checks whether the container is empty (public member function of
  `std::vector<T,Allocator>`)
- **empty** — checks if the path is empty (public member function of
  `std::filesystem::path`)

**Miscellaneous**

- **async (C++11)** — runs a function asynchronously (potentially in a new
  thread) and returns a `std::future` that will hold the result (function
  template)

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P1771R1 | C++17 | `[[nodiscard]]` on constructors has no effect | can cause a
      warning if the constructed object is discarded

### See also

- **ignore (C++11)** — placeholder to skip an element when unpacking a `tuple`
  using `tie` (constant)

**C documentation for `nodiscard`**

---
*Source: https://en.cppreference.com/w/cpp/language/attributes/nodiscard*
