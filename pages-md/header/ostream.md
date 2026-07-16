# Standard library header <ostream>

This header is part of the Input/output library.

**Classes**

- **basic_ostream** — wraps a given abstract device (`std::basic_streambuf`) and
  provides high-level output interface (class template)
- **`std::ostream`** — `std::basic_ostream<char>` (typedef)
- **`std::wostream`** — `std::basic_ostream<wchar_t>` (typedef)

**Functions**

- **operator<<(std::basic_ostream)** — inserts character data or insert into
  rvalue stream (function template)
- **print(std::ostream) (C++23)** — outputs formatted representation of the
  arguments (function template)
- **println(std::ostream) (C++23)** — outputs formatted representation of the
  arguments with appended `'\n'` (function template)
- **vprint_unicode(std::ostream) (C++23)** — performs Unicode aware output using
  type-erased argument representation (function)
- **vprint_nonunicode(std::ostream) (C++23)** — outputs character data using
  type-erased argument representation (function)

**Manipulators**

- **endl** — outputs `'\n'` and flushes the output stream (function template)
- **ends** — outputs `'\0'` (function template)
- **flush** — flushes the output stream (function template)
- **emit_on_flushnoemit_on_flush (C++20)** — controls whether a stream's
  `basic_syncbuf` emits on flush (function template)
- **flush_emit (C++20)** — flushes a stream and emits the content if it is using
  a `basic_syncbuf` (function template)

### Synopsis

```cpp
namespace std {
  template<class CharT, class Traits = char_traits<CharT>>
    class basic_ostream;

  using ostream  = basic_ostream<char>;
  using wostream = basic_ostream<wchar_t>;

  template<class CharT, class Traits>
    basic_ostream<CharT, Traits>& endl(basic_ostream<CharT, Traits>& os);
  template<class CharT, class Traits>
    basic_ostream<CharT, Traits>& ends(basic_ostream<CharT, Traits>& os);
  template<class CharT, class Traits>
    basic_ostream<CharT, Traits>& flush(basic_ostream<CharT, Traits>& os);

  template<class CharT, class Traits>
    basic_ostream<CharT, Traits>& emit_on_flush(basic_ostream<CharT, Traits>& os);
  template<class CharT, class Traits>
    basic_ostream<CharT, Traits>& noemit_on_flush(basic_ostream<CharT, Traits>& os);
  template<class CharT, class Traits>
    basic_ostream<CharT, Traits>& flush_emit(basic_ostream<CharT, Traits>& os);

  template<class Ostream, class T>
    Ostream&& operator<<(Ostream&& os, const T& x);

  // print functions
  template<class... Args>
    void print(ostream& os, format_string<Args...> fmt, Args&&... args);
  template<class... Args>
    void println(ostream& os, format_string<Args...> fmt, Args&&... args);

  void vprint_unicode(ostream& os, string_view fmt, format_args args);
  void vprint_nonunicode(ostream& os, string_view fmt, format_args args);
}
```

#### Class template `std::basic_ostream`

