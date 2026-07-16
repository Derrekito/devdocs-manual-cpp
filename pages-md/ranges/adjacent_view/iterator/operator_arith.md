# std::ranges::adjacent_view<V,N>::*iterator*<Const>::operator++,--,+=,-=

```cpp
constexpr /*iterator*/& operator++();  // (1) (since C++23)
constexpr /*iterator*/ operator++( int );  // (2) (since C++23)
constexpr /*iterator*/& operator--()
  requires ranges::bidirectional_range<Base>;  // (3) (since C++23)
constexpr /*iterator*/ operator--( int )
  requires ranges::bidirectional_range<Base>;  // (4) (since C++23)
constexpr /*iterator*/& operator+=( difference_type n )
  requires ranges::random_access_range<Base>;  // (5) (since C++23)
constexpr /*iterator*/& operator-=( difference_type n )
  requires ranges::random_access_range<Base>;  // (6) (since C++23)
```

Increments or decrements the iterator.

Let `current_` be an underlying array of iterators.

1) Equivalent to: for (auto& i : current_) std::ranges::next(i); return *this;
   The behavior is undefined if before the call the `current_.back()` is not
   incrementable.

2) Equivalent to: auto tmp = *this; ++*this; return tmp;

3) Equivalent to: for (auto& i : current_) std::ranges::prev(i); return *this;
   The behavior is undefined if before the call the `current_.front()` is not
   decrementable.

4) Equivalent to: auto tmp = *this; --*this; return tmp;

5) Equivalent to: for (auto& i : current_) i += n; return *this; The behavior is
   undefined if before the call the `current_.back() + n` does not have
   well-defined behavior.

6) Equivalent to: for (auto& i : current_) i -= n; return *this; The behavior is
   undefined if before the call the `current_.front() - n` does not have
   well-defined behavior.

### Parameters

- **n** — position relative to current location

### Return value

1,3,5,6) `*this`

2,4) A copy of `*this` that was made before the change.

### Example

```cpp
#include <cassert>
#include <list>
#include <ranges>
#include <utility>
#include <vector>

[[nodiscard]]
bool operator==(std::pair<int&, int&> x, std::pair<int, int> y)
{
    return x.first == y.first and x.second == y.second;
}

int main()
{
    {
        auto v = std::vector{0, 1, 2, 3, 4, 5};
        auto i = (v | std::views::pairwise).begin();
        assert((*i == std::pair{0, 1}));
        ++i;                            // overload (1)
        assert((*i == std::pair{1, 2}));
        --i;                            // overload (3)
        assert((*i == std::pair{0, 1}));
        i += 2;                         // overload (5)
        assert((*i == std::pair{2, 3}));
        i -= 2;                         // overload (6)
        assert((*i == std::pair{0, 1}));
    }
    {
        auto v = std::list{0, 1, 2, 3, 4, 5};
        auto i = (v | std::views::pairwise).begin();
        assert((*i == std::pair{0, 1}));
        ++i;                            // overload (1)
        assert((*i == std::pair{1, 2}));
        --i;                            // overload (3)
        assert((*i == std::pair{0, 1}));
//      i += 2; // Error: v is not a random_access_range; overload (5)
//      i -= 2; // Error: v is not a random_access_range; overload (6)
    }
}
```

### See also

- **operator+operator- (C++23)** — performs iterator arithmetic (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_view/iterator/operator_arith*
