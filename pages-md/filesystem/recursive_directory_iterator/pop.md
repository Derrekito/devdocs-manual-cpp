# std::filesystem::recursive_directory_iterator::pop

```cpp
void pop();  // (1) (since C++17)
void pop( std::error_code& ec );  // (2) (since C++17)
```

Moves the iterator one level up in the directory hierarchy. Invalidates all
copies of the previous value of `*this`.

If the parent directory is outside directory hierarchy that is iterated on (i.e.
`depth() == 0`), sets `*this` to an end directory iterator.

### Parameters

- **ec** — error code to store the error status to

### Return value

(none)

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

1) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with the OS error code as the error code argument.

2) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Example

---
*Source: https://en.cppreference.com/w/cpp/filesystem/recursive_directory_iterator/pop*
