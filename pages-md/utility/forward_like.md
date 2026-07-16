# std::forward_like

```cpp
template< class T, class U >
[[nodiscard]] constexpr auto&& forward_like( U&& x ) noexcept;  // (since C++23)
```

Returns a reference to `x` which has similar properties to `T&&`.

The return type is determined as below:

1. If `std::remove_reference_t<T>` is a const-qualified type, then the
   referenced type of the return type is `const std::remove_reference_t<U>`.
   Otherwise, the referenced type is `std::remove_reference_t<U>`.
1. If `T&&` is an lvalue reference type, then the return type is also an lvalue
   reference type. Otherwise, the return type is an rvalue reference type.

The program is ill-formed if `T&&` is not a valid type.

### Parameters

- **x** — a value needs to be forwarded like type `T`

### Return value

A reference to `x` of the type determined as above.

### Notes

Like `std::forward`, `std::move`, and `std::as_const`, `std::forward_like` is a
type cast that only influences the value category of an expression, or
potentially adds const-qualification.

When `m` is an actual member and thus `o.m` a valid expression, this is usually
spelled as `std::forward<decltype(o)>(o).m` in C++20 code.

When `o.m` is not a valid expression, i.e. members of lambda closures, one needs
`std::forward_like</*see below*/>(m)`.

This leads to three possible models, called *merge*, *tuple*, and *language*.

- *merge*: merge the `const` qualifiers, and adopt the value category of the
  `Owner`.
- *tuple*: what `std::get<0>(Owner)` does, assuming `Owner` is a
  `std::tuple<Member>`.
- *language*: what `std::forward<decltype(Owner)>(o).m` does.

The main scenario that `std::forward_like` caters to is adapting “far” objects.
Neither the *tuple* nor the *language* scenarios do the right thing for that
main use-case, so the *merge* model is used for `std::forward_like`.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_forward_like` | 202207L | (C++23) | `std::forward_like`

### Possible implementation

```cpp
template<class T, class U>
[[nodiscard]] constexpr auto&& forward_like(U&& x) noexcept
{
    constexpr bool is_adding_const = std::is_const_v<std::remove_reference_t<T>>;
    if constexpr (std::is_lvalue_reference_v<T&&>)
    {
        if constexpr (is_adding_const)
            return std::as_const(x);
        else
            return static_cast<U&>(x);
    }
    else
    {
        if constexpr (is_adding_const)
            return std::move(std::as_const(x));
        else
            return std::move(x);
    }
}
```

### Example

```cpp
#include <cstddef>
#include <iostream>
#include <memory>
#include <optional>
#include <type_traits>
#include <utility>
#include <vector>

struct TypeTeller
{
    void operator()(this auto&& self)
    {
        using SelfType = decltype(self);
        using UnrefSelfType = std::remove_reference_t<SelfType>;
        if constexpr (std::is_lvalue_reference_v<SelfType>)
        {
            if constexpr (std::is_const_v<UnrefSelfType>)
                std::cout << "const lvalue\n";
            else
                std::cout << "mutable lvalue\n";
        }
        else
        {
            if constexpr (std::is_const_v<UnrefSelfType>)
                std::cout << "const rvalue\n";
            else
                std::cout << "mutable rvalue\n";
        }
    }
};

struct FarStates
{
    std::unique_ptr<TypeTeller> ptr;
    std::optional<TypeTeller> opt;
    std::vector<TypeTeller> container;

    auto&& from_opt(this auto&& self)
    {
        return std::forward_like<decltype(self)>(self.opt.value());
        // It is OK to use std::forward<decltype(self)>(self).opt.value(),
        // because std::optional provides suitable accessors.
    }

    auto&& operator[](this auto&& self, std::size_t i)
    {
        return std::forward_like<decltype(self)>(container.at(i));
        // It is not so good to use std::forward<decltype(self)>(self)[i], because
        // containers do not provide rvalue subscript access, although they could.
    }

    auto&& from_ptr(this auto&& self)
    {
        if (!self.ptr)
            throw std::bad_optional_access{};
        return std::forward_like<decltype(self)>(*self.ptr);
        // It is not good to use *std::forward<decltype(self)>(self).ptr, because
        // std::unique_ptr<TypeTeller> always dereferences to a non-const lvalue.
    }
};

int main()
{
    FarStates my_state{
        .ptr{std::make_unique<TypeTeller>()},
        .opt{std::in_place, TypeTeller{} },
        .container{std::vector<TypeTeller>(1)},
    };

    my_state.from_ptr();
    my_state.from_opt();
    my_state[0]();

    std::cout << '\n';

    std::as_const(my_state).from_ptr();
    std::as_const(my_state).from_opt();
    std::as_const(my_state)[0]();

    std::cout << '\n';

    std::move(my_state).from_ptr();
    std::move(my_state).from_opt();
    std::move(my_state)[0]();

    std::cout << '\n';

    std::move(std::as_const(my_state)).from_ptr();
    std::move(std::as_const(my_state)).from_opt();
    std::move(std::as_const(my_state))[0]();

    std::cout << '\n';
}
```

Output:

```text
mutable lvalue
mutable lvalue
mutable lvalue

const lvalue
const lvalue
const lvalue

mutable rvalue
mutable rvalue
mutable rvalue

const rvalue
const rvalue
const rvalue
```

### See also

- **move (C++11)** — obtains an rvalue reference (function template)
- **forward (C++11)** — forwards a function argument (function template)
- **as_const (C++17)** — obtains a reference to const to its argument (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/utility/forward_like*
