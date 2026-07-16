# std::future<T>::share

```cpp
std::shared_future<T> share() noexcept;
```

Transfers the shared state of `*this`, if any, to a `std::shared_future` object.
Multiple `std::shared_future` objects may reference the same shared state, which
is not possible with `std::future`.

After calling `share` on a `std::future`, `valid()` `== false`.

### Parameters

(none)

### Return value

A `std::shared_future` object containing the shared state previously held by
`*this`, if any, constructed as if by `std::shared_future<T>(std::move(*this))`.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2556 | C++11 | `share()` requires `valid()` to be `true` | requirement
      removed and made `noexcept`

### See also

- **shared_future (C++11)** — waits for a value (possibly referenced by other
  futures) that is set asynchronously (class template)

---
*Source: https://en.cppreference.com/w/cpp/thread/future/share*
