# std::expected<T,E>::swap

```cpp
constexpr void swap( expected& other ) noexcept(/*see below*/);  // (since C++23)
```

Swaps the contents with those of `other`.

- If both `this->has_value()` and `other.has_value()` are true:
- If both `this->has_value()` and `other.has_value()` are false, equivalent to
  `using std::swap; swap(this->error(), other.error());`.
- If `this->has_value()` is false and `other.has_value()` is true, calls
  `other.swap(*this)`.
- If `this->has_value()` is true and `other.has_value()` is false,

```cpp
std::construct_at(std::addressof(unex), std::move(other.unex));
std::destroy_at(std::addressof(other.unex));
```

- Otherwise, let *val* be the member that represents the expected value and
  *unex* be the member that represents the unexpected value, equivalent to:

```cpp
if constexpr (std::is_nothrow_move_constructible_v<E>) {
    E temp(std::move(other.unex));
    std::destroy_at(std::addressof(other.unex));
    try {
        std::construct_at(std::addressof(other.val), std::move(val));
        std::destroy_at(std::addressof(val));
        std::construct_at(std::addressof(unex), std::move(temp));
    } catch(...) {
        std::construct_at(std::addressof(other.unex), std::move(temp));
        throw;
    }
} else {
    T temp(std::move(val));
    std::destroy_at(std::addressof(val));
    try {
        std::construct_at(std::addressof(unex), std::move(other.unex));
        std::destroy_at(std::addressof(other.unex));
        std::construct_at(std::addressof(other.val), std::move(temp));
    } catch(...) {
        std::construct_at(std::addressof(val), std::move(temp));
        throw;
    }
}
```

- In either case, if no exception was thrown, after swap, `this->has_value()` is
  false, and `other.has_value()` is true.

This function participates in overload resolution only if

- either `T` is (possibly cv-qualified) `void`, or `std::is_swappable_v<T>` is
  true, and
- `std::is_swappable_v<E>` is true, and
- either `T` is (possibly cv-qualified) `void`, or
  `std::is_move_constructible_v<T>` is true, and
- `std::is_move_constructible_v<E>` is true, and
- at least one of the following is true: `T` is (possibly cv-qualified) `void`
  `std::is_nothrow_move_constructible_v<T>`
  `std::is_nothrow_move_constructible_v<E>`

### Parameters

- **other** — the `optional` object to exchange the contents with

### Return value

(none)

### Exceptions

If `T` is (possibly cv-qualified) `void`,

`noexcept` specification:

`noexcept( std::is_nothrow_move_constructible_v<E> &&
std::is_nothrow_swappable_v<E> )`

Otherwise,

`noexcept` specification:

`noexcept( std::is_nothrow_move_constructible_v<T> &&
std::is_nothrow_swappable_v<T> && std::is_nothrow_move_constructible_v<E> &&
std::is_nothrow_swappable_v<E> )`

In the case of thrown exception, the states of the contained values of `*this`
and `other` are determined by the exception safety guarantees of `swap` or `T`'s
and `E`'s move constructor, whichever is called. For both `*this` and `other`,
if the object contained an expected value, it is left containing an expected
value, and the other way round.

### Example

### See also

- **swap(std::expected) (C++23)** — specializes the `std::swap` algorithm
  (function)

---
*Source: https://en.cppreference.com/w/cpp/utility/expected/swap*
