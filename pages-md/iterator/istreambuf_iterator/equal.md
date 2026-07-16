# std::istreambuf_iterator<CharT,Traits>::equal

```cpp
bool equal( const istreambuf_iterator& it ) const;
```

Checks whether both `*this` and `it` are valid, or both are invalid, regardless
of the stream buffer objects they use.

### Parameters

- **it** — another stream buffer iterator to compare to

### Return value

`true` if both `*this` and `it` are valid, or both are invalid, `false`
otherwise.

### Exceptions

May throw implementation-defined exceptions.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 110 | C++98 | the signature was `bool equal(istreambuf_iterator& it)` |
      consts added
  LWG 1126 | C++98 | LWG issue 110 was not properly resolved, leaving the
      parameter type as `istreambuf_iterator&` | const added

---
*Source: https://en.cppreference.com/w/cpp/iterator/istreambuf_iterator/equal*
