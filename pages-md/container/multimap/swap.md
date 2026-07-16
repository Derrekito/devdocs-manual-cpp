# std::multimap<Key,T,Compare,Allocator>::swap

```cpp
void swap( multimap& other );  // (until C++17)
void swap( multimap& other ) noexcept(/* see below */);  // (since C++17)
```

Exchanges the contents of the container with those of `other`. Does not invoke
any move, copy, or swap operations on individual elements.

All iterators and references remain valid. The `end()` iterator is invalidated.

The `Compare` objects must be Swappable, and they are exchanged using
unqualified call to non-member `swap`.

If `std::allocator_traits<allocator_type>::propagate_on_container_swap::value`
is `true`, then the allocators are exchanged using an unqualified call to
non-member `swap`. Otherwise, they are not swapped (and if `get_allocator() !=
other.get_allocator()`, the behavior is undefined).
*(since C++11)*

### Parameters

- **other** — container to exchange the contents with

### Return value

(none)

### Exceptions

Any exception thrown by the swap of the `Compare` objects.
*(until C++17)*

`noexcept` specification:
`noexcept(std::allocator_traits<Allocator>::is_always_equal::value &&
std::is_nothrow_swappable<Compare>::value)`
*(since C++17)*

### Complexity

Constant.

### Example

```cpp
#include <iostream>
#include <string>
#include <utility>
#include <map>

// print out a std::pair
template<class Os, class U, class V>
Os& operator<<(Os& os, const std::pair<U, V>& p)
{
    return os << p.first << ':' << p.second;
}

// print out a container
template<class Os, class Co>
Os& operator<<(Os& os, const Co& co)
{
    os << '{';
    for (auto const& i : co)
        os << ' ' << i;
    return os << " }\n";
}

int main()
{
    std::multimap<std::string, std::string>
        m1{{"γ", "gamma"}, {"β", "beta"}, {"α", "alpha"}, {"γ", "gamma"}},
        m2{{"ε", "epsilon"}, {"δ", "delta"}, {"ε", "epsilon"}};

    const auto& ref = *(m1.begin());
    const auto iter = std::next(m1.cbegin());

    std::cout << "──────── before swap ────────\n"
              << "m1: " << m1 << "m2: " << m2 << "ref: " << ref
              << "\niter: " << *iter << '\n';

    m1.swap(m2);

    std::cout << "──────── after swap ────────\n"
              << "m1: " << m1 << "m2: " << m2 << "ref: " << ref
              << "\niter: " << *iter << '\n';

    // Note that every iterator referring to an element in one container before
    // the swap refers to the same element in the other container after the swap.
    // Same is true for references.
}
```

Output:

```text
──────── before swap ────────
m1: { α:alpha β:beta γ:gamma γ:gamma }
m2: { δ:delta ε:epsilon ε:epsilon }
ref: α:alpha
iter: β:beta
──────── after swap ────────
m1: { δ:delta ε:epsilon ε:epsilon }
m2: { α:alpha β:beta γ:gamma γ:gamma }
ref: α:alpha
iter: β:beta
```

### See also

- **std::swap(std::multimap)** — specializes the `std::swap` algorithm (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/container/multimap/swap*
