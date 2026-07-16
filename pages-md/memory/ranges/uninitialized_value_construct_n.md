# std::ranges::uninitialized_value_construct_n

```cpp
Call signature
template< no-throw-forward-iterator I >
requires std::default_initializable<std::iter_value_t<I>>
I uninitialized_value_construct_n( I first, std::iter_difference_t<I> n );  // (since C++20)
```

Constructs `n` objects of type `std::iter_value_t<I>` in the uninitialized
memory area starting at `first` by value-initialization, as if by
`for (; n-- > 0; ++first) ::new (static_cast<void*>(std::addressof(*first)))
std::remove_reference_t<std::iter_reference_t<I>>();`

If an exception is thrown during the initialization, the objects already
constructed are destroyed in an unspecified order.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first** — the beginning of the range of elements to initialize
- **n** — the number of elements to construct

### Return value

The end of the range of objects (i.e., `ranges::next(first, n)`).

### Complexity

Linear in `n`.

### Exceptions

The exception thrown on construction of the elements in the destination range,
if any.

### Notes

An implementation may improve the efficiency of the
`ranges::uninitialized_value_construct_n`, e.g. by using `ranges::fill_n`, if
the value type of the range is TrivialType *and* CopyAssignable.

### Possible implementation

```cpp
struct uninitialized_value_construct_n_fn
{
    template<no-throw-forward-iterator I>
    requires std::default_initializable<std::iter_value_t<I>>
    I operator()(I first, std::iter_difference_t<I> n) const
    {
        using T = std::remove_reference_t<std::iter_reference_t<I>>;
        if constexpr (std::is_trivial_v<T> && std::is_copy_assignable_v<T>)
            return ranges::fill_n(first, n, T());
        I rollback{first};
        try
        {
            for (; n-- > 0; ++first)
                ::new (const_cast<void*>(static_cast<const volatile void*>
                    (std::addressof(*first)))) T();
            return first;
        }
        catch (...) // rollback: destroy constructed elements
        {
            for (; rollback != first; ++rollback)
                ranges::destroy_at(std::addressof(*rollback));
            throw;
        }
    }
};

inline constexpr uninitialized_value_construct_n_fn uninitialized_value_construct_n{};
```

### Example

```cpp
#include <iostream>
#include <memory>
#include <string>

int main()
{
    struct S { std::string m{"█▓▒░ █▓▒░ █▓▒░ "}; };

    constexpr int n{4};
    alignas(alignof(S)) char out[n * sizeof(S)];

    try
    {
        auto first{reinterpret_cast<S*>(out)};
        auto last = std::ranges::uninitialized_value_construct_n(first, n);

        auto count{1};
        for (auto it{first}; it != last; ++it)
            std::cout << count++ << ' ' << it->m << '\n';

        std::ranges::destroy(first, last);
    }
    catch (...)
    {
        std::cout << "Exception!\n";
    }

    // Notice that for "trivial types" the uninitialized_value_construct_n
    // zero-initializes the given uninitialized memory area.
    int v[]{1, 2, 3, 4, 5, 6, 7, 8};
    std::cout << ' ';
    for (const int i : v)
        std::cout << i << ' ';
    std::cout << "\n ";
    std::ranges::uninitialized_value_construct_n(std::begin(v), std::size(v));
    for (const int i : v)
        std::cout << i << ' ';
    std::cout << '\n';
}
```

Output:

```text
1 █▓▒░ █▓▒░ █▓▒░
2 █▓▒░ █▓▒░ █▓▒░
3 █▓▒░ █▓▒░ █▓▒░
4 █▓▒░ █▓▒░ █▓▒░
1 2 3 4 5 6 7 8
0 0 0 0 0 0 0 0
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3870 | C++20 | this algorithm might create objects on a const storage |
      kept disallowed

### See also

- **ranges::uninitialized_value_construct (C++20)** — constructs objects by
  value-initialization in an uninitialized area of memory, defined by a range
  (niebloid)
- **ranges::uninitialized_default_construct (C++20)** — constructs objects by
  default-initialization in an uninitialized area of memory, defined by a range
  (niebloid)
- **ranges::uninitialized_default_construct_n (C++20)** — constructs objects by
  default-initialization in an uninitialized area of memory, defined by a start
  and count (niebloid)
- **uninitialized_value_construct_n (C++17)** — constructs objects by
  value-initialization in an uninitialized area of memory, defined by a start
  and a count (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/ranges/uninitialized_value_construct_n*