```cpp
namespace std {
  template<class CharT, class Traits = char_traits<CharT>>
  class basic_ostream : virtual public basic_ios<CharT, Traits> {
  public:
    // types (inherited from basic_ios)
    using char_type   = CharT;
    using int_type    = typename Traits::int_type;
    using pos_type    = typename Traits::pos_type;
    using off_type    = typename Traits::off_type;
    using traits_type = Traits;

    // constructor/destructor
    explicit basic_ostream(basic_streambuf<char_type, Traits>* sb);
    virtual ~basic_ostream();

    // prefix/suffix
    class sentry;

    // formatted output
    basic_ostream& operator<<(basic_ostream& (*pf)(basic_ostream&));
    basic_ostream& operator<<(basic_ios<CharT, Traits>& (*pf)(basic_ios<CharT, Traits>&));
    basic_ostream& operator<<(ios_base& (*pf)(ios_base&));

    basic_ostream& operator<<(bool n);
    basic_ostream& operator<<(short n);
    basic_ostream& operator<<(unsigned short n);
    basic_ostream& operator<<(int n);
    basic_ostream& operator<<(unsigned int n);
    basic_ostream& operator<<(long n);
    basic_ostream& operator<<(unsigned long n);
    basic_ostream& operator<<(long long n);
    basic_ostream& operator<<(unsigned long long n);
    basic_ostream& operator<<(float f);
    basic_ostream& operator<<(double f);
    basic_ostream& operator<<(long double f);
    basic_ostream& operator<<(/*extended-floating-point-type*/ f);

    basic_ostream& operator<<(const void* p);
    basic_ostream& operator<<(const volatile void* p);
    basic_ostream& operator<<(nullptr_t);
    basic_ostream& operator<<(basic_streambuf<char_type, Traits>* sb);

    // unformatted output
    basic_ostream& put(char_type c);
    basic_ostream& write(const char_type* s, streamsize n);

    basic_ostream& flush();

    // seeks
    pos_type tellp();
    basic_ostream& seekp(pos_type);
    basic_ostream& seekp(off_type, ios_base::seekdir);

  protected:
    // copy/move constructor
    basic_ostream(const basic_ostream&) = delete;
    basic_ostream(basic_ostream&& rhs);

    // assignment and swap
    basic_ostream& operator=(const basic_ostream&) = delete;
    basic_ostream& operator=(basic_ostream&& rhs);
    void swap(basic_ostream& rhs);
  };

  // character inserters
  template<class CharT, class Traits>
    basic_ostream<CharT, Traits>& operator<<(basic_ostream<CharT, Traits>&, CharT);
  template<class CharT, class Traits>
    basic_ostream<CharT, Traits>& operator<<(basic_ostream<CharT, Traits>&, char);
  template<class Traits>
    basic_ostream<char, Traits>& operator<<(basic_ostream<char, Traits>&, char);

  template<class Traits>
    basic_ostream<char, Traits>& operator<<(basic_ostream<char, Traits>&, signed char);
  template<class Traits>
    basic_ostream<char, Traits>& operator<<(basic_ostream<char, Traits>&, unsigned char);

  template<class Traits>
    basic_ostream<char, Traits>& operator<<(basic_ostream<char, Traits>&,
                                            wchar_t) = delete;
  template<class Traits>
    basic_ostream<char, Traits>& operator<<(basic_ostream<char, Traits>&,
                                            char8_t) = delete;
  template<class Traits>
    basic_ostream<char, Traits>& operator<<(basic_ostream<char, Traits>&,
                                            char16_t) = delete;
  template<class Traits>
    basic_ostream<char, Traits>& operator<<(basic_ostream<char, Traits>&,
                                            char32_t) = delete;
  template<class Traits>
    basic_ostream<wchar_t, Traits>&
      operator<<(basic_ostream<wchar_t, Traits>&, char8_t) = delete;
  template<class Traits>
    basic_ostream<wchar_t, Traits>&
      operator<<(basic_ostream<wchar_t, Traits>&, char16_t) = delete;
  template<class Traits>
    basic_ostream<wchar_t, Traits>&
      operator<<(basic_ostream<wchar_t, Traits>&, char32_t) = delete;

  template<class CharT, class Traits>
    basic_ostream<CharT, Traits>& operator<<(basic_ostream<CharT, Traits>&, const CharT*);
  template<class CharT, class Traits>
    basic_ostream<CharT, Traits>& operator<<(basic_ostream<CharT, Traits>&, const char*);
  template<class Traits>
    basic_ostream<char, Traits>& operator<<(basic_ostream<char, Traits>&, const char*);

  template<class Traits>
    basic_ostream<char, Traits>& operator<<(basic_ostream<char, Traits>&,
                                            const signed char*);
  template<class Traits>
    basic_ostream<char, Traits>& operator<<(basic_ostream<char, Traits>&,
                                            const unsigned char*);

  template<class Traits>
    basic_ostream<char, Traits>&
      operator<<(basic_ostream<char, Traits>&, const wchar_t*) = delete;
  template<class Traits>
    basic_ostream<char, Traits>&
      operator<<(basic_ostream<char, Traits>&, const char8_t*) = delete;
  template<class Traits>
    basic_ostream<char, Traits>&
      operator<<(basic_ostream<char, Traits>&, const char16_t*) = delete;
  template<class Traits>
    basic_ostream<char, Traits>&
      operator<<(basic_ostream<char, Traits>&, const char32_t*) = delete;
  template<class Traits>
    basic_ostream<wchar_t, Traits>&
      operator<<(basic_ostream<wchar_t, Traits>&, const char8_t*) = delete;
  template<class Traits>
    basic_ostream<wchar_t, Traits>&
      operator<<(basic_ostream<wchar_t, Traits>&, const char16_t*) = delete;
  template<class Traits>
    basic_ostream<wchar_t, Traits>&
      operator<<(basic_ostream<wchar_t, Traits>&, const char32_t*) = delete;
}
```

#### Class `std::basic_ostream::sentry`

```cpp
namespace std {
  template<class CharT, class Traits>
  class basic_ostream<CharT, Traits>::sentry {
    bool ok_;       // exposition only
  public:
    explicit sentry(basic_ostream& os);
    ~sentry();
    explicit operator bool() const { return ok_; }

    sentry(const sentry&) = delete;
    sentry& operator=(const sentry&) = delete;
  };
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/ostream*
