# std::ios_base::unsetf

```cpp
void unsetf( fmtflags flags );
```

Unsets the formatting flags identified by `flags`.

### Parameters

- **flags** — formatting flags to unset. It can be a combination of the
  formatting flags constants.

##### Formatting flags

- **`dec`** — use decimal base for integer I/O: see `std::dec`
- **`oct`** — use octal base for integer I/O: see `std::oct`
- **`hex`** — use hexadecimal base for integer I/O: see `std::hex`
- **`basefield`** — `dec | oct | hex`. Useful for masking operations
- **`left`** — left adjustment (adds fill characters to the right): see
  `std::left`
- **`right`** — right adjustment (adds fill characters to the left): see
  `std::right`
- **`internal`** — internal adjustment (adds fill characters to the internal
  designated point): see `std::internal`
- **`adjustfield`** — `left | right | internal`. Useful for masking operations
- **`scientific`** — generate floating point types using scientific notation, or
  hex notation if combined with `fixed`: see `std::scientific`
- **`fixed`** — generate floating point types using fixed notation, or hex
  notation if combined with `scientific`: see `std::fixed`
- **`floatfield`** — `scientific | fixed`. Useful for masking operations
- **`boolalpha`** — insert and extract bool type in alphanumeric format: see
  `std::boolalpha`
- **`showbase`** — generate a prefix indicating the numeric base for integer
  output, require the currency indicator in monetary I/O: see `std::showbase`
- **`showpoint`** — generate a decimal-point character unconditionally for
  floating-point number output: see `std::showpoint`
- **`showpos`** — generate a + character for non-negative numeric output: see
  `std::showpos`
- **`skipws`** — skip leading whitespace before certain input operations: see
  `std::skipws`
- **`unitbuf`** — flush the output after each output operation: see
  `std::unitbuf`
- **`uppercase`** — replace certain lowercase letters with their uppercase
  equivalents in certain output operations: see `std::uppercase`

### Return value

(none)

### Example

### See also

- **flags** — manages format flags (public member function)
- **setf** — sets specific format flag (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/ios_base/unsetf*
