# std::nontype, std::nontype_t

```cpp
template< auto V >
struct nontype_t { explicit nontype_t() = default; };  // (1) (since C++26)
template< auto V >
inline constexpr std::nontype_t<V> nontype {};  // (2) (since C++26)
```

1) The class template `std::nontype_t` can be used in the constructor's
   parameter list to match the intended tag.

2) The corresponding `std::nontype` instance of (1) is a disambiguation argument
   tag that can be passed to the constructors of `std::function_ref` to indicate
   that the contained object should be constructed with the value of the
   non-type template parameter `V`.

### Template parameters

- **V** — non-type template parameter of a structural type.

### See also

- **function_ref (C++26)** — non-owning reference to any callable object with
  specified function call signature (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/nontype*
