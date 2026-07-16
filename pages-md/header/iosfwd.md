# Standard library header <iosfwd>

This header contains forward declarations for the Input/output library.

**Forward declarations**

- **`std::char_traits`** — Class Template which describes properties of a
  character type (class template)
- **`std::char_traits<char>`** — (class template specialization)
- **`std::char_traits<wchar_t>`** — (class template specialization)
- **`std::char_traits<char8_t>` (C++20)** — (class template specialization)
- **`std::char_traits<char16_t>` (C++11)** — (class template specialization)
- **`std::char_traits<char32_t>` (C++11)** — (class template specialization)
- **allocator** — the default allocator (class template)
- **basic_ios** — manages an arbitrary stream buffer (class template)
- **fpos** — represents absolute position in a stream or a file (class template)
- **basic_streambuf** — abstracts a raw device (class template)
- **basic_ostream** — wraps a given abstract device (`std::basic_streambuf`) and
  provides high-level output interface (class template)
- **basic_istream** — wraps a given abstract device (`std::basic_streambuf`) and
  provides high-level input interface (class template)
- **basic_iostream** — wraps a given abstract device (`std::basic_streambuf`)
  and provides high-level input/output interface (class template)
- **basic_filebuf** — implements raw file device (class template)
- **basic_ifstream** — implements high-level file stream input operations (class
  template)
- **basic_ofstream** — implements high-level file stream output operations
  (class template)
- **basic_fstream** — implements high-level file stream input/output operations
  (class template)
- **basic_stringbuf** — implements raw string device (class template)
- **basic_istringstream** — implements high-level string stream input operations
  (class template)
- **basic_ostringstream** — implements high-level string stream output
  operations (class template)
- **basic_stringstream** — implements high-level string stream input/output
  operations (class template)
- **basic_syncbuf (C++20)** — synchronized output device wrapper (class
  template)
- **basic_osyncstream (C++20)** — synchronized output stream wrapper (class
  template)
- **basic_spanbuf (C++23)** — implements raw fixed character buffer device
  (class template)
- **basic_ispanstream (C++23)** — implements fixed character buffer input
  operations (class template)
- **basic_ospanstream (C++23)** — implements fixed character buffer output
  operations (class template)
- **basic_spanstream (C++23)** — implements fixed character buffer input/output
  operations (class template)
- **strstreambuf (deprecated in C++98)** — implements raw character array device
  (class)
- **istrstream (deprecated in C++98)** — implements character array input
  operations (class)
- **ostrstream (deprecated in C++98)** — implements character array output
  operations (class)
- **strstream (deprecated in C++98)** — implements character array input/output
  operations (class)

**Typedefs and specializations**

- **`std::streampos`** — `std::fpos<std::char_traits<char>::state_type>`
- **`std::wstreampos`** — `std::fpos<std::char_traits<wchar_t>::state_type>`
- **`std::u8streampos`** — `std::fpos<std::char_traits<char8_t>::state_type>`
- **`std::u16streampos`** — `std::fpos<std::char_traits<char16_t>::state_type>`
- **`std::u32streampos`** — `std::fpos<std::char_traits<char32_t>::state_type>`

**In addition, for each class template `std::basic_T` declared in this header, `std::T` and `std::wT` are declared as a synonym of `std::basic_T<char>` and `std::basic_T<wchar_t>` respectively.**

### Synopsis

