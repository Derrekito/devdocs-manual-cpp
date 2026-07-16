# std::ranges::uninitialized_default_construct

```cpp
Call signature
template< no-throw-forward-iterator I, no-throw-sentinel-for<I> S >
requires std::default_initializable<std::iter_value_t<I>>
I uninitialized_default_construct( I first, S last );  // (1) (since C++20)
template< no-throw-forward-range R >
requires std::default_initializable<ranges::range_value_t<R>>
ranges::borrowed_iterator_t<R>
uninitialized_default_construct( R&& r );  // (2) (since C++20)
```

1) Constructs objects of type `std::iter_value_t<I>` in the uninitialized
   storage designated by the range `[``first``,``last``)` by
   default-initialization, as if by for (; first != last; ++first) ::new
   (static_cast<void*>(std::addressof(*first)))
   std::remove_reference_t<std::iter_reference_t<I>>;

If an exception is thrown during the initialization, the objects already
   constructed are destroyed in an unspecified order.

2) Same as (1), but uses `r` as the range, as if using `ranges::begin(r)` as
   `first`, and `ranges::end(r)` as `last`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — iterator-sentinel pair denoting the range of the elements to
  initialize
- **r** — the range of the elements to initialize

### Return value

An iterator equal to `last`.

### Complexity

Linear in the distance between `first` and `last`.

### Exceptions

The exception thrown on construction of the elements in the destination range,
if any.

### Notes

An implementation may skip the objects construction (without changing the
observable effect) if no non-trivial default constructor is called while
default-initializing a `std::iter_value_t<I>` object, which can be detected by
`std::is_trivially_default_constructible_v`.

### Possible implementation

```cpp
struct uninitialized_default_construct_fn
{
    template<no-throw-forward-iterator I, no-throw-sentinel-for<I> S>
    requires std::default_initializable<std::iter_value_t<I>>
    I operator()(I first, S last) const
    {
        using ValueType = std::remove_reference_t<std::iter_reference_t<I>>;
        if constexpr (std::is_trivially_default_constructible_v<ValueType>)
            return ranges::next(first, last); // skip initialization
        I rollback{first};
        try
        {
            for (; !(first == last); ++first)
                ::new (const_cast<void*>(static_cast<const volatile void*>
                    (std::addressof(*first)))) ValueType;
            return first;
        }
        catch (...) // rollback: destroy constructed elements
        {
            for (; rollback != first; ++rollback)
                ranges::destroy_at(std::addressof(*rollback));
            throw;
        }
    }

    template<no-throw-forward-range R>
    requires std::default_initializable<ranges::range_value_t<R>>
    ranges::borrowed_iterator_t<R>
    operator()(R&& r) const
    {
        return (*this)(ranges::begin(r), ranges::end(r));
    }
};

inline constexpr uninitialized_default_construct_fn uninitialized_default_construct{};
```

### Example

```cpp
#include <cstring>
#include <iostream>
#include <memory>
#include <string>

int main()
{
    struct S { std::string m{"▄▀▄▀▄▀▄▀"}; };

    constexpr int n{4};
    alignas(alignof(S)) char out[n * sizeof(S)];

    try
    {
        auto first{reinterpret_cast<S*>(out)};
        auto last{first + n};

        std::ranges::uninitialized_default_construct(first, last);

        auto count{1};
        for (auto it{first}; it != last; ++it)
            std::cout << count++ << ' ' << it->m << '\n';

        std::ranges::destroy(first, last);
    }
    catch (...) { std::cout << "Exception!\n"; }

    // Notice that for "trivial types" the uninitialized_default_construct
    // generally does not zero-fill the given uninitialized memory area.
    constexpr char etalon[]{'A', 'B', 'C', 'D', '\n'};
    char v[]{'A', 'B', 'C', 'D', '\n'};
    std::ranges::uninitialized_default_construct(std::begin(v), std::end(v));
    if (std::memcmp(v, etalon, sizeof(v)) == 0)
    {
        std::cout << "  ";
        // Maybe undefined behavior, pending CWG 1997:
        // for (const char c : v) { std::cout << c << ' '; }
        for (const char c : etalon)
            std::cout << c << ' ';
    }
    else
        std::cout << "Unspecified\n";
}
```

Possible output:

```text
1 ▄▀▄▀▄▀▄▀
2 ▄▀▄▀▄▀▄▀
3 ▄▀▄▀▄▀▄▀
4 ▄▀▄▀▄▀▄▀
  A B C D
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3870 | C++20 | this algorithm might create objects on a const storage |
      kept disallowed

### See also

- **ranges::uninitialized_default_construct_n (C++20)** — constructs objects by
  default-initialization in an uninitialized area of memory, defined by a start
  and count (niebloid)
- **ranges::uninitialized_value_construct (C++20)** — constructs objects by
  value-initialization in an uninitialized area of memory, defined by a range
  (niebloid)
- **ranges::uninitialized_value_construct_n (C++20)** — constructs objects by
  value-initialization in an uninitialized area of memory, defined by a start
  and a count (niebloid)
- **uninitialized_default_construct (C++17)** — constructs objects by
  default-initialization in an uninitialized area of memory, defined by a range
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/ranges/uninitialized_default_construct*
