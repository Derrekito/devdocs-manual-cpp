# std::formattable

```cpp
template< class T, class CharT >
concept formattable = /* formattable_with */<
  std::remove_reference_t<T>,
  std::basic_format_context</* fmt_iter_for */<CharT>, CharT>
>;  // (1) (since C++23)
Helper templates
template< class CharT >
using /* fmt_iter_for */ = /* unspecified */;  // (2) (exposition only*)
template< class T, class Context,
          class Formatter =
              typename Context::template
                  formatter_type<std::remove_const_t<T>> >
concept /* formattable_with */ =
  std::semiregular<Formatter> &&
  requires (Formatter& f, const Formatter& cf, T&& t, Context fc,
            std::basic_format_parse_context<
                typename Context::char_type
            > pc) {
    { f.parse(pc) } -> std::same_as<typename decltype(pc)::iterator>;
    { cf.format(t, fc) } -> std::same_as<typename Context::iterator>;
  };  // (3) (exposition only*)
```

The concept `formattable` specifies that std::formatter<std::remove_cvref_t<T>,
CharT> meets the requirements of BasicFormatter and Formatter (if
std::remove_reference_t<T> is const-qualified).

The exposition-only alias template /* fmt_iter_for */ yields an unspecified type
that satisfies std::output_iterator<const CharT&>.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3925 | C++23 | the second template argument of `std::basic_format_context`
      was not provided | provided

### See also

- **formatter (C++20)** — defines formatting rules for a given type (class
  template)
- **BasicFormatter (C++20)** — abstracts formatting operations for a given
  formatting argument type and character type (named requirement)
- **Formatter (C++20)** — defines functions used by the formatting library
  (named requirement)

---
*Source: https://en.cppreference.com/w/cpp/utility/format/formattable*
