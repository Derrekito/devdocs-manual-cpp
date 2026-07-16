# std::default_accessor

```cpp
template< class ElementType >
class default_accessor;  // (since C++23)
```

The `std::default_accessor` class template is the default AccessorPolicy used by
`std::mdspan` if no user-specified accessor policy is provided.

Each specialization of `default_accessor` models `semiregular` and is
TriviallyCopyable.

### Template parameters

- **ElementType** — the element type. Shall be a complete object type that is
  neither an abstract class type nor an array type. Otherwise, the program is
  ill-formed

### Member types

- **`offset_policy`** — `default_accessor`
- **`element_type`** — `ElementType`
- **`reference`** — `ElementType&`
- **`data_handle_type`** — `ElementType*`

### Member functions

- **(constructor)** — constructs a `default_accessor` (public member function)
- **access** — returns a nth element provided by arguments (public member
  function)
- **offset** — returns a data handle that has an offset provided by arguments
  (public member function)

## std::default_accessor::default_accessor

```cpp
constexpr default_accessor() noexcept = default;  // (1)
template< class OtherElementType >
constexpr default_accessor( default_accessor<OtherElementType> ) noexcept {}  // (2)
```

1) Default constructs a `default_accessor`.

2) Constructs a `default_accessor` from `default_accessor<OtherElementType>`.
   The constructor has no visible effect. This overload participates in overload
   resolution only if `std::is_convertible_v<OtherElementType(*)[],
   element_type(*)[]>` is true.

## std::default_accessor::access

```cpp
constexpr reference access( data_handle_type p, std::size_t i ) const noexcept;
```

Equivalent to `return p[i];`.

## std::default_accessor::offset

```cpp
constexpr data_handle_type offset( data_handle_type p, std::size_t i ) const noexcept;
```

Equivalent to `return p + i;`.

### Example

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/default_accessor*
