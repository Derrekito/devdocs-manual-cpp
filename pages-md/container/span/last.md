# std::span<T,Extent>::last

```cpp
template< std::size_t Count >
constexpr std::span<element_type, Count> last() const;  // (1) (since C++20)
constexpr std::span<element_type, std::dynamic_extent> last( size_type Count ) const;  // (2) (since C++20)
```

Obtains a span that is a view over the last `Count` elements of this span. The
program is ill-formed if `Count > Extent`. The behavior is undefined if `Count >
size()`.

### Return value

A span `r` that is a view over the last `Count` elements of `*this`, such that
`r.data() == this->data() + (this->size() - Count) && r.size() == Count`.

### Example

```cpp
#include <iostream>
#include <span>
#include <string_view>

void println(std::string_view const title, auto const& container)
{
    std::cout << title << '[' << std::size(container) << "]{ ";
    for (auto const& elem : container)
        std::cout << elem << ", ";
    std::cout << "};\n";
};

void run(std::span<const int> span)
{
    println("span: ", span);

    std::span<const int, 3> span_last = span.last<3>();
    println("span.last<3>(): ", span_last);

    std::span<const int, std::dynamic_extent> span_last_dynamic = span.last(2);
    println("span.last(2): ", span_last_dynamic);
}

int main()
{
    int a[8]{1, 2, 3, 4, 5, 6, 7, 8};
    println("int a", a);
    run(a);
}
```

Output:

```text
int a[8]{ 1, 2, 3, 4, 5, 6, 7, 8, };
span: [8]{ 1, 2, 3, 4, 5, 6, 7, 8, };
span.last<3>(): [3]{ 6, 7, 8, };
span.last(2): [2]{ 7, 8, };
```

### See also

- **first** — obtains a subspan consisting of the first `N` elements of the
  sequence (public member function)
- **subspan** — obtains a subspan (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/span/last*
