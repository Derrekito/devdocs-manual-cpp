# std::basic_streambuf<CharT,Traits>::gbump

```cpp
protected:
void gbump( int count );
```

Skips `count` characters in the get area. This is done by adding `count` to the
*get pointer*. No checks are done for underflow.

### Parameters

- **count** — number of characters to skip

### Return value

(none)

### Notes

Because this function takes an `int`, it cannot manipulate buffers larger than
`std::numeric_limits<int>::max()` characters (LWG issue 255).

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 59 | C++98 | it was unclear whether `gbump` can be implemented as calling
      `sbumpc` `count` times (which may check underflow) | only add `count` to
      the get pointer

### See also

- **pbump** — advances the next pointer of the output sequence (protected member
  function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/gbump*
