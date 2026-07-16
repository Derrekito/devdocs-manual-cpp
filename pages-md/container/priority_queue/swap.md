# std::priority_queue<T,Container,Compare>::swap

```cpp
void swap( priority_queue& other ) noexcept(/* see below */);  // (since C++11)
```

Exchanges the contents of the container adaptor with those of `other`.
Effectively calls

```cpp
using std::swap;
swap(c, other.c);
swap(comp, other.comp);
```

### Parameters

- **other** — container adaptor to exchange the contents with

### Return value

(none)

### Exceptions

`noexcept` specification:
`noexcept(noexcept(swap(c, other.c)) && noexcept(swap(comp, other.comp)))`
In the expression above, the identifier `swap` is looked up in the same manner
as the one used by the C++17 `std::is_nothrow_swappable` trait.
*(since C++11)(until C++17)*

`noexcept` specification:
`noexcept(std::is_nothrow_swappable_v<Container> &&
std::is_nothrow_swappable_v<Compare>)`
*(since C++17)*

### Complexity

Same as underlying container (typically constant).

### Notes

Some implementations (e.g. libc++) provide the `swap` member function as an
extension to pre-C++11 modes.

### Example

```cpp
#include <iostream>
#include <concepts>
#include <functional>
#include <queue>
#include <string>
#include <string_view>
#include <vector>

template<typename Adaptor>
requires (std::ranges::input_range<typename Adaptor::container_type>)
void print(std::string_view name, const Adaptor& adaptor)
{
    struct Printer : Adaptor // to use protected Adaptor::Container c;
    {
        void print(std::string_view name) const
        {
            std::cout << name << " [" << std::size(this->c) << "]: ";
            for (auto const& elem : this->c)
                std::cout << elem << ' ';
            std::cout << '\n';
        }
    };

    static_cast<Printer const&>(adaptor).print(name);
}

int main()
{
    std::vector<std::string> v1{"1","2","3","4"},
                             v2{"Ɐ","B","Ɔ","D","Ǝ"};

    std::priority_queue s1(std::less<>(), std::move(v1));
    std::priority_queue s2(std::less<>(), std::move(v2));

    print("s1", s1);
    print("s2", s2);

    s1.swap(s2);

    print("s1", s1);
    print("s2", s2);
}
```

Output:

```text
s1 [4]: 4 3 2 1
s2 [5]: Ǝ D Ɔ B Ɐ
s1 [5]: Ǝ D Ɔ B Ɐ
s2 [4]: 4 3 2 1
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2456 | C++11 | the `noexcept` specification is ill-formed | made to work

### See also

- **std::swap(std::priority_queue) (C++11)** — specializes the `std::swap`
  algorithm (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/priority_queue/swap*
