# Standard library header <streambuf>

This header is part of the Input/Output library.

**Classes**

- **basic_streambuf** — abstracts a raw device (class template)
- **`streambuf`** — `std::basic_streambuf<char>` (typedef)
- **`wstreambuf`** — `std::basic_streambuf<wchar_t>` (typedef)

### Synopsis

```cpp
namespace std {
  template<class CharT, class Traits = char_traits<CharT>>
    class basic_streambuf;
  using streambuf  = basic_streambuf<char>;
  using wstreambuf = basic_streambuf<wchar_t>;
}
```

#### Class template `std::basic_streambuf`

```cpp
namespace std {
  template<class CharT, class Traits = char_traits<CharT>>
  class basic_streambuf {
  public:
    using char_type   = CharT;
    using int_type    = typename Traits::int_type;
    using pos_type    = typename Traits::pos_type;
    using off_type    = typename Traits::off_type;
    using traits_type = Traits;

    virtual ~basic_streambuf();

    // locales
    locale   pubimbue(const locale& loc);
    locale   getloc() const;

    // buffer and positioning
    basic_streambuf* pubsetbuf(char_type* s, streamsize n);
    pos_type pubseekoff(off_type off, ios_base::seekdir way,
                        ios_base::openmode which
                          = ios_base::in | ios_base::out);
    pos_type pubseekpos(pos_type sp,
                        ios_base::openmode which
                          = ios_base::in | ios_base::out);
    int      pubsync();

    // get and put areas
    // get area
    streamsize in_avail();
    int_type snextc();
    int_type sbumpc();
    int_type sgetc();
    streamsize sgetn(char_type* s, streamsize n);

    // putback
    int_type sputbackc(char_type c);
    int_type sungetc();

    // put area
    int_type   sputc(char_type c);
    streamsize sputn(const char_type* s, streamsize n);

  protected:
    basic_streambuf();
    basic_streambuf(const basic_streambuf& rhs);
    basic_streambuf& operator=(const basic_streambuf& rhs);

    void swap(basic_streambuf& rhs);

    // get area access
    char_type* eback() const;
    char_type* gptr()  const;
    char_type* egptr() const;
    void       gbump(int n);
    void       setg(char_type* gbeg, char_type* gnext, char_type* gend);

    // put area access
    char_type* pbase() const;
    char_type* pptr() const;
    char_type* epptr() const;
    void       pbump(int n);
    void       setp(char_type* pbeg, char_type* pend);

    // virtual functions
    // locales
    virtual void imbue(const locale& loc);

    // buffer management and positioning
    virtual basic_streambuf* setbuf(char_type* s, streamsize n);
    virtual pos_type seekoff(off_type off, ios_base::seekdir way,
                             ios_base::openmode which
                               = ios_base::in | ios_base::out);
    virtual pos_type seekpos(pos_type sp,
                             ios_base::openmode which
                               = ios_base::in | ios_base::out);
    virtual int      sync();

    // get area
    virtual streamsize showmanyc();
    virtual streamsize xsgetn(char_type* s, streamsize n);
    virtual int_type   underflow();
    virtual int_type   uflow();

    // putback
    virtual int_type   pbackfail(int_type c = Traits::eof());

    // put area
    virtual streamsize xsputn(const char_type* s, streamsize n);
    virtual int_type   overflow(int_type c = Traits::eof());
  };
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 56 | C++98 | the return type of `showmanyc` of was `int` in the synopsis |
      corrected to `streamsize`

---
*Source: https://en.cppreference.com/w/cpp/header/streambuf*
