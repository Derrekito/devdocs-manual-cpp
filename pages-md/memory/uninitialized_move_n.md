# std::uninitialized_move_n

```cpp
template< class InputIt, class Size, class NoThrowForwardIt >
std::pair<InputIt, NoThrowForwardIt>
    uninitialized_move_n( InputIt first, Size count,
                          NoThrowForwardIt d_first );  // (1) (since C++17)
template< class ExecutionPolicy, class ForwardIt,
          class Size, class NoThrowForwardIt >
std::pair<ForwardIt, NoThrowForwardIt>
    uninitialized_move_n( ExecutionPolicy&& policy, ForwardIt first,
                          Size count, NoThrowForwardIt d_first );  // (2) (since C++17)
```

1) Moves `count` elements from a range beginning at `first` to an uninitialized
   memory area beginning at `d_first` as if by `for (; n > 0; ++d_first, (void)
   ++first, --n) ::new (static_cast<void*>(std::addressof(*d_first))) typename
   std::iterator_traits<NoThrowForwardIt>::value_type(std::move(*first));`

If an exception is thrown during the initialization, some objects in
   `first``+``[``​0​``,``n``)` are left in a valid but unspecified state, and
   the objects already constructed are destroyed in an unspecified order. If
   `d_first``+``[``​0​``,``n``)` overlaps with `first``+``[``​0​``,``n``)`, the
   behavior is undefined. (since C++20)

2) Same as (1), but executed according to `policy`. This overload does not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first** — the beginning of the range of the elements to move
- **d_first** — the beginning of the destination range
- **count** — the number of elements to move
- **policy** — the execution policy to use. See execution policy for details.

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-`ForwardIt` must meet the requirements of LegacyForwardIterator.**

**-`NoThrowForwardIt` must meet the requirements of LegacyForwardIterator.**

**-No increment, assignment, comparison, or indirection through valid instances of `NoThrowForwardIt` may throw exceptions.**

### Return value

A pair whose first element is an iterator to the element past the last element
moved in the source range, and whose second element is an iterator to the
element past the last element moved in the destination range.

### Complexity

Linear in `count`.

### Exceptions

The overload with a template parameter named `ExecutionPolicy` reports errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Possible implementation

```cpp
template<class InputIt, class Size, class NoThrowForwardIt>
std::pair<InputIt, NoThrowForwardIt>
    uninitialized_move_n(InputIt first, Size count, NoThrowForwardIt d_first)
{
    using Value = typename std::iterator_traits<NoThrowForwardIt>::value_type;
    NoThrowForwardIt current = d_first;
    try
    {
        for (; count > 0; ++first, (void) ++current, --count)
            ::new (static_cast<void*>(std::addressof(*current))) Value(std::move(*first));
    }
    catch (...)
    {
        std::destroy(d_first, current);
        throw;
    }
    return {first, current};
}
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
    std::string in[]{"One", "Definition", "Rule"};
    print("initially, in: ", std::begin(in), std::end(in));

    if (
        constexpr auto sz = std::size(in);
        void* out = std::aligned_alloc(alignof(std::string), sizeof(std::string) * sz))
    {
        try
        {
            auto first{static_cast<std::string*>(out)};
            auto last{first + sz};
            std::uninitialized_move_n(std::begin(in), sz, first);

            print("after move, in: ", std::begin(in), std::end(in));
            print("after move, out: ", first, last);

            std::destroy(first, last);
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
initially, in: "One" "Definition" "Rule"
after move, in: "" "" ""
after move, out: "One" "Definition" "Rule"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3870 | C++20 | this algorithm might create objects on a const storage |
      kept disallowed

### See also

- **uninitialized_move (C++17)** — moves a range of objects to an uninitialized
  area of memory (function template)
- **uninitialized_copy_n (C++11)** — copies a number of objects to an
  uninitialized area of memory (function template)
- **ranges::uninitialized_move_n (C++20)** — moves a number of objects to an
  uninitialized area of memory (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/memory/uninitialized_move_n*
