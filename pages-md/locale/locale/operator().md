# std::locale::operator()

```cpp
template< class CharT, class Traits, class Alloc >
bool operator()( const basic_string<CharT,Traits,Alloc>& s1,
                 const basic_string<CharT,Traits,Alloc>& s2 ) const;
```

Compares two string arguments `s1` and `s2` according to the lexicographic
comparison rules defined by this locale's `std::collate<CharT>` facet. This
operator allows any locale object that has a collate facet to be used as a
binary predicate in the standard algorithms (such as `std::sort`) and ordered
containers (such as `std::set`).

### Parameters

- **s1** — the first string to compare
- **s2** — the second string to compare

### Return value

`true` if `s1` is lexicographically less than `s2`, `false` otherwise.

### Possible implementation

```cpp
template<class CharT, class Traits, class Alloc>
bool operator()(const std::basic_string<CharT,Traits,Alloc>& s1,
                const std::basic_string<CharT,Traits,Alloc>& s2) const;
{
    return std::use_facet<std::collate<CharT>>(*this).compare(
               s1.data(), s1.data() + s1.size(),
               s2.data(), s2.data() + s2.size()) < 0;
}
```

### Example

A `vector` of `string`s can be sorted according to a non-default locale by using
the locale object as comparator:

```cpp
#include <algorithm>
#include <cassert>
#include <locale>
#include <string>
#include <vector>

int main()
{
    std::vector<std::wstring> v = {L"жил", L"был", L"пёс"};
    std::sort(v.begin(), v.end(), std::locale("ru_RU.UTF8"));
    assert(v[0] == L"был");
    assert(v[1] == L"жил");
    assert(v[2] == L"пёс");
}
```

### See also

- **collate** — defines lexicographical comparison and hashing of strings (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/locale/locale/operator()*
