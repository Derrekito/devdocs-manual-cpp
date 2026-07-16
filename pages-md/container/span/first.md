# std::span<T,Extent>::first

```cpp
template< std::size_t Count >
constexpr std::span<element_type, Count> first() const;  // (since C++20)
constexpr std::span<element_type, std::dynamic_extent> first( size_type Count ) const;  // (since C++20)
```

Obtains a span that is a view over the first `Count` elements of this span. The
program is ill-formed if `Count > Extent`. The behavior is undefined if `Count >
size()`.

### Return value

A span `r` that is a view over the first `Count` elements of `*this`, such that
`r.data() == this->data() && r.size() == Count`.

### Example

```cpp
#include <iostream>
#include <ranges>
#include <span>
#include <string_view>

void print(std::string_view const title,
           std::ranges::forward_range auto const& container)
{
    auto size{std::size(container)};
    std::cout << title << '[' << size << "]{";
    for (auto const& elem : container)
        std::cout << elem << (--size ? ", " : "");
    std::cout << "};\n";
}

void run_game(std::span<const int> span)
{
    print("span: ", span);

    std::span<const int, 5> span_first = span.first<5>();
    print("span.first<5>(): ", span_first);

    std::span<const int, std::dynamic_extent> span_first_dynamic = span.first(4);
    print("span.first(4): ", span_first_dynamic);
}

int main()
{
    int a[8]{1, 2, 3, 4, 5, 6, 7, 8};
    print("int a", a);
    run_game(a);
}
```

Output:

```text
int a[8]{1, 2, 3, 4, 5, 6, 7, 8};
span: [8]{1, 2, 3, 4, 5, 6, 7, 8};
span.first<5>(): [5]{1, 2, 3, 4, 5};
span.first(4): [4]{1, 2, 3, 4};
```

### See also

- **last** — obtains a subspan consisting of the last `N` elements of the
  sequence (public member function)
- **subspan** — obtains a subspan (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/span/first*
