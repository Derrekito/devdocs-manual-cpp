# std::multiset<Key,Compare,Allocator>::swap

```cpp
void swap( multiset& other );  // (until C++17)
void swap( multiset& other ) noexcept(/* see below */);  // (since C++17)
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
#include <functional>
#include <iostream>
#include <set>

template<class Os, class Co>
Os& operator<<(Os& os, const Co& co)
{
    os << '{';
    for (auto const& i : co)
        os << ' ' << i;
    return os << " } ";
}

int main()
{
    std::multiset<int> a1{3, 1, 3, 2}, a2{5, 4, 5};

    auto it1 = std::next(a1.begin());
    auto it2 = std::next(a2.begin());

    const int& ref1 = *(a1.begin());
    const int& ref2 = *(a2.begin());

    std::cout << a1 << a2 << *it1 << ' ' << *it2 << ' ' << ref1 << ' ' << ref2 << '\n';
    a1.swap(a2);
    std::cout << a1 << a2 << *it1 << ' ' << *it2 << ' ' << ref1 << ' ' << ref2 << '\n';

    // Note that every iterator referring to an element in one container before the swap
    // refers to the same element in the other container after the swap. Same is true
    // for references.

    struct Cmp : std::less<int>
    {
        int id{};
        Cmp(int i) : id{i} {}
    };

    std::multiset<int, Cmp> s1{{2, 2, 1, 1}, Cmp{6}}, s2{{4, 4, 3, 3}, Cmp{9}};

    std::cout << s1 << s2 << s1.key_comp().id << ' ' << s2.key_comp().id << '\n';
    s1.swap(s2);
    std::cout << s1 << s2 << s1.key_comp().id << ' ' << s2.key_comp().id << '\n';

    // So, comparator objects (Cmp) are also exchanged after the swap.
}
```

Output:

```text
{ 1 2 3 3 } { 4 5 5 } 2 5 1 4
{ 4 5 5 } { 1 2 3 3 } 2 5 1 4
{ 1 1 2 2 } { 3 3 4 4 } 6 9
{ 3 3 4 4 } { 1 1 2 2 } 9 6
```

### See also

- **std::swap(std::multiset)** — specializes the `std::swap` algorithm (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/container/multiset/swap*
