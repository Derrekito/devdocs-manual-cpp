# std::codecvt<InternT,ExternT,StateT>::unshift, do_unshift

```cpp
public:
result unshift( StateT& state, ExternT* to, ExternT* to_end,
                ExternT*& to_next ) const;  // (1)
protected:
virtual result do_unshift( StateT& state, ExternT* to, ExternT* to_end,
                           ExternT*& to_next ) const;  // (2)
```

1) Public member function, calls the member function `do_unshift` of the most
   derived class.

2) If the encoding represented by this `codecvt` facet is state-dependent, and
   `state` represents a conversion state that is not the initial shift state,
   writes the characters necessary to return to the initial shift state. The
   characters are written to a character array whose first element is pointed to
   by `to`. No more than `to_end - to` characters are written. The parameter
   `to_next` is updated to point one past the last character written.

### Return value

A value of type `std::codecvt_base::result`, indicating the success status as
follows:

- **`ok`** — all necessary characters were written. `state` now represents
  initial shift state
- **`partial`** — not enough space in the output buffer. `to_next == to_end`
- **`error`** — an unspecified error has occurred
- **`noconv`** — the encoding is not state-dependent, no termination sequence
  necessary

### Notes

This function is called by `std::basic_filebuf::close()` and in other situations
when finalizing a state-dependent multibyte character sequence.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 305 | C++98 | std::codecvt<wchar_t, char, std::mbstate_t>::do_unshift was
      required not to write any character | not required
  LWG 380 | C++98 | the meaning of returning `partial` was 'more characters need
      to be supplied to complete termination', but no character is supplied |
      corrected to indicating insufficient buffer space
  LWG 381 | C++98 | `state` was not required to be valid, and `error` is
      returned if `state` is invalid | `state` is required to be valid, and
      returning `error` indicates an error
  LWG 664 | C++98 | std::codecvt<char, char, std::mbstate_t>::do_unshift was
      required not to write any character | not required
  LWG 665 | C++98 | std::codecvt<char, char, std::mbstate_t>::do_unshift was
      required to return `noconv` | not required

### See also

- **wcrtomb** — converts a wide character to its multibyte representation, given
  state (function)
- **do_out [virtual]** — converts a string from `InternT` to `ExternT`, such as
  when writing to file (virtual protected member function)

---
*Source: https://en.cppreference.com/w/cpp/locale/codecvt/unshift*
