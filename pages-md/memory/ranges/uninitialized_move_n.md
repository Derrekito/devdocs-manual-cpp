# std::ranges::uninitialized_move_n, std::ranges::uninitialized_move_n_result

```cpp
Call signature
template< std::input_iterator I, no-throw-forward-iterator O,
          no-throw-sentinel-for<O> S >
requires std::constructible_from<std::iter_value_t<O>,
         std::iter_rvalue_reference_t<I>>
uninitialized_move_n_result<I, O>
uninitialized_move_n( I ifirst, std::iter_difference_t<I> n, O ofirst, S olast );  // (1) (since C++20)
Helper types
template< class I, class O >
using uninitialized_move_n_result = ranges::in_out_result<I, O>;  // (2) (since C++20)
```

Moves `N` elements from the input range beginning at `ifirst` to the
uninitialized storage designated by the range `[``ofirst``,``olast``)`, where
`N` is `min(n, ranges::distance(ofirst, olast))`.

The effect is equivalent to:

```cpp
for (; n-- > 0 && ofirst != olast; ++ifirst, ++ofirst)
    ::new (static_cast<void*>(std::addressof(*first)))
        std::remove_reference_t<std::iter_reference_t<O>>(ranges::iter_move(ifirst));
```

If an exception is thrown during the initialization then the objects that
already constructed in `[``ofirst``,``olast``)` are destroyed in an unspecified
order. Also, the objects in the input range beginning at `ifirst`, that were
already moved, are left in a valid but unspecified state.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **ifirst** — the beginning of the input range of elements to move from
- **ofirst, olast** — iterator-sentinel pair denoting the output range of
  elements to initialize
- **n** — the number of elements to move

### Return value

`{ifirst + N, ofirst + N}`.

### Complexity

Linear in `N`.

### Exceptions

The exception thrown on construction of the elements in the destination range,
if any.

### Notes

An implementation may improve the efficiency of the
`ranges::uninitialized_move_n`, e.g. by using `ranges::copy_n`, if the value
type of the output range is TrivialType.

### Possible implementation

```cpp
struct uninitialized_move_n_fn
{
    template<std::input_iterator I, no-throw-forward-iterator O,
             no-throw-sentinel-for<O> S>
    requires std::constructible_from<std::iter_value_t<O>,
             std::iter_rvalue_reference_t<I>>
    ranges::uninitialized_move_n_result<I, O>
    operator()(I ifirst, std::iter_difference_t<I> n, O ofirst, S olast) const
    {
        O current{ofirst};
        try
        {
            for (; n-- > 0 && current != olast; ++ifirst, ++current)
                ::new (const_cast<void*>(static_cast<const volatile void*>
                    (std::addressof(*current)))) std::remove_reference_t<
                        std::iter_reference_t<O>>(ranges::iter_move(ifirst));
            return {std::move(ifirst), std::move(current)};
        }
        catch (...) // rollback: destroy constructed elements
        {
            for (; ofirst != current; ++ofirst)
                ranges::destroy_at(std::addressof(*ofirst));
            throw;
        }
    }
};

inline constexpr uninitialized_move_n_fn uninitialized_move_n{};
```

### Example

```cpp
#include <cstdlib>
#include <iomanip>
#include <iostream>
#include <memory>
#include <string>

void print(auto rem, auto first, auto last)
{
    for (std::cout << rem; first != last; ++first)
        std::cout << std::quoted(*first) << ' ';
    std::cout << '\n';
}

int main()
{
    std::string in[]{ "No", "Diagnostic", "Required", };
    print("initially, in: ", std::begin(in), std::end(in));

    if (
        constexpr auto sz = std::size(in);
        void* out = std::aligned_alloc(alignof(std::string), sizeof(std::string) * sz)
    )
    {
        try
        {
            auto first{static_cast<std::string*>(out)};
            auto last{first + sz};
            std::ranges::uninitialized_move_n(std::begin(in), sz, first, last);

            print("after move, in: ", std::begin(in), std::end(in));
            print("after move, out: ", first, last);

            std::ranges::destroy(first, last);
        }
        catch (...)
        {
            std::cout << "Exception!\n";
        }
        std::free(out);
    }
}
```

Possible output:

```text
initially, in: "No" "Diagnostic" "Required"
after move, in: "" "" ""
after move, out: "No" "Diagnostic" "Required"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3870 | C++20 | this algorithm might create objects on a const storage |
      kept disallowed

### See also

- **ranges::uninitialized_move (C++20)** — moves a range of objects to an
  uninitialized area of memory (niebloid)
- **uninitialized_move_n (C++17)** — moves a number of objects to an
  uninitialized area of memory (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/ranges/uninitialized_move_n*
