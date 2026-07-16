# operator<<(std::sub_match)

```cpp
template< class CharT, class Traits, class BidirIt >
std::basic_ostream<CharT,Traits>&
    operator<<( std::basic_ostream<CharT,Traits>& os,
                const sub_match<BidirIt>& m );  // (since C++11)
```

Writes the representation of the matched subsequence `m` to the output stream
`os`.

Equivalent to `os << m.str()`.

### Parameters

- **os** — output stream to write the representation to
- **m** — a sub-match object to output

### Return value

`os`

### Example

---
*Source: https://en.cppreference.com/w/cpp/regex/sub_match/operator_ltlt*