```cpp
namespace std {
  template<class CharT> struct char_traits;
  template<> struct char_traits<char>;
  template<> struct char_traits<char8_t>;
  template<> struct char_traits<char16_t>;
  template<> struct char_traits<char32_t>;
  template<> struct char_traits<wchar_t>;

  template<class T> class allocator;

  template<class CharT, class Traits = char_traits<CharT>>
    class basic_ios;
  template<class CharT, class Traits = char_traits<CharT>>
    class basic_streambuf;
  template<class CharT, class Traits = char_traits<CharT>>
    class basic_istream;
  template<class CharT, class Traits = char_traits<CharT>>
    class basic_ostream;
  template<class CharT, class Traits = char_traits<CharT>>
    class basic_iostream;

  template<class CharT, class Traits = char_traits<CharT>,
           class Allocator = allocator<CharT>>
    class basic_stringbuf;
  template<class CharT, class Traits = char_traits<CharT>,
           class Allocator = allocator<CharT>>
    class basic_istringstream;
  template<class CharT, class Traits = char_traits<CharT>,
           class Allocator = allocator<CharT>>
    class basic_ostringstream;
  template<class CharT, class Traits = char_traits<CharT>,
           class Allocator = allocator<CharT>>
    class basic_stringstream;

  template<class CharT, class Traits = char_traits<CharT>>
    class basic_filebuf;
  template<class CharT, class Traits = char_traits<CharT>>
    class basic_ifstream;
  template<class CharT, class Traits = char_traits<CharT>>
    class basic_ofstream;
  template<class CharT, class Traits = char_traits<CharT>>
    class basic_fstream;

  template<class CharT, class Traits = char_traits<CharT>,
           class Allocator = allocator<CharT>>
    class basic_syncbuf;
  template<class CharT, class Traits = char_traits<CharT>,
           class Allocator = allocator<CharT>>
    class basic_osyncstream;

  template<class CharT, class Traits = char_traits<CharT>>
    class basic_spanbuf;
  template<class CharT, class Traits = char_traits<CharT>>
    class basic_ispanstream;
  template<class CharT, class Traits = char_traits<CharT>>
    class basic_ospanstream;
  template<class CharT, class Traits = char_traits<CharT>>
    class basic_spanstream;

  template<class CharT, class Traits = char_traits<CharT>>
    class istreambuf_iterator;
  template<class CharT, class Traits = char_traits<CharT>>
    class ostreambuf_iterator;

  using ios  = basic_ios<char>;
  using wios = basic_ios<wchar_t>;

  using streambuf = basic_streambuf<char>;
  using istream   = basic_istream<char>;
  using ostream   = basic_ostream<char>;
  using iostream  = basic_iostream<char>;

  using stringbuf     = basic_stringbuf<char>;
  using istringstream = basic_istringstream<char>;
  using ostringstream = basic_ostringstream<char>;
  using stringstream  = basic_stringstream<char>;

  using filebuf  = basic_filebuf<char>;
  using ifstream = basic_ifstream<char>;
  using ofstream = basic_ofstream<char>;
  using fstream  = basic_fstream<char>;

  using syncbuf     = basic_syncbuf<char>;
  using osyncstream = basic_osyncstream<char>;

  using spanbuf     = basic_spanbuf<char>;
  using ispanstream = basic_ispanstream<char>;
  using ospanstream = basic_ospanstream<char>;
  using spanstream  = basic_spanstream<char>;

  using wstreambuf = basic_streambuf<wchar_t>;
  using wistream   = basic_istream<wchar_t>;
  using wostream   = basic_ostream<wchar_t>;
  using wiostream  = basic_iostream<wchar_t>;

  using wstringbuf     = basic_stringbuf<wchar_t>;
  using wistringstream = basic_istringstream<wchar_t>;
  using wostringstream = basic_ostringstream<wchar_t>;
  using wstringstream  = basic_stringstream<wchar_t>;

  using wfilebuf  = basic_filebuf<wchar_t>;
  using wifstream = basic_ifstream<wchar_t>;
  using wofstream = basic_ofstream<wchar_t>;
  using wfstream  = basic_fstream<wchar_t>;

  using wsyncbuf     = basic_syncbuf<wchar_t>;
  using wosyncstream = basic_osyncstream<wchar_t>;

  using wspanbuf     = basic_spanbuf<wchar_t>;
  using wispanstream = basic_ispanstream<wchar_t>;
  using wospanstream = basic_ospanstream<wchar_t>;
  using wspanstream  = basic_spanstream<wchar_t>;

  template<class State> class fpos;
  using streampos = fpos<char_traits<char>::state_type>;
  using wstreampos = fpos<char_traits<wchar_t>::state_type>;
  using u8streampos = fpos<char_traits<char8_t>::state_type>;
  using u16streampos = fpos<char_traits<char16_t>::state_type>;
  using u32streampos = fpos<char_traits<char32_t>::state_type>;
}

// deprecated
namespace std {
  class strstreambuf;
  class istrstream;
  class ostrstream;
  class strstream;
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/iosfwd*
