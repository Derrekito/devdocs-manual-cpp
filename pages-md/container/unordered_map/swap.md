# std::unordered_map<Key,T,Hash,KeyEqual,Allocator>::swap

```cpp
void swap( unordered_map& other );  // (since C++11) (until C++17)
void swap( unordered_map& other ) noexcept(/* see below */);  // (since C++17)
```

Exchanges the contents of the container with those of `other`. Does not invoke
any move, copy, or swap operations on individual elements.

All iterators and references remain valid. The `end()` iterator is invalidated.

The `Hash` and `KeyEqual` objects must be Swappable, and they are exchanged
using unqualified calls to non-member `swap`.

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

Any exception thrown by the swap of the `Hash` or `KeyEqual` objects.
*(until C++17)*

`noexcept` specification:
`noexcept(std::allocator_traits<Allocator>::is_always_equal::value &&
std::is_nothrow_swappable<Hash>::value &&
std::is_nothrow_swappable<key_equal>::value)`
*(since C++17)*

### Complexity

Constant.

### Example

```cpp
#include <iostream>
#include <string>
#include <utility>
#include <unordered_map>

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
    std::unordered_map<std::string, std::string>
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

Possible output:

```text
──────── before swap ────────
m1: { α:alpha β:beta γ:gamma }
m2: { δ:delta ε:epsilon }
ref: α:alpha
iter: β:beta
──────── after swap ────────
m1: { δ:delta ε:epsilon }
m2: { α:alpha β:beta γ:gamma }
ref: α:alpha
iter: β:beta
```

### See also

- **std::swap(std::unordered_map) (C++11)** — specializes the `std::swap`
  algorithm (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_map/swap*
