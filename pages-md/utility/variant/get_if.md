# std::get_if (std::variant)

```cpp
template< std::size_t I, class... Types >
constexpr std::add_pointer_t<std::variant_alternative_t<I, std::variant<Types...>>>
    get_if( std::variant<Types...>* pv ) noexcept;
template< std::size_t I, class... Types >
constexpr std::add_pointer_t<const std::variant_alternative_t<I, std::variant<Types...>>>
    get_if( const std::variant<Types...>* pv ) noexcept;
template< class T, class... Types >
constexpr std::add_pointer_t<T>
    get_if( std::variant<Types...>* pv ) noexcept;
template< class T, class... Types >
constexpr std::add_pointer_t<const T>
    get_if( const std::variant<Types...>* pv ) noexcept;
```

1) Index-based non-throwing accessor: If `pv` is not a null pointer and
   `pv->index() == I`, returns a pointer to the value stored in the variant
   pointed to by `pv`. Otherwise, returns a null pointer value. The call is
   ill-formed if `I` is not a valid index in the variant.

2) Type-based non-throwing accessor: Equivalent to (1) with `I` being the
   zero-based index of `T` in `Types...`. The call is ill-formed if `T` is not a
   unique element of `Types...`.

### Parameters

- **I** — index to look up
- **Type** — unique type to look up
- **pv** — pointer to a variant

### Return value

Pointer to the value stored in the pointed-to variant or null pointer on error.

### Example

```cpp
#include <iostream>
#include <variant>

int main()
{
    auto check_value = [](const std::variant<int, float>& v)
    {
        if (const int* pval = std::get_if<int>(&v))
            std::cout << "variant value: " << *pval << '\n';
        else
            std::cout << "failed to get value!" << '\n';
    };

    std::variant<int, float> v{12}, w{3.f};
    check_value(v);
    check_value(w);
}
```

Output:

```text
variant value: 12
failed to get value!
```

### See also

- **get(std::variant) (C++17)** — reads the value of the variant given the index
  or the type (if the type is unique), throws on error (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/variant/get_if*
